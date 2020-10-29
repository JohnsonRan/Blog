title: CloudFlair - 获取藏在 Cloudflare 后的源站IP
categories: 实用工具
tags: [CloudFlare,CDN]
date: 2020-09-07 20:18:00
---
具体原理可以查看[原作者的文章](https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/)
CloudFlair 开源地址：[Github](https://github.com/christophetd/CloudFlair)
这个工具是用pytohn编写的，兼容`python2.7`和`3.5`

#配置
1. 在 https://censys.io/register 注册一个账号 **(FREE)**
2. 访问https://censys.io/account/api 页面，获取账号的`API ID`和`API secret`，并设置到环境变量中
```bash
$ export CENSYS_API_ID='<here-is-your-api-id>'
$ export CENSYS_API_SECRET='<here-is-your-api-secret>'
```

3. 克隆整个仓库
```bash
$ git clone https://github.com/christophetd/cloudflair.git
```
4. 安装依赖
```bash
$ cd cloudflair
$ pip install -r requirements.txt
```

5. 运行CloudFair
```bash
$ python cloudflair.py example.com
```

#用法
```bash
$ python cloudflair.py --help
usage: cloudflair.py [-h] [-o OUTPUT_FILE] [--censys-api-id CENSYS_API_ID] [--censys-api-secret CENSYS_API_SECRET]
                     domain

positional arguments:
  domain                The domain to scan

optional arguments:
  -h, --help            show this help message and exit
  -o OUTPUT_FILE, --output OUTPUT_FILE
                        A file to output likely origin servers to (default: None)
  --censys-api-id CENSYS_API_ID
                        Censys API ID. Can also be defined using the CENSYS_API_ID environment variable (default:
                        None)
  --censys-api-secret CENSYS_API_SECRET
                        Censys API secret. Can also be defined using the CENSYS_API_SECRET environment variable
                        (default: None)
```
python cloudflair.py 后面跟上需要查找的域名即可

一些可选参数解释：
```bash
-h, --help  查看帮助  
  -o OUTPUT_FILE, --output OUTPUT_FILE
                        输出可能的源站服务器地址到文件中  
  --censys-api-id CENSYS_API_ID
                        手动指定Censys API ID  
  --censys-api-secret CENSYS_API_SECRET
                        手动指定Censys API secret
```

