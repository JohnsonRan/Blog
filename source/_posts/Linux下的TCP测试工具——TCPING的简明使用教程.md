title: Linux下的TCP测试工具——TCPING的简明使用教程
categories: 教程
tags: [扫描工具]
date: 2020-04-21 14:31:00
toc: true
---
前言
===
PING是一种网络工具，用来测试数据包能否通过ICMP协议到到达目标主机，程序会按时间和成功响应的次数估算丢失数据包率。但是相较于ICMP协议，TCP则更为广泛的被大家熟知和使用。下面我们介绍一种新型的网络测试工具——TCPING。

TCPING是基于TCP协议的一种PING命令，用来测试数据包能否通过TCP协议到到达目标主机（其实就是抄上面的描述）。他又一大特点，就是可以监听某个端口的状态，在禁PING的时候，也可以检测网络连通率。闲话少说，开始教程:
<!-- more -->
> 前提条件:  
操作系统:CentOS6+/Debian7+/Ubuntu12+

开始操作
---
1. 先更新系统软件源:
```
yum update -y
#CentOS系统
apt-get update -y
#Debian/Ubuntu系统
```
2. 安装依赖，这里用到的是tcptraceroute和bc
```
yum install -y tcptraceroute bc
#CentOS系统
apt-get install -y tcptraceroute bc
#Debian/Ubuntu系统
```
3. 安装TCPING
* 切换目录到/usr/bin:
```
cd /usr/bin
```
* 下载TCPING:
```
wget -O tcping http://www.vdberg.org/~richard/tcpping
```
* 赋予文件执行权限:
```
chmod +x tcping
```
4.测试TCPING:
```
[root@localhost bin]# tcping 8.8.8.8 53
# 通过TCP-PING 8.8.8.8 端口 53 
traceroute to 8.8.8.8 (8.8.8.8), 255 hops max, 60 byte packets
seq 0: tcp response from google-public-dns-a.google.com (8.8.8.8) <syn,ack>  1.723 ms
traceroute to 8.8.8.8 (8.8.8.8), 255 hops max, 60 byte packets
seq 1: tcp response from google-public-dns-a.google.com (8.8.8.8) <syn,ack>  8.850 ms
traceroute to 8.8.8.8 (8.8.8.8), 255 hops max, 60 byte packets
```

附录
---
用法详解:
```
# 用法：tcpping [-d] [-c] [-C] [-w sec] [-q num] [-x count] ipaddress [port]
# -d 在每个响应时间前，打印时间戳
# -c 以列表形式显示
# -C 输出类似于fping工具中-C选项的结果
# -w 等待时间（默认 3）
# -r 每N秒重试一次（默认 1）
# -x 限定测试总时长 (默认 无限)
# 实例：测试服务器到大陆TCP是否畅通
# 在这里，我们要用到百度官网的IP：119.75.217.109 以及他的TCP端口：80
```