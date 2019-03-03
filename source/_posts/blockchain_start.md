---
layout:     post
title:      "区块链简单入门"
subtitle:   "BlockChain"
date:       2018-03-16
catalog:    true
author:     "Nova"
header-img: "bj001.jpg"
tags:
    - BlockChain
---


区块链是最近比较火的一个技术，他的诞生已经有很长一段时间。那区块链到底是什么？他真的很难理解吗？下面，简单的了解以下区块链。

其实，区块链的核心概念也非常简单，没有想象中的难。通过本文，不仅可以理解区块链，还会明白什么是挖矿，为什么挖矿越来越难等问题。

### 区块链本质
区块链什么？它是一种特殊的分布式数据库。

首先，区块链的主要作用是存储信息。任何需要保存的信息，都可以写入区块链，也可以从里面读取，所以它是数据库。

其次，任何人都可以架设服务器，加入区块链网络，成为一个节点。区块链的世界里面，没有中心节点，每个节点都是平等的，都是保存着整个数据库。

你可以向任何一个节点，写入/读取数据，因为所有节点最后都会同步，保证区块链一致。

认识了区块链之后，那可能会有以下问题。

1. 怎么架设服务器，实现加入区块链网络？
2. 写入/读取数据，读取的是什么？
3. 区块链的新区块（数据）怎么同步？


带着以下问题继续深入。

### 区块链最大特点

分布式数据库并不是新发明。但区块链有一个革命性特点：区块链没有管理员，它是彻底无中心的。

其它数据库都有管理员，但是区块链没有。

如果有人想对区块链添加审核，也实现不了。因为它的设计目标就是防止出现居于中心地位的管理当局。

正式因为无法管理，区块链才能做到无法被控制。否则一旦大公司大集团控制了管理权，他们会控制整个平台。

**那么，没有了管理员，怎么保证数据是可信的呢？**被坏人改了怎么办？真正的认识区块之后你就明白了。

### 区块
区块链由一个个区块组成。

>区块很像数据库的记录，每次写入数据，就是创建一个区块。

每个区块包含两部分：


1. 区块头（Head）：记录当前区块的特征值
2. 区块体（Body）:实际数据


区块头包含了当前区块的多项特征值：


1. 生成时间
2. 实际数据（即区块体）的哈希
3. 上一个区块的哈希
4. ......


这里，你需要理解什么叫哈希(hash)。

所谓“哈希”就是计算机对任意内容，计算出一个长度相同的特征值。区块链的哈希长度为256位，就是说，不管原始内容是什么，最后都计算出一个256位的二进制数字。而且保证，只要原始内容不同，对应的哈希一定是不同的。


举例：字符串123的哈希。


因此，这里有２个重要的推论：

```
1. 每个区块的哈希都是不一样的，可以通过哈希标示区块。
2. 如果区块的内容变了，它的哈希一定会改变。
```

### Hash的不可修改性
区块和哈希是一一对应的，每个区块的哈希都是针对“区块头”计算的。也就是说，把区块头的各项特征值，按照顺序连接在一起，组成一个很长的字符串，再对这个字符串计算哈希。

```
Hash = SHA256(区块头)
```

上面就是区块哈希的计算公式，SHA256是区块链的哈希算法。注意，这个公式里面只包含区块头，不包含区块体，也就是说，哈希由区块头唯一决定。



### 采矿

### 难度系数

### 难度系数的动态调节

### 区块链的分叉

六次确认。按照１０分钟一个区块计算，一个小时就可以解决。

由于新区块的生成速度由计算能力决定，所以这条规则就是说，拥有大多数计算能力的那条分支，就是正宗的区块链。

### 总结
区块链作为无人管理的分布式数据库，从２００９年开始已经运行了８年，没有出现大的问题。这证明它是可行的。

但是为了保证数据的可靠性，区块链也有自己的代价。一是效率，数据写入区块链，最少要等待１０分钟，所有节点都同步数据，则需要更多的时间；二是能耗，区块链的生成需要旷工进行无数无意义的计算，这是非常耗费能源的。

所以，区块链的使用场景，非常有限。

1. 不存在所有与成员都信任的管理当局
2. 写入的数据不要求实时使用
3. 挖矿的收益能够弥补本身的成本

如果不能满足上述条件，那么传统的数据库是最好的解决方案。目前，区块链最大的应用场景就是以比特币为代表的加密货币。


