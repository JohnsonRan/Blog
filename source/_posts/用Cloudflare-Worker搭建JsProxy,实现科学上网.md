title: 用Cloudflare Worker搭建JsProxy,实现科学上网
categories: 教程
tags: [CloudFlare]
date: 2020-06-30 14:20:00
toc: true
---
这里感谢一下Github上的这位大佬开源
<!-- more -->
```html
https://github.com/yangmyc/jsproxy
https://github.com/yangmyc/jsproxy/tree/master/cf-worker
```
注册，登陆，Start building，取一个子域名，Create a Worker。
复制 index.js 到左侧代码框，Save and deploy。如果正常，右侧应显示首页。
收藏地址框中的https://xxxx.子域名.workers.dev，以后可直接访问。
或者可以绑定自己的域名来简化长度