title: 路由器中如何使用CloudFlare的DDNS
categories: 教程
tags: [教程,CloudFlare]
date: 2020-04-21 14:07:00
---
前言
===
动态 DNS（英语：Dynamic DNS，简称 DDNS）是域名系统（DNS）中的一种自动更新名称服务器（Name server）内容的技术。根据互联网的域名订立规则，域名必须跟从固定的IP地址。但动态DNS系统为动态网域提供一个固定的名称服务器（Name server），通过即时更新，使外界用户能够连上动态用户的网址。摘自 维基百科-动态DNS

当你需要使用你的域名指向一个动态 ip 时，就需要有一个支持动态 dns 的域名托管商。
CloudFlare 是非常有名的域名托管商和 CDN 提供商，提供高质量的免费服务，拥有国内外大量用户。本文就将讲解如何使用 CloudFlare 实现 DDNS (动态域名解析)

开始操作
---
`CloudFlare` 提供有非常强大的 `API` ，具体可以参看 [Cloudflare API documentation v4](https://api.cloudflare.com/#zone-properties)  
为了实现 DDNS ，需要使 `A记录指向的 IP 的能够动态变化`，我们这里利用 `API` 进行定时操作

获取帐号信息
---
首先要 `获取你的 CloudFlare 帐号的相关信息`：

1. 邮箱  
就是你注册 CloudFlare 时使用的邮箱
2. Zone_ID  
这个位于你的域名设置右边的 `API` 分类下的 `Zone ID` 可直接复制下来以备使用  
![cfzoneid](https://img.johnsonran.cn/CF-DDNS/1.png)
3. API_Key  
找到 `Zone_ID` 之后，点击下面的`获取您的API令牌` 进入页面后，点击API密钥栏 `Global API Key` 后面的 `查看` 即可获取
![cfapikey](https://img.johnsonran.cn/CF-DDNS/2.png)

脚本
---
#### 创建DNS Record
```bash
curl -X POST "https://api.cloudflare.com/client/v4/zones/8ee1be75cxxxx(你的Zone_ID)/dns_records" \
    -H "X-Auth-Email: xxxx(你的帐号的邮箱)" \
    -H "X-Auth-Key: 123456bdxxxx(你的Api_Key)" \
    -H "Content-Type: application/json" \
    --data '{"type":"A","name":"example.com（指定要创建记录的域名）","content":"127.0.0.1(指定A记录指向的IP)","ttl":120（指定TTL）,"proxied":false}'
#返回结果
{"result":{"id":"123456xxxx(这条创建的DNS记录的ID)","type":"A","name":"eg.example.com（创建记录的域名）","content":"127.0.0.1(A记录指向的IP)","proxiable":false,"proxied":false,"ttl":120,"locked":false,"zone_id":"123456cxxxx(你的Zone_ID)","zone_name":"example.com","modified_on":"2020-04-12T16:38:18.430924Z","created_on":"2020-04-12T16:38:18.430924Z","meta":{"auto_added":false}},"success":true（指命令执行成功）,"errors":[],"messages":[]}
```
#### 查看 DNS Record 列表
执行下面这个命令后，Shell 窗口中会列出你的帐号上的所有 `DNS` 记录（不仅仅是A记录）
在 `CloudFlare` ，每一条解析记录都有对应的一个固定的 `ID`
为了能够通过 `API` 修改解析记录，我们需要通过此步骤来获取这个 `ID`
```bash
curl -X GET "https://api.cloudflare.com/client/v4/zones/8ee1be75cxxxx(你的Zone_ID)/dns_records" \
    -H "X-Auth-Email: xxxx(你的帐号的邮箱)" \
    -H "X-Auth-Key: 123456bdxxxx(你的Api_Key)" \
    -H "Content-Type: application/json"
#返回结果
{"result":[{"id":"1234aca56xxxx(这条被查看记录的ID)","type":"A","name":"eg.example.com（域名）","content":"xx.xx.xx.xx(A记录指向的IP)","proxiable":true,"proxied":true（启用cloudflare反代与否的状态值）,"ttl":1（1 表示自动TTL）,"locked":false,"zone_id":"8ee1be75cxxxx(我的zone_id)","zone_name":"example.com（根域名）","modified_on":"2020-04-12T15:24:00.567936Z","created_on":"2020-04-12T15:24:00.567936Z","meta":{"auto_added":false}}}
```
其中的 `"id":"1234aca56xxxx"` 就是你需要的 DNS 记录的 `ID`

使脚本自动获取你的IP并让CloudFlare解析
---
当获取到了需要的一切,可按照以下编辑你的`.sh`脚本:
```bash
ipl=$(ifconfig (你拨号设备的名称，可通过ifconfig查看) | awk '/inet addr/{print substr($2,6)}') #获取你设备所使用的IP
ip=$(curl -s http://ipv4.icanhazip.com)
curl -k -X PUT "https://api.cloudflare.com/client/v4/zones/(你的Zone_ID)/dns_records/(这条被查看记录的ID)" \
         -H "X-Auth-Email: xxxx(你的帐号的邮箱)" \
         -H "X-Auth-Key: 123456bdxxxx(你的Api_Key)" \
         -H "Content-Type: application/json" \
         --data '{"type":"A","name":"你想使用的域名","content":"'${ipl}'","ttl":120,"proxied":false}'
```
为了方便了解该脚本的执行时间，以及CloudFlare所获取的IP,可自行测试以下命令(虽然我写的看起来像个笨比):
```bash
export ipl=$(ifconfig ppp0 | awk '/inet addr/{print substr($2,6)}') && bash (PATH_TO_SCRIPT) && echo `date +"%Y-%m-%d %H:%M:%S"` Done $ipl >> /tmp/ddns.txt
```
执行完后，即可使用 `tail -f /tmp/ddns.txt` 查看是否执行完成

如果想让脚本在某时段运行，具体可参照[此处](https://wangchujiang.com/linux-command/c/crontab.html)  
下面展示我配置的crontab：
```bash
0 0 * * * export ipl=$(ifconfig ppp0 | awk '/inet addr/{print substr($2,6)}') && bash /media/AiDisk_a1/ddns.sh && echo `date +"%Y-%m-%d %H:%M:%S"` Done $ipl >> /tmp/ddns.txt
```
该crontab表示每天零点自动执行脚本

---
教程就到此结束啦!  
本博客文章均为原创，转载请注明来源

---
