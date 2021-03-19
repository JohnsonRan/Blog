title: Blaze - 端对端的文件传输免费工具
categories: 实用工具
tags: [下载工具,P2P]
date: 2020-07-21 21:01:00
toc: true
---
#前言
**Blaze** 是一款开源免费的“网页版”在线文件传输工具，它有着非常鲜明的特点，就是免安装任何程序，你只需用较为先进的浏览器 (如电脑上的 **Chrome**、新版 **Edge**、**FireFox**；手机上的 **Safari**、**Chrome** 等) 访问其网站即可传文件。
与网盘完全不同的是，**Blaze** 通过使用 **WebSockets** 和 **WebRTC** 技术，能让你的多个设备实现**Peer to Peer**「点对点」文件传输，整个传输过程完全无需将文件上传到任何的服务器，而是直接在设备之间连接互传，所以如果你的设备都处于同一局域网内，就能实现不耗费公网流量的高速且安全的内网传输了。

>部署起来的话其实是非常简单的，博主使用的是**Centos7**系统

##安装epel源
```bash
yum install epel-release
```
##更新软件
```bash
yum update -y
```
##安装node.js
```bash
yum install -y nodejs
```
##拉取代码
```bash
git clone https://github.com/blenderskool/blaze
git remote add upstream https://github.com/blenderskool/blaze.git
```
##部署
```bash
npm run dev
```
>然后会监听**3030**端口，用**ip:3030**端口访问

##后台运行
```bash
yum -y install screen //安装screen
screen -S blaze //进入screen
npm run dev //启动blaze
```

