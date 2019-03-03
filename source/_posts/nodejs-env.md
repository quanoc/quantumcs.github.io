---
title: Nodejs环境搭建
date: 2017-10-12
author: Nova
catalog:    true
tags: NodeJs
categories:
    - NodeJs
thumbnail:
blogexcerpt:

---

搭建过[NodeJs](http://nodejs.cn/)环境的都知道node的版本等问题导致node环境会有各种问题。

在这里总结下，顺便对node有一个清楚的认识。

以搭建hexo环境为例


### Node环境说明及搭建
------

node环境搭建有两种方式

* 使用离线包安装
* 使用nvm管理node版本

搭建过程中注意：稳定的node版本，版本管理。保持node版本和npm版本对应。

#### 使用离线包安装
此方法是自己下载node的稳定版本，自定义安装配置。推荐这种方式。

* 下载node稳定版本

在[淘宝镜像地址](https://npm.taobao.org/)选择稳定版本下载。2016-7， 官方推荐v4.4.4长期支持版

选择node-v4.4.4-linux-x86.tar.gz（这里是32位的ubuntu，所以选择32位node）将其移动到 /opt/目录下，解压

```
tar -xJf node-v4.4.4-linux-x86.tar.xz
```

然后配置node 和npm环境，使命令可以使用

```
ln -s /opt/node-v4.4.4-linux-x86/bin/node  /usr/local/bin/node
ln -s /opt/node-v4.4.4-linux-x86/bin/npm  /usr/local/bin/npm
```

验证

```
node -v
npm -v
```

* 安装hexo

```
npm install -g hexo-cli
...等好长时间
ln -s /opt/node-v4.4.4-linux-x86/bin/hexo  /usr/local/bin/hexo

## 加速方案
npm install -g hexo-cli --registry=http://registry.npm.taobao.org
```

#### 使用nvm管理node版本
nvm是很方便的node版本管理工具，但对于初学者建议使用离线安装node，以免会混淆一些概念。
要使用nvm，安装配置如下：

```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
或者
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```

使用nvm安装node，如下

```
nvm install stable
```

之后可以安装hexo

```
npm install -g hexo-cli
```

#### 安装node后的换源
node默认使用国外的源，下载三方库和框架速度缓慢，很有必要使用国内的源。

* 设置npm使用淘宝源

在~/.bashrc中添加

```
alias cnpm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
```

使其生效

```
source ~/.bashrc
```
之后就可以使用cnpm安装库

* 使用淘宝镜像安装npm包
```
cnpm install [name]
```

### 相关概念说明
------

#### 什么是hexo

> Hexo is a fast, simple and powerful blog framework. You write posts in Markdown (or other languages) and Hexo generates static files with a beautiful theme in seconds.

#### 什么是npm
NPM的全称是Node Package Manage。是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。
类似于 ruby的gem，Python的pypi、setuptools，PHP的pear。
Nodejs自身提供了基本的模块，但是开发实际应用过程中仅仅依靠这些基本模块则还需要较多的工作。幸运的是，Nodejs库和框架为我们提供了帮助，让我们减少工作量。但是成百上千的库或者框架管理起来又很麻烦，有了NPM，可以很快的找到特定服务要使用的包，进行下载、安装以及管理已经安装的包。

#### 路径说明
模块路径：node的一些库和框架，比如hexo这些一般存放在node安装路径下的lib下（例：/opt/node-v4.4.4-linux-x86/lib/node_modules）。

模块相关的命令一般存放在node安装路径下的bin下（例如：/opt/node-v4.4.4-linux-x86/bin）。

### npm相关命令
------

全局安装

```
npm install -g 软件名
```

全局安装的路径可以通过下面的命令查看

```
npm config get prefix
```

全局安装的路径可以通过下面的命令修改

```
npm config set prefix "目录"
```

局部安装（将模块下载到当前命令行所在目录），不推荐

```
npm install 软件包名
```
