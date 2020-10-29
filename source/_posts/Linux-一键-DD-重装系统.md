title: Linux 一键 DD 重装系统
categories: 实用工具
tags: [Linux]
date: 2020-07-25 19:05:00
---
一般情况下主机上加提供的系统镜像中内置了一些奇奇怪怪的东西，可能会对我们的应用运行造成一些负面的影响，这时你就需要一个纯净的系统了，一键 DD 重装就是一个非常不错的选择，让你在无技术的前提下完成系统的一键重装。

#支持重装的系统
- Debian 9/10
- Ubuntu 18.04/16.04
- CentOS 6/7
- 自定义 DD 镜像

#特征 / 优化
- 自动获取 IP 地址、网关、子网掩码
- 自动判断网络环境，选择国内 / 外镜像，再也不用担心卡半天了
- 超级懒人一键化，无需复杂的命令
- CentOS 7 镜像抛弃 LVM，回归 ext4，减少不稳定因素

[notice]注意:
重装后系统密码均在脚本中有提供，安装后请尽快修改密码，Linux 系统建议启用密钥登陆。
OpenVZ / LXC 架构系统不适用![/notice]

#使用方法
```bash
wget --no-check-certificate -O AutoReinstall.sh https://cdn.jsdelivr.net/gh/hiCasper/Shell/AutoReinstall.sh && chmod +x AutoReinstall.sh && bash AutoReinstall.sh
```
Linux 密码

- Pwd@CentOS
- Pwd@Linux

修改 Linux 密码
```bash
passwd
```