title: Flarum, 一个轻量化新兴的开源论坛源码
categories: 教程
tags: [源码]
date: 2020-04-21 14:29:00
toc: true
---
前言
===

说起开源论坛程序，我们都会想到国内两大巨头：Discuz 和 phpwind。一个拥抱腾讯一个拥抱阿里，实力确实不容小视。当然他们本身确实也是做得很强大，不仅仅是论坛，还可以用来做 CMS，企业网站等等。大家都知道虎嗅网一开始也是用的 Discuz！（还有宝塔面板）

所以，程序强大了，也就使得程序本身变得越来越臃肿，对于那些只想单纯做一个论坛的用户来说，很显然这些臃肿的程序已经不适合他们了。于是我们开始寻找国内外的一些其他替代品，轻论坛产品。

国内目前的一些轻论坛产品，像 Xiunobbs，Startbbs 等等，体验下来，总感觉差点意思。
<!-- more -->
Flarum
===
上个月，一次偶让的机会在网上看到国外一个新兴的开源社区程序 ——Flarum。  
Flarum 是一款现代的，优雅的，简洁的，强大的论坛软件。Flarum 让在线交流变得更加轻松愉快。虽然现在他只是 Beta 版（已更新到 beta12），但是相信在未来一定会退出正式 版本！(听说开发
者走了一个)  
不过这个开发者的理由有点扯淡   
[Why We're Building Flarum](https://flarum.org/story/)  
Flarum 官网并不支持中文，国内衍生了不少 Flarum 中文网，这里推荐两个主要的  
1.[Flarum 中文站：优雅简洁的轻论坛](https://www.flarum.org.cn/) 这个主要是讲官方的代码，主要基于官方的教程翻译而来  
2.[FlarumChina](https://www.flarumchina.org/) 这个网站主要是基于官方代码进行本土化，二次打包，进行发布，对于新手支持较好
这里我个人比较推荐 Flarumchina，他的网站和软件都是同步官方更新的，挺不错。
他们的论坛 [TowerLight Community](https://flarum.atowerlight.cn/)

安装教程
===
虚拟机安装可以看 [Gitee releases](https://gitee.com/FlarumChina/flarum) 或者 [Github releases](https://github.com/skywalker512/FlarumChina/releases/tag/v0.1.0-beta.12)，下载压缩包  

下面是内置 nginx 和 fpm 的 docker 镜像，据维护者说，在稳定下来之后将会有更多选项，目前代码在[此处](https://gitee.com/FlarumChina/flarum)


以下是环境变量参考:
```
DEBUG=false
FORUM_URL=http://xxx

#FlarumChina 特殊的可以使用 cdn 来加速静态资源，若不使用，请与 FORUM_URL 相同
FORUM_CDN = xx 

DB_HOST=xx
DB_NAME=xx
DB_USER=xx
DB_PASS=xx
DB_PREF=xx
DB_PORT=3306

FLARUM_ADMIN_USER=admin
FLARUM_ADMIN_PASS=xxx
FLARUM_ADMIN_MAIL=xxx@xxx.com
FLARUM_TITLE=xxx
```
宝塔面板安装
===
#### 安装面板
请访问[面板信息](https://bt.cn/download/linux.html)以获取安装信息。  
据博主发文前，最新版为 v0.1.0-beta.12 [点击前往下载](https://gitee.com/FlarumChina/FlarumChina/attach_files/350061/download)
#### 安装环境  
![envshow](https://pan.johnsonran.cn/AliDrive/Blog-IMG/Flarum/1.png)  
此处 Nginx，PHP，Mysql 是必须的，切记。  
#### 安装 PHP 插件
![phpplug](https://pan.johnsonran.cn/AliDrive/Blog-IMG/Flarum/2.png)
![phpplugshow](https://pan.johnsonran.cn/AliDrive/Blog-IMG/Flarum/3.png)
#### 网站配置伪静态
![fakestatic](https://pan.johnsonran.cn/AliDrive/Blog-IMG/Flarum/4.png)
可以填入这个:
```javascript
location / {
  try_files $uri $uri/ /index.php?$query_string;
}
```
当然最好全部填入:
```javascript
location / {
  try_files $uri $uri/ /index.php?$query_string;
}

location = /sitemap.xml { 
  try_files $uri $uri/ /index.php?$query_string; 
}

location ~* \.(?:manifest|appcache|html?|xml|json)$ {
  add_header Cache-Control "max-age=0";
}

location ~* \.(?:rss|atom)$ {
  add_header Cache-Control "max-age=3600";
}

location ~* \.(?:jpg|jpeg|gif|png|ico|cur|gz|svg|mp4|ogg|ogv|webm|htc)$ {
  add_header Cache-Control "max-age=2592000";
  access_log off;
}

location ~* \.(?:css|js)$ {
  add_header Cache-Control "max-age=31536000";
  access_log off;
}

location ~* \.(?:ttf|ttc|otf|eot|woff|woff2)$ {
  add_header Cache-Control "max-age=2592000";
  access_log off;
}

gzip on;
gzip_comp_level 5;
gzip_min_length 256;
gzip_proxied any;
gzip_vary on;
gzip_types
    application/atom+xml
    application/javascript
    application/json
    application/ld+json
    application/manifest+json
    application/rss+xml
    application/vnd.geo+json
    application/vnd.ms-fontobject
    application/x-font-ttf
    application/x-web-app-manifest+json
    application/xhtml+xml
    application/xml
    font/opentype
    image/bmp
    image/svg+xml
    image/x-icon
    text/cache-manifest
    text/css
    text/plain
    text/vcard
    text/vnd.rim.location.xloc
    text/vtt
    text/x-component
    text/x-cross-domain-policy;
```
#### 设置运行目录
要确保网站根目录结构如下  
* Apache 请开启 mod_rewrite 并将网站根目录设置到 /path/to/flarum/public
* Nginx 进行设置 并将网站根目录设置到 /path/to/flarum/public
 
 
   接下来访问网站，进行设置，就可以了