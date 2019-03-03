---
layout:     post
title:      "系统load average详解及问题排查思路"
subtitle:   "load average"
date:       2018-04-12
catalog:    true
author:     "Nova"
header-img: "post-bg-es-learn.jpg"
tags:
    - Linux
---


load average指的是top命令输出的表示系统负载的展示项。
有三个值：load average: 18.89, 17.93, 16.33。他们表示系统平均负载。

系统平均负载被定义为特定时间间隔内*运行队列*中的平均进程数。

如果一个进程满足以下条件则其就会位于运行队列中：

- 它没有再等待I/O操作的结果
- 它没有主动进入等待状态（也就是没有调用wait）
- 没有被停止（例如：等待终止）

三个值分别表示1分钟、5分钟、15分钟的负载情况。数据每隔5秒钟检查一次活跃的进程数，然后根据这个数值计算出负载值。

> 如果这个数除以CPU的数目，结果高于5.就表明系统在超负荷运转了。

类比汽车过桥，一辆汽车的过桥时间就好比是处理器处理某线程的实际时间.

多核，表示有多个车道，多核CPU的话，满负荷状态的数字为 "1.00 * CPU核数"，即双核CPU为2.00，四核CPU为4.00。


### 查看系统CPU核数。
```
# 查看物理CPU的个数
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l
# 查看逻辑CPU的个数
cat /proc/cpuinfo |grep "processor"|wc -l
# 查看CPU是几核
cat /proc/cpuinfo |grep "cores"|uniq
# 查看CPU的主频
cat /proc/cpuinfo |grep MHz|uniq
# 直接获得CPU核心数  （该命令即可全部算出多少核）
grep 'model name' /proc/cpuinfo | wc -l
```




参考：

[top命令的Load average 含义及性能参考基值](https://blog.csdn.net/yjh314/article/details/51767287)

[top命令输出解释以及load average 详解及排查思路](https://blog.csdn.net/zhangchenglikecc/article/details/52103737)

[linux top命令VIRT,RES,SHR,DATA的含义](https://javawind.net/p131)
