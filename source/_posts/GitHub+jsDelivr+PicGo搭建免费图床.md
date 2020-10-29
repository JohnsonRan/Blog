title: GitHub+jsDelivr+PicGo搭建免费图床
categories: 教程
tags: [Github,jsdeliver,图床,CDN]
date: 2020-06-30 19:12:00
---
[notice]本篇文章主要讲解如何使用PicGo上传并使用jsDelivr加速，当然你也可以手动上传或者加速其他静态文件。
[/notice]
jsDelivr是一个比较好的CDN平台，官方号称jsDelivr – Open Source CDN free, fast, and reliable，简单来说就是开源的CDN，免费、快、可靠。
##使用限制
- 目前GITHUB仓库容量是没有上限的！不过官方推荐在1G以内！
- 仓库单个文件50M会收到警告，大于100M会被拒绝！
- jsDelivr仅能针对50M以下的文件CDN加速！
放一个测试图：https://img.johnsonran.cn/qndxx/vconsole.jpg
来源于：[跳过微信青年大学习的方法](https://521331.xyz/archives/14.html)

##创建仓库
当然，首先你得有个`Github`的帐号。
新建一个`仓库`，填写`仓库名`，将权限设置成`public`，根据需求选择是否为仓库初始化一个`README.md`描述文件。
![创建新仓库](https://img.johnsonran.cn/Pic-Bed/new.png)  

##生成token
点击用户头像 -> 选择`Settings`
![Settings](https://img.johnsonran.cn/Pic-Bed/Settings.png)  
点击`Developer settings`
![Developer settings](https://img.johnsonran.cn/Pic-Bed/Developer.png)  
点击`Personal access tokens`并点击`Generate new token`新建一个`Token`。
![Personal access tokens](https://img.johnsonran.cn/Pic-Bed/tokens.png)  
填写`Token`描述，勾选`repo`，然后点击`Generate token`生成一个`Token`。
![Generate token](https://img.johnsonran.cn/Pic-Bed/tokencreate.png)

##获取Token密钥
生成之后会显示Token的密钥，复制保存好。
[notice]注意这个Token只会显示一次，自己先保存下来，或者等后面配置好PicGo后再关闭此网页。
[/notice]

##配置PicGo
进入[PicGo官网](https://github.com/Molunerfinn/PicGo/releases)下载，由于Github的问题下载速度较慢，在这里使用放上CloudFlare加速后的链接:[稳定版](https://github.johnsonran.workers.dev/https:/github.com/Molunerfinn/PicGo/releases/download/v2.2.2/PicGo-Setup-2.2.2.exe)
[测试版](https://github.johnsonran.workers.dev/https://github.com/Molunerfinn/PicGo/releases/download/v2.3.0-beta.1/PicGo-Setup-2.3.0-beta.1.exe)
使用CloudFlare加速Github下载文章链接:[点我](https://521331.xyz/archives/10.html)  

- 设定仓库名：按照`用户名/图床仓库名`的格式填写
- 设定分支名：`master`
- 设定Token：粘贴之前生成的`Token`
- 指定存储路径：填写想要储存的路径，如`Pic-Bed/`，这样就会在仓库下创建一个名为`Pic-Bed`的文件夹，图片将会储存在此文件夹中。
- 设定自定义域名：它的的作用是，在图片上传后，PicGo会按照`自定义域名+上传的图片名`的方式生成访问链接，放到粘贴板上，因为我们要使用`jsDelivr`加速访问，所以可以设置为`https://cdn.jsdelivr.net/gh/用户名/图床仓库名`
![PicGo](https://img.johnsonran.cn/Pic-Bed/picgo.png)

##上传图片
配置完成之后，只需要将图片拖动上传即可，然后在相册区可以复制链接了。
![Pic-Upload](https://img.johnsonran.cn/Pic-Bed/pic-upload.png)

##手动上传
直接使用`Git`或者网页上传`图片/文件夹`即可
官方的访问方法就是：
`https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名或版本号/文件名`
例如我在repo根目录下传了一张名为`1.jpg`的图片，那么文件链接就是
`https://cdn.jsdelivr.net/gh/lbwnb/Pic-Bed@1.0/1.jpg`
上面说了也可以不创建`releases`，就直接用分支代替版本号也是可以的。
`https://cdn.jsdelivr.net/gh/lbwnb/Pic-Bed@master/1.jpg`
jsdelivr也可以直接获取仓库目录，格式如下。
`https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名或版本号/`
[notice]如果直接使用分支进行访问，例：`https://cdn.jsdelivr.net/gh/lbwnb/Pic-Bed@master/1.jpg`。
`master`分支会有缓存，缓存应该是一天更新一次。如果想进行及时更新，可以把`master`直接改成`latest`即可。
格式如下：`https://cdn.jsdelivr.net/gh/lbwnb/cdn@latest/1.jpg`
[/notice]

[notice]建议只用作静态文件加速，例如`JS/CSS/Image`。并不适合大文件分发，大文件分发还是移步国内各厂的对象存储。
[/notice]