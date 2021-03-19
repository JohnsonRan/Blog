title: 如何编译NEWIFI3固件-Padavan(老毛子)
categories: 教程
tags: [教程,编译]
date: 2020-04-21 14:27:00
---
编译前的准备工作
===
~~你需要一台Linux平台的系统，这里使用[Debian10](https://mirrors.tuna.tsinghua.edu.cn/debian/)~~ 这不是废话吗！  
首先从Github下载你想编译的固件仓库并进入，这里使用ChongshengB的固件:[链接](https://github.com/chongshengB/rt-n56u)
```bash
git clone https://github.com/chongshengB/rt-n56u.git /opt/rt-n56u-n3
cd /opt/rt-n56u-n3
```

* 安装依赖包:
```bash
sudo apt update
sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd sudo nano screen \
cpio git python-docutils gettext automake autopoint texinfo build-essential help2man \
pkg-config zlib1g-dev libgmp3-dev libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev gcc-multilib
```

* 准备工具链
```bash
cd /opt/rt-n56u-n3/toolchain-mipsel
# 可以从源码编译工具链，这需要一些时间：
./clean_toolchain
./build_toolchain
#或者下载预编译的工具链：
mkdir -p toolchain-3.4.x
wget https://github.com/hanwckf/padavan-toolchain/releases/download/v1.1/mipsel-linux-uclibc.tar.xz
tar -xvf mipsel-linux-uclibc.tar.xz -C toolchain-3.4.x
```

选择你需要的插件
===
```bash
cd trunk
nano build_firmware_modify
```

执行后会出现如下界面:  
![PlugSetup](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CompliePadavan/PlugSetup.png)  
如果你想使用Adbyby plus++可将下图的`n`改为`y`  
![eg](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CompliePadavan/eg.png)  
其他配置同上!  

* 修改机型文件:  
```bash
nano /opt/rt-n56u/trunk/configs/templates/PSG1218.config
```

开始编译
===
* 修改你的固件编译时间:
```bash
nano versions.inc
```

* 清理代码树并开始编译
```bash
cd /opt/rt-n56u/trunk
sudo ./clear_tree
sudo ./build_firmware_modify NEWIFI3
```

如果觉得编译时间是在太长，不想干等着，可在编译之前执行:
```bash
screen -S padavan
```

执行完后，可按下`Ctrl`+`A`+`D`退出该会话，恢复会话则使用:
```bash
screen -r padavan
```

编译完成后,执行`cd images`看到你所编译的固件
![compshow](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CompliePadavan/compshow.png)  
如想下载该固件，可以使用`Python`自带的软件包`SimpleHTTPServer`搭建文件浏览器来下载:
```bash
python -m SimpleHTTPServer
```

![httpserver](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CompliePadavan/httpserver.png)

---
教程就到此结束啦!  
本博客文章均为原创，转载请注明来源

---