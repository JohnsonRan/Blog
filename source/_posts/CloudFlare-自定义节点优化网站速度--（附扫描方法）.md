title: CloudFlare 自定义节点优化网站速度 -（附扫描方法）
categories: 干货
tags: [CloudFlare,扫描工具]
date: 2020-04-21 14:04:00
toc: true
---
前言
---
`CloudFlare` 官网虽然不提供 `CNAME`/`AAAA`/`A` 记录接入 `CloudFlare` 的 `CDN`，但是我们可以通过 `CloudFlare Partner` 免费使用 `CNAME`/`A` 记录接入 `CloudFlare` ，然后就可以解锁许多上网新知识。
而我们正好利用 `CloudFlare` 使用 `A` 记录接入 `CDN` 的方式，自定义节点 `IP` ，例如 `1.1.1.1` 等，使用 `CloudFlare` 自定义节点 `IP` 的好处就是可以一定程度上缓解 `CloudFlare` 速度慢的问题，据说 `CloudFlare` 免费套餐节点比较少，且 “鱼龙混杂”，对中国大陆的线路不友好，本文就详细教大家 `CloudFlare` 如何自定义 `IP` 节点对三网线路进行优化，以及分享一些 `CloudFlare` 分别对中国三网线路友好一点的 `IP` 段，并教大家自己寻找最优的 `CloudFlare` 的节点 `IP` ！
<!-- more -->
通过`CloudFlare Partner` 接入 `CloudFlare`
---
这里推荐 3 个大佬的:
> 1. [挖站否](https://cdn.wzfou.com)(推荐)(在挖站否可以申请免费的 railgun但是只有一个节点，付费的话就有很多了）
2. [笨牛](http://cdn.bnxb.com/) (笨牛）
3. [如优](https://cdn.rruu.net/) (王大佬）

#### 添加域名
这个相信大家都会了，就不多说了

自定义`CDN`节点`IP`
---
>> 由于 CF 的路由经常进行调整，文章的内容仅供参考，后续应该不会更新
这里收集了一些大佬们扫描出来的 IP

##### 优选节点`IP`:
```
172.64.32.1/24 （推荐移动，走香港） 
104.28.14.0/24 （推荐移动，走新加坡） 
104.23.240.0-104.23.243.254 （推荐联通、移动，线路未知） 
108.162.236.1/24 （推荐联通，走美国） 
104.20.157.0/24 （推荐联通，走日本） 
104.16.160.1/24 （推荐电信，走洛杉矶） 
172.64.0.0/24 （推荐电信，走旧金山） 
172.64.32.* （走欧洲）
```

##### `CloudFlare`的百度云合作`IP`:
```
162.159.208.4-162.159.208.103 
162.159.209.4-162.159.209.103 
162.159.210.4-162.159.210.103 
162.159.211.4-162.159.211.103 
162.159.211.4-103 
103.21.244.0/22 
103.22.200.0/22 
103.31.4.0/22 
104.16.0.0/12 
108.162.192.0/18 
131.0.72.0/22 
141.101.64.0/18 
162.158.0.0/15 
172.64.0.0/13 
173.245.48.0/20 
188.114.96.0/20 
190.93.240.0/20 
197.234.240.0/22 
198.41.128.0/17
```
##### 网友收集的`CloudFlare`国内友好`IP`:
```
108.162.236.1/24 联通 走美国 
172.64.32.1/24 移动 走香港 
104.16.160.1/24 电信 走美国洛杉矶 
172.64.0.0/24 电信 美国旧金山 
104.20.157.0/24 联通 走日本 
104.28.14.0/24 移动 走新加坡
```
##### 其他节点`IP`:
```
104.18.62.1/24 香港hkix.net
104.16.35.1/24 香港hkix.net
104.16.36.1/24 香港hkix.net
104.18.35.1/24 香港hkix.net
104.18.36.1/24 香港hkix.net
104.16.54.1/24 香港
104.16.55.1/24 香港
104.18.128.1/24 香港
104.18.129.1/24 香港
104.18.130.1/24 香港
104.18.131.1/24 香港
104.18.132.1/24 香港
104.19.195.1/24 香港
104.19.196.1/24 香港
104.19.197.1/24 香港
104.19.198.1/24 香港
104.19.199.1/24 香港
#适合电信的节点
104.23.240.*
#走欧洲各国出口 英国德国荷兰等 延迟比美国高一些 适合源站在欧洲的网站
172.64.32.*
#虽然去程走新加坡，但是回程线路的绕路的，实际效果不好，不推荐
104.16.160.*
#圣何塞的线路，比洛杉矶要快一点，推荐
108.162.236.*
#亚特兰大线路，延迟稳定，但是延迟较高
#适合移动的节点
162.158.133.* 
#走的丹麦，这一段ip只有部分能用，可以自己试一下，绕美国
198.41.214.*
198.41.212.*
198.41.208.*
198.41.209.*
172.64.32.*
141.101.115.*
#移动走香港的IP段有很多，以上并不是全部。CF移动走香港的分直连和走ntt的效果都挺不错的，不过部分地区晚上还是会丢包。
172.64.0. *
#这是走圣何塞的，一般用香港的就行
172.64.16.* 
#欧洲线路.绕
#1.0.0.1效果较好
电信部分
大多数省直接使用1.0.0.0即可，延迟低，丢包少，
# 移动部分
#新加坡
104.18.48.0-104.18.63.255
104.24.112.0-104.24.127.255
104.27.128.0-104.27.143.255
104.28.0.0-104.28.15.255
# 移动部分
#圣何塞 
104.28.16.0-31.255
104.27.144.0-243.254
104.23.240.0-243.254
#香港cloudflare1-100g.hkix.net
1.0.0.0-254
1.1.1.0-254
#香港直连
104.16.0.0-79.255
104.16.96.0-175.254
104.16.192.0-207.255
```
自动查找最优CloudFlare节点IP
---
如果上面所列出来的 `CloudFlare` 节点 `IP` 都不能使用了，博主在这里提供一个由 `犯罪高手` 大佬写的自动查找最优 `CloudFlare` 节点 `IP` bat 脚本
下载地址:[链接](https://img.mzrme.com/2020-04-11/6a14b5eb1a2de.zip)
#### 推荐使用半自动方式查找（可以发给不同地区的网站用户帮忙进行测速）
这里引用一下[小俊博主](https://www.xjisme.com/)的解释

1. 执行 1-自动查找100个丢包最少的IP.bat 设置对 IP 丢包测试 PING 的次数，默认 100 次，可手动设定，推
荐 50 次以上。运行完毕后命令行窗口会自动关闭再进行下一步操作

2. 执行 2-对100个丢包最少的IP测速.bat 此过程是利用 curl 下载托管于 cloudflare 的大文件，默认每个 IP
下载时间为 10 秒钟。下载结束后到 temp
文件夹根据文件大小排序查看下载文件的大小。文件越大，代表单位时间内传输的数据越多，速度就越快。其中文件名是以 IP
地址的名称命名的。如果想要对 IP 单线程测速，可参考第三步。如果第三步找不到好用的 IP ，可重新执行第二步，
再此完整测速分析

3. 执行 3-单IP测速.bat 输入第二步筛选出来的 IP 地址，回车后进行文件下载速度测试
#### 如果觉得以上步骤过于繁琐，请参照`最后一步`
1. 懒人版全自动处理，执行 `自动查找最优CF节点-懒人专用.bat` 等待运行完毕后自动弹出 `IP` 速度从大到小的排名文本
文件该测试的结果不一定能达到预期的效果。