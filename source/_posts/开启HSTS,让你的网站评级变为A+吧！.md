title: 开启HSTS,让你的网站评级变为A+吧！
categories: 干货
tags: [HSTS,博客]
date: 2020-09-09 20:03:00
---
#什么是HSTS
以下来自[Wikipedia](http://zh.wikipedia.org/wiki/HTTP%E4%B8%A5%E6%A0%BC%E4%BC%A0%E8%BE%93%E5%AE%89%E5%85%A8)
>HSTS 可以用来抵御 SSL 剥离攻击。SSL 剥离攻击是中间人攻击的一种，由 Moxie Marlinspike 于2009年发明。他在当年的黑帽大会上发表的题为 “New Tricks For Defeating SSL In Practice” 的演讲中将这种攻击方式公开。SSL剥离的实施方法是阻止浏览器与服务器创建HTTPS连接。它的前提是用户很少直接在地址栏输入https://，用户总是通过点击链接或3xx重定向，从HTTP页面进入HTTPS页面。所以攻击者可以在用户访问HTTP页面时替换所有https://开头的链接为http://，达到阻止HTTPS的目的。 HSTS可以很大程度上解决SSL剥离攻击，因为只要浏览器曾经与服务器创建过一次安全连接，之后浏览器会强制使用HTTPS，即使链接被换成了HTTP。 另外，如果中间人使用自己的自签名证书来进行攻击，浏览器会给出警告，但是许多用户会忽略警告。HSTS解决了这一问题，一旦服务器发送了HSTS字段，用户将不再允许忽略警告。

#场景举例
>当你通过一个无线路由器的免费 WiFi 访问你的网银时，很不幸的，这个免费 WiFi 也许就是由黑客的笔记本所提供的，他们会劫持你的原始请求，并将其重定向到克隆的网银站点，然后，你的所有的隐私数据都曝光在黑客眼下。 严格传输安全可以解决这个问题。如果你之前使用 HTTPS 访问过你的网银，而且网银的站点支持 HSTS，那么你的浏览器就知道应该只使用 HTTPS，无论你是否输入了 HTTPS。这样就防范了中间人劫持攻击。

[warn]如果你之前没有使用 HTTPS 访问过该站点，那么 HSTS 是不奏效的。网站需要通过 HTTPS 协议告诉你的浏览器它支持 HSTS。 服务器开启 HSTS 的方法是，当客户端通过HTTPS发出请求时，在服务器返回的 HTTP 响应头中包含**Strict-Transport-Security**字段。非加密传输时设置的HSTS字段无效。[/warn]

#在Apache中配置HSTS
编辑你的 apache 配置文件（如
`/etc/apache2/sites-enabled/website.conf`
和
`/etc/apache2/httpd.conf`
），并加以下行到你的 HTTPS VirtualHost：
```apache
# Optionally load the headers module:
LoadModule headers_module modules/mod_headers.so
<VirtualHost example.com:443>
    Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains; preload"
</VirtualHost>
```
现在你的 web 站点在每次访问时都会发送该请求头，失效时间是两年（秒数）。这个失效时间每次都会设置为两年后，所以，明天你访问时，它会设置为明天的两年后。
重启Apache

#在Nginx中配置HSTS
**Nginx**很简单，将下述行添加到你的 HTTPS 配置的 server 块中：
```nginx
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
```
重启Nginx

#加入HSTS Preload List
HSTS preload list是Chrome浏览器中的HSTS预载入列表，在该列表中的网站，使用Chrome浏览器访问时，会自动转换成HTTPS。Firefox、Safari、Edge浏览器也在采用这个列表。
>https://hstspreload.org/

#测试HSTS是否生效
直接打开Chrome查看网络，就可以看到头部已经包含了HSTS信息了。
![hsts-enabled](https://img.johnsonran.cn/HSTS/HSTS-ENABLED.png)