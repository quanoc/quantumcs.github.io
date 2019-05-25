---
layout:     post
title:      "ElasticSearch学习指南"
subtitle:   "Elasticsearch Learning Guide"
date:       2017-11-24
catalog:    true
author:     "Nova"
header-img: "post-bg-es-learn.jpg"
tags:
    - Elasticsearch
    - 分词
---


Es是目前主流的开源搜索引擎之一，它的用途还是很广泛的。我接触Es是在想实践下Skyeye（分布式应用监控及日志采集项目）的时候。项目中使用Es索引日志，提供可视化的日志搜索功能以及生产日志实时滚动。

Es不仅用于日志索引、日志分析，还一般用于商品网站的商品搜索以及其它应用级别的搜索功能实现等等。那它为什么有这么强大的功能的，它和数据库相比有哪些优势，又有哪些劣势。我们来认识一下这个主流的搜索引擎组件。

### Es认识
Es是什么，ElasticSearch是一个基于Lucence构建的开源、分布式、RESTful接口全文搜索引擎。ES还可以是一个分布式文档数据库，其中每个字段均是被索引的数据且可被搜索，它能够扩展至数以百计的服务器存储以及处理PB级的数据。它可以在很短的时间内存储、搜索和分析大量的数据。通常作为具有复杂搜索场景情况下的核心发动机。

总结下来其具有以下特点：

- 全文搜索引擎（基于Lucence构建、提供RESTful接口）
- 面向文档数据库（字段均是被索引的数据、字段可被搜索）
- 开源分布式
- 扩展性极强
- 应用于复杂搜索场景

除了以上基础的特性外，还有的场景可以是使用Es存储数据，然后使用配件Kibana建立自定义的仪表盘或者使用Python等语言开发出优美的展示界面，实现数据分析。还可以使用Es的聚合功能来执行复杂的商业智能与数据查询。

> PS:Es的出现源自于一个叫Shay的人为了给妻子构建一个食谱搜索引擎，直接使用Lucence比较困难，所以他抽象Lucence代码来实现了一个方便Java程序员可以在应用中添加搜索功能，这个开源项目叫做Compass。后来Shay工作于高性能和内存数据网格的分布式环境下，他重写了Compass库使其成为一个独立的服务叫做ElasticSearch。

正是Es通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单
对Es有了大概的认识之后，再来比较下Es与其他关系型数据库、非关系型数据库的差别。

### Es与DB对比
Es对于关系型数据库最大的优势是具有很好的扩展性，它可以在10亿、100亿的数据量下实现快速搜索，源自于它天生的分布式特性。而对于非关系型数据比如Mongodb的比较，分一下几个点:

- 搜索：Es完胜
- 横向扩展：Es很简单方便，mongo也不错
- 数据聚合操作：待评估
- 事务操作：mongo完胜
- 权限管理：Es不完善，没有授权和认证特性
- 存储灵活性：mongo完胜
- 写操作：mongo完胜

并且Es还有提升优化的方面：

>对于查询单条数据的应用场景来说，我们可以使用ES的路由机制，将同一索引内的具有相同特征（比如具有相同的userid）的文档全部存储于一个节点上，这样我们之后的查询都可以直接定位到这个节点上，而不用将查询广播道所有的节点上；
随着数据节点的增加，适当增加分片数量，提升系统的分布水平，也可以通过分而治之的方式优化查询性能；


还有对于基本的数据结构，可类比于传统关系型数据库：

| RDB           | ElasticSearch | 
| ------------- |:-------------:| 
| 数据库         | 索引           | 
| Table       | Type       |
| Row         | Document   |   
| Column      | Field    |   
| Schema      | Mapping  |
| Index       | Everything is index|
| SQL         | Query DSL |
| SELECT * FROM table...| GET http://...|
| UPDATE table SET ...  | PUT http://...|


- 关系型数据库中的数据库（DataBase），等价于ES中的索引（Index） 
- 一个数据库下面有N张表（Table），等价于1个索引Index下面有N多类型（Type）， 
- 一个数据库表（Table）下的数据由多行（ROW）多列（column，属性）组成，等价于1个Type由多个文档（Document）和多Field组成。 
- 在一个关系型数据库里面，schema定义了表、每个表的字段，还有表和字段之间的关系。 与之对应的，在ES中：Mapping定义索引下的Type的字段处理规则，即索引如何建立、索引类型、是否保存原始索引JSON文档、是否压缩原始JSON文档、是否需要分词处理、如何进行分词处理等。 
- 在数据库中的增insert、删delete、改update、查search操作等价于ES中的增PUT/POST、删Delete、改_update、查GET.


### Es环境搭建
环境搭建见 [看云:Es环境搭建](http://book.quartz.ren/456832)


### Es相关概念
- 集群（Cluster）
- 索引、文档
- 聚合操作
- 分片（Shard）
- 副本（Replia）
- 倒排索引
- 全文搜索


本篇文章定位是一篇入门级别文章。没有深层次的细节，比如索引结构这些在后续整理，同时加一些自己的认识。
此篇只针对es做以入门级别的介绍，让大家认识到Es的优势。同时总结一下自己学习Es的过程及重要点，分享给大家，以便大家了解学习。如有建议，请不吝赐教。

了解了以上内容之后基本上对Es、Es与Db的差别有了认识，需要深入学习Es还要做些什么？

- Es的搜索原理
- 节点发现机制（广播还是单播）
- 索引
- 映射

[Elasticsearch（lucene）可以代替NoSQL（mongodb）吗](https://www.zhihu.com/question/25535889)
[Elasticsearch学习及解决方案](http://blog.csdn.net/laoyang360/article/details/52244917)