title: 在VPS上搭建LNMP环境
categories: 教程
tags: [LNMP,VPS]
date: 2020-07-22 21:37:00
toc: true
---
本文将介绍如何使用第三方源在 Debian Buster 发行版安装最新版 LNMP
~~宝塔是什么辣鸡？~~
（本文 LNMP 安装过程严重参考兽兽大佬的**[这篇博客](https://sb.sb/blog/debian-install-nginx-php-mysql/)**。Debian 8 请点进去自己举一反三。）

首先我们需要一台运行 Debian 10 发行版的服务器，请尽量选择内存大于 2G 性能充足的机器（放面板还舍不得用好机器？）
我们需要依次安装 Nginx + PHP + Percona Server（MySQL 的开源替代品）。
[notice]请先使用`su root`命令进入root用户后再执行一下操作[/notice]

# 0x00 准备工作
## 0x01 更新系统并安装必要软件包
```bash
apt update && apt upgrade -y
apt install -y curl vim wget unzip apt-transport-https lsb-release ca-certificates git gnupg2
```

## 0x02 加入 Backports 源方便安装更新的软件
```bash
cat >> /etc/apt/sources.list.d/backports.list << EOF
deb http://deb.debian.org/debian $(lsb_release -sc)-backports main
deb-src http://deb.debian.org/debian $(lsb_release -sc)-backports main
EOF

apt -t $(lsb_release -sc)-backports update && apt -y -t $(lsb_release -sc)-backports upgrade
```

## 0x03 设置当前系统时间为中国上海(UTC+8)
```bash
timedatectl set-timezone Asia/Shanghai
```

## 0x04 重启系统
由于更新了内核，我们需要重启才能用上新的内核
```bash
reboot
```

# 0x10 安装 Nginx-Mainline
我们使用 [Ondřej Surý](https://deb.sury.org/) 大神打包好的 Nginx 源，附带了常用的各种工具。

## 0x11 添加 Nginx 源
```bash
wget -O /etc/apt/trusted.gpg.d/nginx-mainline.gpg https://packages.sury.org/nginx-mainline/apt.gpg
cat >> /etc/apt/sources.list.d/nginx.list << EOF
deb https://packages.sury.org/nginx-mainline/ $(lsb_release -sc) main
EOF
```

## 0x12 屏蔽 Backports 仓库中的 Nginx
```bash
cat >> /etc/apt/preferences << EOF
Package: nginx*
Pin: release a=buster-backports
Pin-Priority: 499
EOF
```

## 0x13 更新源信息并安装 Nginx
```bash
apt update
apt install -y nginx-extras
systemctl enable nginx
```

## 0x14 检查 nginx 版本
无错误执行完毕之后，我们使用 `nginx -v` 命令检查 nginx 版本
```bash
root@jr:~# nginx -v
nginx version: nginx/1.19.0
```
[notice]至此Nginx安装完成[/notice]

# 0x20 安装 PHP
同样使用 [Ondřej Surý](https://deb.sury.org/) 大神打包的 PHP 源，我们选择最新的 PHP 7.4 安装。
>[Ondřej Surý](https://deb.sury.org/) 大佬打包的 PHP 源更是好用，Ubuntu 的 PPA for PHP 就是这位大佬做的，当然少不了 Debian 的源了。

## 0x21 添加 PHP 源
```bash
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
```

## 0x22 更新源信息并安装 PHP 7.4 和常用组件和部分依赖库升级
```bash
apt update
apt install -y php7.4-fpm php7.4-mysql php7.4-curl php7.4-gd php7.4-mbstring php7.4-xml php7.4-xmlrpc php7.4-opcache php7.4-zip php7.4 php7.4-json php7.4-bz2 php7.4-bcmath
apt upgrade -y
```
**提示：这里安装的 php-fpm 的重启命令为 `systemctl restart php7.4-fpm`至此PHP安装完成**

# 0x30 安装 Percona Server 8
>“Percona Server 是由 Oracle 发布的最接近官方 MySQL Enterprise 发行版的版本。”
Percona Server 与 MySQL 完全兼容，不必担心对接问题。

## 0x31 添加并启用 Percona Server 官方源
```bash
wget https://repo.percona.com/apt/percona-release_latest.$(lsb_release -sc)_all.deb
dpkg -i percona-release_latest.$(lsb_release -sc)_all.deb
percona-release setup ps80
```

## 0x32 安装 Percona Server
```bash
apt install -y percona-server-server
```
>安装过程中会弹出设置密码界面，自行设置即可。
>接下来会弹出加密方法选择界面。由于 MySQL 8 的最新加密方法大多数客户端不支持，所以我们这里选择第二种也就是传统加密方法。

## 0x33 检查 Percona Server 版本
无错误执行完毕之后，我们使用 `mysql -v` 命令检查 Percona Server 版本
```bash
root@jr:~# mysql -v
mysql  Ver 8.0.19-10 for debian-linux-gnu on x86_64 (Percona Server (GPL), Release '10', Revision 'f446c04')
```
**至此LNMP安装完成**