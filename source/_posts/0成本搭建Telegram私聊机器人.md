title: 0成本搭建Telegram私聊机器人
categories: 教程
tags: [Telegram,Bot]
date: 2020-08-30 21:24:00
---
>Telegram是一个跨平台的即时通信软件，它的客户端是自由及开放源代码软件，但是它的服务器是专有软件。用户可以相互交换加密与自毁消息，发送照片、影片等所有类型文件。
<!-- more -->
## 新建机器人
私聊[@BotFather](https://t.me/BotFather)创建新机器人
1.使用命令行创建新机器人`/newbot`，如图所示
2.回复机器人名字并以`bot`结尾，此处以举例为`JR_ChatBot`
3.提示**Done**，则创建完成，否则按提示重新命名或重新操作一遍
4.最后，获得`API token`
![newbot](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TGBot-Free/newbot.png)

## 绑定Bot API
私聊[@LivegramBot](https://t.me/LivegramBot)绑定API
1.使用命令行创建新机器人`/addbot`，如图所示
2.发送`API token`
![setup](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TGBot-Free/setup.png)
>到此为止私聊机器人已经搭建完毕，如果你经常接触到 CN（+86） 用户，可能会出现对方无法联系你的情况，此时就需要一个机器人供其联系，机器人转发消息即可。

## 测试图
![test](https://pan.johnsonran.cn/AliDrive/Blog-IMG/TGBot-Free/test.png)

本文转自:[这里](https://5ime.cn/telegram-bot.html)