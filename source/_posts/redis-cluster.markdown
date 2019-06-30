---
layout:     post
title:      "Redis集群方案"
date:       2017-03-22
author:     "Nova"
header-img: "post-bg-js-version.jpg"
avatar: /images/favicon.ico
categories: 后台
tags:
    - redis
---


### 客户端分片
客户端分片是把分片的逻辑放在Redis客户端实现，通过Redis客户端预先定义好的路由规则，把对Key的访问转发到不同的Redis实例中，最后把返回结果汇集。


* 优缺点

好处是所有的逻辑都是可控的，不依赖与第三方分布式中间件。开发清楚怎么实现分片、路由的规则，不用担心踩坑。

缺点是，这是一种静态的分片方案，需要增加或者减少redis实例的数量时，需要手工调整分片的程序。并且可运维性差，集群的数据出了任何问题都需要运维人员和开发人员一起合作，减缓了解决问题的速度，增加了跨部门沟通的成本。

因此这种部署方案基本没有使用

### Twemproxy
Twemproxy是由Twitter开源的Redis代理，其基本原理是：Redis客户端把请求发送到Twemproxy，Twemproxy根据路由规则发送到正确的Redis实例，最后Twemproxy把结果汇集返回给客户端。

Twemproxy通过引入一个代理层，将多个Redis实例进行统一管理，使Redis客户端只需要在Twemproxy上进行操作，而不需要关心后面有多少个Redis实例，从而实现了Redis集群。

* 优缺点

客户端像连接Redis实例一样连接Twemproxy，不需要改任何的代码逻辑。

由于Redis客户端的每个请求都经过Twemproxy代理才能到达Redis服务器，这个过程中会产生性能损失。
没有友好的监控管理后台界面，不利于运维监控。

最大的问题是Twemproxy无法平滑地增加Redis实例。对于运维人员来说，当因为业务需要增加Redis实例时工作量非常大。

Twemproxy作为最被广泛使用、最久经考验、稳定性最高的Redis代理，在业界被广泛使用。

### codis
Twemproxy不能平滑增加Redis实例的问题带来了很大的不便，于是豌豆荚自主研发了Codis，一个支持平滑增加Redis实例的Redis代理软件，其基于Go和C语言开发，并于2014年11月在GitHub上开源。

Codis包含下面4个部分。具体见[Redis 集群方案](http://www.sohu.com/a/79200151_354963)


### Redis 3.0集群
Redis 3.0集群采用了P2P的模式，完全去中心化。Redis把所有的Key分成了16384个slot，每个Redis实例负责其中一部分slot。集群中的所有信息（节点、端口、slot等），都通过节点之间定期的数据交换而更新。

> Redis客户端在任意一个Redis实例发出请求，如果所需数据不在该实例中，通过重定向命令引导客户端访问所需的实例。

一个Redis实例具备了“数据存储”和“路由重定向”，完全去中心化的设计。这带来的好处是部署非常简单，直接部署Redis就行，不像Codis有那么多的组件和依赖。但带来的问题是很难对业务进行无痛的升级，如果哪天Redis集群出了什么严重的Bug，就只能回滚整个Redis集群。

[Redis Cluster（Redis 3.X）设计要点](http://blog.csdn.net/yfkiss/article/details/39996129)





参考：

* [Redis 核心概念](http://blog.jobbole.com/112024/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
* [Redis 集群方案](http://www.sohu.com/a/79200151_354963)
* [基于 Redis 实现分布式应用限流](http://blog.jobbole.com/112381/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
* [redis节点管理-新增主节点](https://www.cnblogs.com/shihaiming/p/5984010.html)
