title: 自己动手搭建必应壁纸API
categories: 教程
tags: [Bing,API,壁纸]
date: 2020-09-08 19:19:00
---
[必应](https://cn.bing.com)（Bing）集成了多个独特功能，包括每日首页美图，通过将来自世界各地的高质量图片设置为首页背景，美轮美奂的必应美图备很多人当做壁纸使用，今天我们自己搭建**API**服务自动获取每天的必应壁纸美图，搭建好的api服务可以用来作为网页背景或者其他服务调用，非常方便！

#本站Bing壁纸API
https://johnsonran.cn/API/bing

#原理分析
经过对必应首页的抓包，我们可以获得首页图的获取API。它的格式是这样的：
**https://cn.bing.com/HPImageArchive.aspx?format=xml&idx=0&n=1**
这里有几个GET参数，它们的作用分别是：

- n，必要参数。这是输出信息的数量。比如n=1，即为1条，以此类推，至多输出8条。
- format，非必要。返回结果的格式，不存在或者等于xml时，输出为xml格式，等于js时，输出json格式
- idx，非必要。不存在或者等于0时，输出当天的图片，-1为已经预备用于明天显示的信息，1则为昨天的图片，以此类推，idx最多获取到前16天的图片信息

这里将n设定为1、format设定为xml、idx设定为1，去发出GET请求，返回的数据是这样的：
```xml
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<images>
<image>
<startdate>20200907</startdate>
<fullstartdate>202009070900</fullstartdate>
<enddate>20200908</enddate>
<url>/th?id=OHR.OttoSettembre_ZH-CN7378112626_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp</url>
<urlBase>/th?id=OHR.OttoSettembre_ZH-CN7378112626</urlBase>
<copyright>瓦莱塔，马耳他 (© Deejpilot/GettyImages)</copyright>
<copyrightlink>https://www.bing.com/search?q=%E7%93%A6%E8%8E%B1%E5%A1%94&form=hpcapt&mkt=zh-cn</copyrightlink>
<headline/>
<drk>1</drk>
<top>1</top>
<bot>1</bot>
<hotspots/>
</image>
<tooltips>
<loadMessage>
<message>正在加载...</message>
</loadMessage>
<previousImage>
<text>上一个图像</text>
</previousImage>
<nextImage>
<text>下一个图像</text>
</nextImage>
<play>
<text>播放视频</text>
</play>
<pause>
<text>暂停视频</text>
</pause>
</tooltips>
</images>
```

其中的“images”节点下的“url”值便是我们要获取的图像地址。我们把它取出来，再加上Bing的网址前缀(http://cn.bing.com)即组合成了完整的图像地址。比如说上面返回数据的完整图像地址是这样的：

https://cn.bing.com/th?id=OHR.OttoSettembre_ZH-CN7378112626_1920x1080.jpg&rf=LaDigue_1920x1080.jpg&pid=hp

知道了背景图的获取方式，接下来就是用PHP去动态抓取了。

#搭建API
你只需新建一个php文件，贴入如下代码：
```php
<?php
$str = file_get_contents('https://cn.bing.com/HPImageArchive.aspx?idx=0&n=1');   // 从bing获取数据
 
if(preg_match('/<url>([^<]+)<\/url>/isU', $str, $matches)) { // 正则匹配抓取图片url
    $imgurl = 'https://cn.bing.com'.$matches[1];
} else {  // 如果由于某些原因，没抓取到图片地址
    $imgurl = 'https://img.infinitynewtab.com/InfinityWallpaper/2_1.jpg'; // 使用默认的图像(默认图像链接可修改为自己的)
}
 
header("Location: {$imgurl}");    // 跳转至目标图像
```

#使用方法
直接将那个php文件的绝对地址当做图片放进网页中即可。你也可以直接调用本站的必应壁纸api服务使用
地址: https://johnsonran.cn/API/bing