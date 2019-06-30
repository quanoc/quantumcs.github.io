yout:     post
title:      "原来你是这样的Stream"
date:       2018-10-11
catalog:    true
author:     "Nova"
avatar: /images/favicon.ico
categories: Java
tags:
    - Java
description: 浅析 Java Stream 实现原理
---

Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。

Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象。

Stream API可以极大提高Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。

元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。

<!--more-->

先来看一道题目.

## 一.题目

从一列单词中选出字母a开头的单词,按字母排序后返回前3个.


> 比如 are,where,advance, anvato, java, abc

### 外部迭代方式
```java
List<String> list = Lists.newArrayList("are", "where", "advance", "java", "abc");
List<String> tempList = Lists.newArrayList();
List<String> result = List.newArrayList();
for (int i=0; i<list.size();i++){
	if(list.get(i).startsWith("a")){
		tempList.add(list.get(i));
	}
}
tempList.sort(Comparator.naturalOrder());
result = tempList.subList(0,3);
result.forEach(System.out::println);
```

### stream实现方式
```java
List<String> list = Lists.newArrayList("are", "where", "advance", "java", "abc");
list.stream().filter(s -> s.startsWith("a")).sorted().limit(3)
	.collect(Collectors.toList()).forEach(System.out::println)

```


## 二.解读
对于遍历一个集合,传统的做法大概是用Iterator,或者for循环.这两种方式都属于外部迭代.

### 外部迭代:

外部迭代存在的问题:

- 迭代的逻辑需要开发者自己手写
- 如果存在排序这样的有状态的中间操作,不得不进行多次迭代
- 多次迭代增加了临时变量,导致内存的浪费


Java5引入的foreach解决了部分问题,但新问题:

- foreach遍历不能对元素进行赋值操作
- 遍历的时候,只有当前被遍历元素可见,其他不可见

### Stream:

随着大数据兴起,就需要更高效便捷的遍历方式.来对数据进行遍历.那就是stream出现了.

它的优点:

- 像流水线一样来处理数据
- 兼容常用的集合
- 高效的可读性
- 提供对数据集合的常规操作
- 可以拼装不同的操作

另外stream有lambda表达式的加成,更是如虎添翼.


那么问题来了.简介的lambda有很好的可读性,但是Stream是怎么实现的呢,它的效率提升在哪里.

流水线.如何定义流水线?原料如何流入?如何让流水线上的工人将处理过的原料交给下一个工人?流水线何时开始,何时结束运行?

> Stream处理数据的过程可以类别成工厂的流水线,数据可以看做流水线上的原料,对数据的操作可以看做流水线上的工人对原料的操作.



Stream事实上是一个接口,并没有操作的缺省实现.最主要的实现是ReferencePipeline,而ReferencePipeline继承自AbstractPipeline,AbstractPipeline实现了BaseStream接口并实现了它的方法.
但是ReferencePipeline任然是一个抽象类.因为它并没有实现所有的抽象方法,比如AbstractPipeline中的opWrapSink.

ReferencePipeline内部定义三个静态内部类,分别是:Head,StatelessOp,StatefulOp,但只有Head不再是抽象类.

![](Stream.png)

流水线的结构有点像双向链表,节点之间通过引用连接,节点可以分为三类,控制数据输入的节点,操作数据的中间节点和控制数据输出的节点.

ReferencePipeline包含了控制数据流入的Head,中间操作StatelessOp,StatefulOp,终止操作哦TerminalOp.

#### Stream常用的操作包括:

##### 中间操作(Intermedate Operations)

- 无状态(Stateless)操作

每个数据处理是独立的,不会影响或者依赖之前的数据.如:

filter(), flatMap(), flatMapToDouble(), flatMapToInt(), flatMapToLang(), map(), mapToDouble(), peek(), unordered() 等;

- 有状态(Stateful)操作

处理时会记录状态,比如处理了几个, 后面元素的处理会依赖前面记录的状态, 或者拿到所有元素才能继续下去.如:

distinct(), sorted(), sorted(comparator), limit(), skip()等

##### 终止操作(Terminal Operations)

- 非短路操作

处理完所有数据才能得到结果,如:

collect(), count(), forEach(), forEachOrdered(), max(), min(), reduce(), toArray()

- 短路操作

拿到符合预期的结果就会停下来, 不一定会处理完所有数据,如

anyMatch(), allMatch(), noneMatch(), findFirst(), findAny()等.





[转:浅析 Java Stream 实现原理](https://mp.weixin.qq.com/s/BMvvTBkDHIyFFX6HQ4tNDA)


