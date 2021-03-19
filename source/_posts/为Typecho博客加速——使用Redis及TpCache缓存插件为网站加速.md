title: 为Typecho博客加速——使用Redis及TpCache缓存插件为网站加速
categories: 干货
tags: [Typecho,Redis,加速]
date: 2020-10-19 08:02:00
toc: true
---
#前言
这段时间我在我的~~删库塔~~（宝塔）看了一下我博客访问时的`CPU`占用，让我没想到的是，首次访问网站时机器的`CPU`占用高达**97.8%**，这让我惊讶不已，所以我想到使用Redis来对全站进行动态缓存，它不仅能够加快网站的访问速度，提升访客体验；同时也能保持在高并发状态下的稳定性，降低对服务器的资源占用。 在`Github`搜了搜关键词，并按提供的教程实现，这篇博客就来辣！

#介绍
##Redis
Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库。

Redis的优势：
- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作合并后的原子性执行。（事务）
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

Redis 与其他 key - value 缓存产品有以下三个特点：
- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。

##TpCache
TpCache是由老高开发的一款Typecho缓存插件，支持Memcache，Redis，Mysql三种驱动。
项目地址：https://github.com/phpgao/TpCache

#安装
##Redis
因为我用的时删库塔，可一键便捷安装Redis。如图：
![InstallRedis](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TpRedis/InstallRedis.png)
[warn-block]千万不要从宝塔开启6379外网访问，因为会把端口开放给外网，而TpCache是不支持密码连接Redis的。这种操作会使任何人都能访问并管理Redis，很危险。[/warn-block]
##TpCache
到项目地址下载插件包并解压，修改文件夹名为TpCache，上传到Typecho插件目录（`/usr/plugins`),接着到后台启用插件。
![TpCacheShow](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TpRedis/TpCacheShow.png)
如果博客使用`https`则开启`SSL`，缓存驱动选择`Redis`，端口默认为`6379`（可到宝塔面板修改），保存设置即可启用Typecho的Redis。

#管理
在宝塔面板的软件商店中，安装Redis数据管理工具，即可对Redis进行管理查看，如图：
![TpCacheShow](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TpRedis/RedisManage.png)
如需清除所有Redis缓存，在插件中的最后一项选择清除所有数据，保存设置即可。

#后记
启用Redis后确实降低了小鸡的CPU占用并提升了博客的访问速度和稳定性，不过博客的评论功能貌似会爆炸 （
只能说有利有弊吧 （