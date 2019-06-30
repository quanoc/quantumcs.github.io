---
title: 神奇的Flink
date: 2018-11-03 20:59:00
author: Nova
catalog:    true
avatar: /images/favicon.ico
tags: Flink
categories: 后台
thumbnail:
blogexcerpt:
description: Flink入门篇

---

[What is Apache Flink?](https://flink.apache.org/flink-architecture.html)

- 一个框架
- 数据流状态计算
- 分布式处理引擎

Flink可以处理有界和无界的数据流, 应用场景有日志分析,用户行为,信用卡交易,传感器测量,所有这些数据都是作为流生成的.

## 简单的case
来看一个Sliding Window的需求:每隔5分钟输出过去一小时内点击量最多的前 N 个商品.

关键词: 1小时窗口, 5分钟滑动一次, 点击最多, 前N个

<!--more-->

- 数据的模拟
流式应用应该是一个一直运行着的程序，需要消费一个无限数据源. 但可以通过文件来模拟真实的数据源.

文件中每一行是一个用户行为.创建行为的POJO.

```java
/** 用户行为数据结构 **/
public static class UserBehavior {
  public long userId;         // 用户ID
  public long itemId;         // 商品ID
  public int categoryId;      // 商品类目ID
  public String behavior;     // 用户行为, 包括("pv", "buy", "cart", "fav")
  public long timestamp;      // 行为发生的时间戳，单位秒
}
```


- 实现计算

数据读取的逻辑在这里省略了,主要看下Flink来实现窗口计算.

> PS: 事件被处理的时间, 事件发生的时间.根据需求不同需要对行为加上时间来用于计算.

首先过滤出点击事件:

```java
DataStream<UserBehavior> pvData = timedData
    .filter(new FilterFunction<UserBehavior>() {
      @Override
      public boolean filter(UserBehavior userBehavior) throws Exception {
        // 过滤出只有点击的数据
        return userBehavior.behavior.equals("pv");
      }
    });
```

统计点击量:


```java
DataStream<ItemViewCount> windowedData = pvData
    .keyBy("itemId")
    .timeWindow(Time.minutes(60), Time.minutes(5))
    .aggregate(new CountAgg(), new WindowResultFunction());
```

我们使用.keyBy("itemId")对商品进行分组，使用.timeWindow(Time size, Time slide)对每个商品做滑动窗口（1小时窗口，5分钟滑动一次）。然后我们使用 .aggregate(AggregateFunction af, WindowFunction wf) 做增量的聚合操作，它能使用AggregateFunction提前聚合掉数据，减少 state 的存储压力。较之.apply(WindowFunction wf)会将窗口中的数据都存储下来，最后一起计算要高效地多。aggregate()方法的第一个参数用于这里的CountAgg实现了AggregateFunction接口，功能是统计窗口中的条数，即遇到一条数据就加一。

```java
/** COUNT 统计的聚合函数实现，每出现一条记录加一 */
public static class CountAgg implements AggregateFunction<UserBehavior, Long, Long> {
  @Override
  public Long createAccumulator() {
    return 0L;
  }
  @Override
  public Long add(UserBehavior userBehavior, Long acc) {
    return acc + 1;
  }
  @Override
  public Long getResult(Long acc) {
    return acc;
  }
  @Override
  public Long merge(Long acc1, Long acc2) {
    return acc1 + acc2;
  }
}
```

.aggregate(AggregateFunction af, WindowFunction wf) 的第二个参数WindowFunction将每个 key每个窗口聚合后的结果带上其他信息进行输出。我们这里实现的WindowResultFunction将主键商品ID，窗口，点击量封装成了ItemViewCount进行输出。

```java
/** 用于输出窗口的结果 */
public static class WindowResultFunction implements WindowFunction<Long, ItemViewCount, Tuple, TimeWindow> {
  @Override
  public void apply(
      Tuple key,  // 窗口的主键，即 itemId
      TimeWindow window,  // 窗口
      Iterable<Long> aggregateResult, // 聚合函数的结果，即 count 值
      Collector<ItemViewCount> collector  // 输出类型为 ItemViewCount
  ) throws Exception {
    Long itemId = ((Tuple1<Long>) key).f0;
    Long count = aggregateResult.iterator().next();
    collector.collect(ItemViewCount.of(itemId, window.getEnd(), count));
  }
}
/** 商品点击量(窗口操作的输出类型) */
public static class ItemViewCount {
  public long itemId;     // 商品ID
  public long windowEnd;  // 窗口结束时间戳
  public long viewCount;  // 商品的点击量
  public static ItemViewCount of(long itemId, long windowEnd, long viewCount) {
    ItemViewCount result = new ItemViewCount();
    result.itemId = itemId;
    result.windowEnd = windowEnd;
    result.viewCount = viewCount;
    return result;
  }
}
```

TopN 计算最热门商品:

```java
DataStream<String> topItems = windowedData
    .keyBy("windowEnd")
    .process(new TopNHotItems(3));  // 求点击量前3名的商品
```
