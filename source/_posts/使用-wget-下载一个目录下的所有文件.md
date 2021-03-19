title: 使用 wget 下载一个目录下的所有文件
categories: 教程
tags: [下载工具]
date: 2020-04-21 14:28:00
toc: true
---
**今天想要下载清华源上的`Termux`源到本地，使用`wget`却只是下载了一个index.html**  
**于是我就参考资料，写此博客以记录**  
方法如下：
```
wget -r -np -nH -R index.html* -e robots=off https://mirrors.tuna.tsinghua.edu.cn/termux/
```
- 各个参数的含义：

>-r : 遍历所有子目录  
-np : 不到上一层子目录去  
-nH : 不要将文件保存到主机名文件夹  
-R index.html : 不下载所有匹配到包含 `index.html` 的文件  
-e robots=off : 绕过网站的robots.txt下载文件
- 效果如下：  
![wget](https://pan.johnsonran.cn/AliDrive/Blog-IMG/DL-Site/wget.png)