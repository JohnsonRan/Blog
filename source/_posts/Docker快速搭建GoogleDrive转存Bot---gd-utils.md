title: Docker快速搭建GoogleDrive转存Bot - gd-utils
categories: 教程
tags: [Docker,GoogleDrive]
date: 2020-07-21 18:36:00
toc: true
---
>看到有人说搭建不成功，我觉得非常简单，
安装版编译时我两台1G的小鸡的卡着不动，还是大鸡编译好小鸡用好,如果只能搭建bot推荐使用docker版  

##准备：
- SA配置文件(Service Accounts)
参考:http://blog.jialezi.net/?post=153
- **TelegramBot**的`API`,使用**BotFather**创建时可见
- 一个可解析的域名
- **GoogleDrive**文件夹的`ID`
- `Docker`镜像
(我自用的小修改版 https://hub.docker.com/r/jialezi/gd-utils)
原版：https://github.com/gdtool/gd-utils-docker.git

##开始搭建
###示例：
域名: abc.com (提前解析abc.com到你的服务器`IP`,用于申请`SSL`)
**GoogleDrive**文件夹的`ID`为: 1hhDZw2SKjNeuwWroHSenoY-TXiFZyDoM
**TelegramBot**的`API`为: 13xxxx2380:AAGDPL_2-LPIA0iQ6RxxxxM9bBOFjtErFGE

1. 下载配置文件，按要求修改里面的参数
```bash
wget https://raw.githubusercontent.com/gdtool/gd-utils-docker/master/config.example.js -O config.js
```
>用SA的修改以下三项即可，其他按需修改
>const DEFAULT_TARGET = ''(必填，拷贝默认目的地ID，如果不指定target，则会复制到此处，建议填写团队盘ID)
>tg_token: '', // 你的**TelegramBot**的`token`,获取方法参考：https://core.telegram.org/bots#6-botfather
>tg_whitelist:  [''] // 你的Telegram用户名(t.me/username)，bot只会执行这个列表里的用户所发送的指令

2. **创建**SA文件夹**上传**SA文件**到**SA文件夹

3. 运行
```bash
docker run --restart=always  -idt -e USERPWD=123qwe  -p 443:443  -p 80:80   -e Domain=abc.com -v ${PWD}/sa:/gd-utils/sa  -v ${PWD}/config.js:/gd-utils/config.js   --name gd-utils jialezi/gd-utils
```
>申请SSL需要80端口
>**USERPWD**为**shellinabox**密码  
>**Domain**=abc.com 为自己域名，要提前解析，由caddy自动申请SSL

4. 对接TelegramBot
```bash
curl -F "url=[YOUR_WEBSITE]/api/gdurl/tgbot" 'https://api.telegram.org/bot[YOUR_BOT_TOKEN]/setWebhook'
```
>例如：
>curl -F "url=https://abc.com/api/gdurl/tgbot" 'https://api.telegram.org/bot1394xxx380:AAGDPL_2-LPIA0iQ6xxxxM9bBOFjtErFGE/setWebhook'
>返回true ok

##访问
1.https://abc.com   //**gd-utils**地址
2. https://abc.com/shell    //**shellinabox**地址，使用**root**需要先使用**gd**用登录，再切换**root**
账号**gd**，密码在上面自行设置，使用`su root`切换到**root**
3. https://abc.com/file  //filebrowser 账号密码**admin**




