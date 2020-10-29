title: NextDNS-一个性化DNS
categories: 干货
tags: [DNS]
date: 2020-08-22 20:11:00
---
在得知阿里DNS支持DOH之后，我试用了将近一周，发现他并不能让我满意，他和大多数公共DNS一样，被污染了，直接给我不返回解析，在DNS被污染的情况下,DOH又能起多大用呢，对于另一家红鱼DNS，虽然说是免费一个月，但一个月后不能白嫖，还是没去用，后来我在基安发现了它-NextDNS
打开官网：[nextdns.io](nextdns.io) ，注册登录，于是乎你就可以看到这样一个界面
![showup](https://img.johnsonran.cn/NextDNS/showup.png)
网站会自动给你下发DNS，你想要的DOH(DNS-over-HTTPS)，DOT(DNS-over-TLS)全都有！
当然，它还有类似于红鱼DNS的功能，个性化功能如下图一览:
![custom](https://img.johnsonran.cn/NextDNS/custom.png)
![custom1](https://img.johnsonran.cn/NextDNS/custom1.png)
![custom2](https://img.johnsonran.cn/NextDNS/custom2.png)
对于费用方面，白嫖党的胜利？（不是）
![prize](https://img.johnsonran.cn/NextDNS/prize.png)
每月**30万**DNS免费请求，付费版**12RMB/Month**，无限制
速度方面
![speed](https://img.johnsonran.cn/NextDNS/speed.png)
数据源自ipip.net中国大陆ping检测情况，基本无丢包，延迟均在80-100左右