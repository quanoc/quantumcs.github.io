---
layout:     post
title:      "NLP简单入门"
subtitle:   "NLP introduction"
date:       2018-04-26
catalog:    true
author:     "Nova"
header-img: "/img/post-img/0003.jpg"
tags:
    - NLP
---

NLP (Natural Language Processing) 是人工智能（AI）的一个子领域。研究如何让计算机读懂人类语言。

首先，在这里介绍下NLP的应用级别入门，了解一些基本的概念、应用场景，学会使用之后，再去深究其中的技术实现和算法原理。

- 能做什么
- 一些相关概念
- NLP自定义：像自定义语料等
- 现有NLP库
- 百度的自然语言处理服务API
- 书籍推荐


### 能做什么
简单的例子：百度搜索[4个又念什么]，出现的结果是叕。

像类似的搜索服务，推荐系统及智能聊天等场景都有涉及。可以提供一种NLP服务供这些场景使用。

这背后计算机是怎么实现这个语义识别的，这就是NLP的应用。


### 一些概念

- 两个学派

自然语言处理的两个学派：语言学派和统计学派。

统计自然语言处理运用了推测学、机率、统计的方法来解决上述，尤其是针对容易高度模糊的长串句子，当套用实际文法进行分析产生出成千上万笔可能性时所引发之难题。处理这些高度模糊句子所采用消歧的方法通常运用到语料库以及马可夫模型（Markov models）。统计自然语言处理的技术主要由同样自人工智能下与学习行为相关的子领域：机器学习及资料采掘所演进而成。

- 语料

语料库就是把平常我们说话的时候的句子、一些文学作品的语句段落、报刊杂志上出现过的语句段落等等在现实生活中真实出现过的语言材料整理在一起，形成一个语料库，以便做科学研究的时候能够从中取材或者得到数据佐证。 

例如我如果想写一篇关于“给力”这个词的普及性的文章，就可以到语料库中查询这个词出现的频率、用法等等。

[百度百科：语料](https://baike.baidu.com/item/%E8%AF%AD%E6%96%99/8062686?fr=aladdin)

[知乎：语料的概念](https://www.zhihu.com/question/24015002)

- 命名实体识别

一般来说，命名实体识别的任务就是识别出待处理文本中三大类（实体类、时间类和数字类）、七小类（人名、机构名、地名、时间、日期、货币和百分比）命名实体。

> 识别文本中具有特定意义的实体，主要包括人名、地名、机构名、专有名词等。

[百度百科：命名实体识别](https://baike.baidu.com/item/%E5%91%BD%E5%90%8D%E5%AE%9E%E4%BD%93%E8%AF%86%E5%88%AB/6968430)

[命名实体识别的两种方法](https://blog.csdn.net/babydx/article/details/77836810)

### 现有NLP库

- HanLP

HanLP是由一系列模型与算法组成的Java工具包，目标是普及自然语言处理在生产环境中的应用。

不仅仅是分词，而是提供词法分析、句法分析、语义理解等完备的功能

自带一些语料处理工具，帮助用户训练自己的语料。

### 百度的自然语言处理服务API
以下是提供的API服务，前4个是比较常用的。

- 中文分词
- 中文词向量表示
- 词义相似度
- 短文本相似度
- 中文DNN语言模型
- 评论观点抽取
- 情感倾向分析
- 文章分类
- 文章标签
- 依存句法分析
- 词法分析
- 词法分析（定制）

[百度AI开放平台：自然语言处理服务](http://ai.baidu.com/tech/nlp)


### 书籍推荐
[自然语言处理综论](http://www.52nlp.cn/tag/%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86%E7%BB%BC%E8%AE%BA)

[几本自然语言处理入门书](http://www.52nlp.cn/natural-language-processing-primer-books)

[非常不错的入门资料：(NLP)入门指南](https://blog.csdn.net/xgjianstart/article/details/77157393)

[自然语言处理(NLP)入门学习资源清单](https://blog.csdn.net/lb521200200/article/details/73321334)

- 网站

[自然语言处理怎么最快入门？](https://www.zhihu.com/question/19895141)

[我爱自然语言处理](http://www.52nlp.cn/)

[百度AI开放平台：自然语言处理服务](http://ai.baidu.com/tech/nlp)

[HanLP自然语言处理包开源](http://www.hankcs.com/nlp/hanlp.html#h3-6)

- 文章

[利用NLP技术获取托福的核心词汇](http://group.jobbole.com/11115/?utm_source=group.jobbole.com&utm_medium=relatedTopics)

[500 行 Python 代码做一个英文解析器](http://blog.jobbole.com/67009/?utm_source=group.jobbole.com&utm_medium=relatedArticles)

[150 多个 ML、NLP 和 Python 相关的教程](http://blog.jobbole.com/112185/?utm_source=group.jobbole.com&utm_medium=relatedArticles)

[NLP入门+实战必读：一文教会你最常见的10种自然语言处理技术](http://baijiahao.baidu.com/s?id=1583572877180330664&wfr=spider&for=pc)


下一阶段任务：了解NLP都有那些服务，相关的概念及相互关系。怎么用这些服务。

最后：应用层面的话，先了解都有哪些NLP服务，能做什么，可以借鉴百度的API学习学习。然后能开发自己的app使用起来。这是第一阶段，第二阶段是可以实现NLP的算法，做一些模型，已提供其它应用程序使用。第三阶段应该是熟悉这些算法相关的细节。可以做出新的模型什么的。come on。