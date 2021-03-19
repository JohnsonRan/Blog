title: Typecho修改Gravatar头像为镜像源
categories: 干货
tags: [Gravatar]
date: 2021-02-28 20:17:00
---
1. `/var/Typecho/common.php`第991行：
```php
$url = $isSecure ? 'https://secure.gravatar.com' : 'http://secure.gravatar.com';
```
修改为
```php
  $url = $isSecure ? 'https://gravatar.inwao.com' : 'https://gravatar.inwao.com';
```

- 节点位置：韩国 首尔 oracle 云计算数据中心
- 节点协议：TLS1.2

来自大佬：https://inwao.com/gravatartx.html