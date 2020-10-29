title: UpgradeAll - 用一款软件，检查所有应用更新
categories: 实用工具
tags: [UpgradeAll]
date: 2020-10-19 21:25:00
---
# 前言
以前我总是想着：有没有什么应用可以一键检查手机里所有应用的更新，并且可以追踪自己添加的软件呢，直到我在[Explore](https://github.com/explore)Github上的项目时，发现了一款名叫**UpgradeAll**的应用,它不仅能检查手机应用更新，还能实现从云规则下载软件源，自行添加软件源等等功能，所以来给大家安利一下。

# UpgradeAll 介绍
**UpgradeAll**这款软件可以让你通过它来检测某个应用是否有更新并帮你下载更新，但是却不同于**Google Play**，**CoolApk**等应用市场，他们只能检测到商店所上架应用的更新，而**UpgradeAll**就不同了，他支持很多个应用发布渠道，像Github，CoolApk，F-Droid啊（更多的在下方列举）它都是支持的。这对于应用有多种安装渠道的 Android 系统来说，是个非常好的工具。因为你不用打开各个应用商店来检查你手机里的应用是否为最新版。这个应用的开发者们还提供了**云规则**，覆盖了一些深受网友们喜爱的~~色批~~（正经）软件。另外，对于想自己尝试的用户，可以自行选择应用渠道并设置更新规则。  
（下面是来自UPA开发者~~肖战~~的划重点）
![划重点](https://img.johnsonran.cn/UpgradeAll/upadev.png)
## UpgradeAll 目前支持下列应用发布渠道的应用更新检测：
- Github
- Z直播
- 酷安
- 闪电下载
- 手机乐园
- F-droid
- Xposed Module Repository
- PanDownloadAndroid
- GitLab
- Google Play（将在UpgradeAll正式版完全支持）

# UpgradeAll 的使用
第一次接触这个软件可能会一脸茫然（哈？这软件怎么用的？）所以下面会详细介绍一下软件的使用

## UpgradeAll 云规则的使用方法
安装之后打开左侧侧边栏，选择云规则  
![云规则](https://img.johnsonran.cn/UpgradeAll/CloudConfig.jpg)  
(别问为啥有个2B在上面，问就是我喜欢)
在云规则列表里，有俩个选项，一个是**开发者/社区维护者们**提供的一些应用更新检测配置（如下图），如果有应用是你需要的，可以先点击右上角的搜索按钮，搜索一下软件名字。如果没有，可以先转到软件源，根据自己想添加的应用发布渠道，下载软件源云端配置留着备用  
![软件](https://img.johnsonran.cn/UpgradeAll/Softs.jpg)  
![软件](https://img.johnsonran.cn/UpgradeAll/SoftsSources.jpg)  

对于想要自己配置应用更新规则的，可以继续看下面的教程。

## UpgradeAll 自主添加应用规则
回到`主页-全部应用`，点击右下角加号按钮,会出现如下两个选项  
![软件](https://img.johnsonran.cn/UpgradeAll/AddChoice.jpg)  
`添加单个跟踪项`可以添加任何UpgradeAll**支持的软件源**,`添加应用市场`则可以对比本地已安装应用的版本号和应用市场是否一致，如果有更新版本则提示更新。  
选择`添加单个跟踪项`之后，会出现如下界面
![软件](https://img.johnsonran.cn/UpgradeAll/AddString.jpg)  
这个就是自己配置更新规则的界面了,虽然旁边有帮助按钮，但是我还是想~~多此一举~~一项一项和大家解释
- 软件源：指你想要添加的应用来自于哪里，还记得我一开始说留着备用的软件源云端配置吗 （
- 名称： 顾名思义，用来填软件名的，当然啦，只要是自己记得住的都行
- 网址：应用发布的地方，让软件帮你去看看是否有更新
- 跟踪目标：要填写应用完整的包名称，不然软件获取不到本地应用的版本号，则无法判断是否有跟新  

如果无法理解的话，下面以**UpgradeAll**这个软件来举例,这个应用发布于`Github`,发布地址为：`https://github.com/DUpdateSystem/UpgradeAll`，应用的包名为`net.xzos.upgradeall`,那么则需要这么填写:
- 软件源：Github
- 名称：UpgradeAll (或者留空)
- 网址：https://github.com/DUpdateSystem/UpgradeAll
- 跟踪目标：net.xzos.upgradeall

像我这样添加好之后，你就可以在软件列表里看到刚添加的引用了，点开之后还能看到更详细的应用跟新信息：
![软件](https://img.johnsonran.cn/UpgradeAll/InfoShow.jpg)  
啊？你说为什么最新版本和本地版本版本号差这么多？问就是开发者太懒了

PS:一般大家都可以轻松的从应用发布网站找到包名。实在找不到的，可打开手机文件管理,打开`Android/data/`文件夹这个目录里的文件夹都是所有软件的完整包名。什么？你还是觉得麻烦？那建议使用这个软件：[LibChecker](https://github.com/zhaobozhen/LibChecker)

# UpgradeAll 应用信息
应用名称：UpgradeAll
应用作者：[xz-dev](https://github.com/xz-dev)
系统版本要求：Android 5.0 及更高版本 (或许)
支持语言：中文英文皆可
开源协议：GPL-3.0
开源地址：https://github.com/DUpdateSystem/UpgradeAll
Github 下载：https://github.com/DUpdateSystem/UpgradeAll/releases
酷安应用市场地址：https://www.coolapk.com/apk/net.xzos.upgradeall