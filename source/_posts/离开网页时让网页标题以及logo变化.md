title: 离开网页时让网页标题以及logo变化
categories: 干货
tags: [Javascript]
date: 2020-09-07 20:29:00
---
```javascript
<script>
  document.addEventListener('visibilitychange', function () {
    if (document.visibilityState == 'hidden') {
        normal_title = document.title;
        document.title = 'Free Porn Videos & Sex Movies - Porno,XXX,Porn Tube | Pornhub'; //替换网站title
        var url="URL" ;//替换网站logo               
        var link = document.querySelector("link[rel*='icon']") || document.createElement('link');
        link.type = 'image/png';
        link.rel = 'leave icon';
        link.href = url;
        document.getElementsByTagName('head')[0].appendChild(link);
    } else document.title = normal_title;
});
</script>
```
修改到TYPECHO博客主题里时，需要写入到主题header.php文件里。