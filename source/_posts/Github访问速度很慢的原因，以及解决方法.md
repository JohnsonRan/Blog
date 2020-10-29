title: Github访问速度很慢的原因，以及解决方法
categories: 干货
tags: [Github,hosts,CDN]
date: 2020-06-27 20:52:00
---
##什么是CDN
CDN，Content Distribute Network，可以直译成内容分发网络，CDN解决的是如何将数据快速可靠从源站传递到用户的问题。用户获取数据时，不需要直接从源站获取，通过CDN对于数据的分发，用户可以从一个较优的服务器获取数据，从而达到快速访问，并减少源站负载压力的目的。
##为什么访问速度慢，下载慢？
Github的CDN被某墙屏了，由于网络代理商的原因，所以访问下载很慢。ping github.com 时，速度只有300多ms。
##解决方法
绕过dns解析，在本地直接绑定host，该方法也可加速其他因为CDN被屏蔽导致访问慢的网站。

hosts文件所在目录:
```bash
C:\Windows\System32\drivers\etc
```
修改windows里的hosts文件，添加如下内容
```bash
# Github Hosts
# update: 2020-06-10
140.82.113.3 github.com
140.82.113.3 github.global.ssl.fastly.net
140.82.113.3 nodeload.github.com
140.82.113.3 api.github.com
140.82.113.3 training.github.com
140.82.113.3 codeload.github.com
185.199.108.153 assets-cdn.github.com
185.199.108.153 documentcloud.github.com
185.199.108.153 help.github.com
185.199.108.153 githubstatus.com
199.232.68.133 raw.github.com
199.232.68.133 cloud.githubusercontent.com
199.232.68.133 gist.githubusercontent.com
199.232.68.133 marketplace-screenshots.githubusercontent.com
199.232.68.133 raw.githubusercontent.com
199.232.68.133 repository-images.githubusercontent.com
199.232.68.133 user-images.githubusercontent.com
199.232.68.133 desktop.githubusercontent.com
199.232.68.133 avatars0.githubusercontent.com
199.232.68.133 avatars1.githubusercontent.com
199.232.68.133 avatars2.githubusercontent.com
199.232.68.133 avatars3.githubusercontent.com
199.232.68.133 avatars4.githubusercontent.com
199.232.68.133 avatars5.githubusercontent.com
199.232.68.133 avatars6.githubusercontent.com
199.232.68.133 avatars7.githubusercontent.com
199.232.68.133 avatars8.githubusercontent.com
```
##Windows下刷新DNS的方法：
```bash
打开cmd
输入ipconfig /flushdns
```
亲测有用，下载速度明显提升

 

