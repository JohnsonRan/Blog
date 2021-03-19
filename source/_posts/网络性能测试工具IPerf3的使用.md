title: 网络性能测试工具IPerf3的使用
categories: 教程
tags: [测试工具]
date: 2020-04-21 14:29:00
---
>iPerf3 is a tool for active measurements of the maximum achievable bandwidth on IP networks. It supports tuning of various parameters related to timing, buffers and protocols (TCP, UDP, SCTP with IPv4 and IPv6). For each test it reports the bandwidth, loss, and other parameters. This is a new implementation that shares no code with the original iPerf and also is not backwards compatible. iPerf was orginally developed by [NLANR/DAST](https://iperf.fr/contact.php#authors). iPerf3 is principally developed by [ESnet](https://www.es.net/) / [Lawrence Berkeley National Laboratory](https://www.lbl.gov/). It is released under a three-clause [BSD license.](https://en.wikipedia.org/wiki/BSD_licenses)

以下内容来自Google Translate:  
>iPerf3是用于主动测量IP网络上可达到的最大带宽的工具。 它支持与时序，缓冲区和协议（TCP，UDP，带有IPv4和IPv6的SCTP）相关的各种参数的调整。 对于每个测试，它都会报告带宽，损耗和其他参数。 这是一个新的实现，与原始iPerf不共享任何代码，并且也不向后兼容。 iPerf最初是由NLANR / DAST开发的。 iPerf3主要由ESnet / Lawrence Berkeley国家实验室开发。 它以三条款BSD许可发布。


Linux版本下载地址：http://code.google.com/p/iperf/downloads/list

下载IPerf3
---
大家可以从[IPerf](https://iperf.fr/)官网下载Windows版IPerf，而安卓版则可以从[Google Play](https://play.google.com/store/apps/details?id=com.nextdoordeveloper.miperf.miperf)下载  
或者从我给各位大佬提供的站点下载：[Windows版](https://pan.johnsonran.cn/AliDrive/Blog-IMG/IPerf3/IPerf3%E7%BD%91%E7%BB%9C%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95%E5%B7%A5%E5%85%B7.zip) [安卓版](https://pan.johnsonran.cn/AliDrive/Blog-IMG/IPerf3/iperf.apk)

如何使用
---
#### PC端
如果大佬您是从我提供的站点下载的话，那么您可以直接解压压缩包所有文件并运行相对应的批处理文件辣！
例如您像让您的电脑作为客户端，则运行`IPerf3客户端.bat`，反之，如果想让您的电脑作为服务端，则运行`IPerf3服务端.bat`
关于PC的运行教程就到这里，下面来介绍安卓端的运行
#### Android端
同PC端差不多，如果你想让你的Android手机作为客户端，安装您下载的`iperf.apk`，并打开，输入`-c (你的服务端IP地址) -i (每次报告的间隔)) `点击`Stopped`运行即可!  
像这样:  ![c](https://pan.johnsonran.cn/AliDrive/Blog-IMG/IPerf3/c.png)  

我这里PC端IP为`10.0.0.120`，报告间隔为`1s`

---

如果想成为服务端，则输入`-s -i 1`点击`Stopped`运行即可!  
像这样:  ![c](https://pan.johnsonran.cn/AliDrive/Blog-IMG/IPerf3/s.png)  
意思为，将手机设为`服务端`报告间隔为`1s`

可选参数
---
太多了懒得写自己去看[Wiki](https://iperf.fr/iperf-doc.php)别让我写求求你了好吗

---
教程就到此结束啦!  
本博客文章均为原创，转载请注明来源

---