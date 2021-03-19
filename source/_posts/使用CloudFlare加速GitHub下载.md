title: 使用CloudFlare加速GitHub下载
categories: 干货
tags: [Github,CloudFlare,CDN]
date: 2020-04-21 20:41:00
---
前言
===
GitHub 作为世界上最大的代码托管平台之一，国内当然有不少开发者和软件用户会使用 GitHub。但是由于 GitHub 主机的地理位置，国内访问 GitHub 的速度普遍较慢，更别说下载速度，惨不忍睹。对于没有能力魔法上网的用户确实是不太友好，有少部分地区甚至没有办法正常访问 GitHub。

最近发现了一个开源项目，能够通过 Cloudflare Workers 加速 GitHub 文件的下载，支持直接下载仓库和 Release，也可以单独下载 release 或者仓库内其中一个文件。自己尝试搭建了一下，没有能力的朋友可以通过下面的链接访问并下载 GitHub 上的文件。但是不要滥用，如果需要的话还是推荐自己搭建一下。  
https://github.johnsonran.workers.dev/  
![gh-preview](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CF-GitHub/gh-preview.png)  
尽管 Cloudflare 总体上在国内的速度也不太可观，但在某些地区的速度还是比较快的。个人测试了一下，如果使用魔法上网的话，用这个转发也会比直接在 GitHub 下载稍微快一些（日本的节点）。  
###原项目
这个`Worker`的`JavaScript`脚本是[hunsh](https://hunsh.net/)大佬写的，可以在`GitHub`上访问[hunshcn/gh-proxy](https://github.com/hunshcn/gh-proxy)并点击`Star`支持一下原作者୧(๑•̀⌄•́๑)૭  
###自己搭建
原作者在`README`中，进用四行文本就把这个程序的搭建方法写清楚了：
>首页：https://workers.cloudflare.com
注册，登陆，Start building，取一个子域名，Create a Worker。
复制`index.js`或`index2.js`到左侧代码框，Save and deploy。如果正常，右侧应显示首页。
`index.js`的clone走`github.com.cnpmjs.org`,`index2.js`的 clone 走你的`cf worker`，请自行选择
创建Worker
---
[notice]如果你已经拥有 Cloudflare Workers，那么创建一个新的 Worker 就可以跳过此步[/notice]  
访问 [CloudFlare](https://cloudflare.com/login)并登录你的账号，如果没有则点击`Sign up`进行注册。随后进入到 Dashboard 页面，点击右侧边栏的 Workers 选项。  
![cfwr](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CF-GitHub/cfwr.png)  
点击后会让你自定义一个域名,不过是`workers.dev`的子域,一会你创建 Workers 的时候,一个Worker占用一个四级域名。之后会让你选择一个方案（Plan）,免费的对于小型的网站来说足够了,每天允许一万次请求，每分钟限制 1000 次；如果是花费五刀使用所谓的`Unlimited`方案，每月允许`1000万`次请求,超出部分按$0.5/百万次请求的方式计费。

之后，点击`Create a Worker`按钮来创建一个新的 Worker,然后继续下一步。

添加脚本
---
原项目中有两个可选择的`JavaScript`脚本——`index.js`和`index2.js`。按照作者的意思，他们两个的主要区别是 Clone 的转发途径：  
>`index.js`的clone走`github.com.cnpmjs.org`,`index2.js`的clone走你的`CF Worker`  
这里就自行选择了，复制按照需求复制代码即可：
* index.js
```javascript
'use strict'
 
/**
 * static files (404.html, sw.js, conf.js)
 */
const ASSET_URL = 'https://hunshcn.github.io/gh-proxy'
 
/** @type {RequestInit} */
const PREFLIGHT_INIT = {
    status: 204,
    headers: new Headers({
        'access-control-allow-origin': '*',
        'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS',
        'access-control-max-age': '1728000',
    }),
}
 
/**
 * @param {any} body
 * @param {number} status
 * @param {Object<string, string>} headers
 */
function makeRes(body, status = 200, headers = {}) {
    headers['access-control-allow-origin'] = '*'
    return new Response(body, {status, headers})
}
 
 
/**
 * @param {string} urlStr
 */
function newUrl(urlStr) {
    try {
        return new URL(urlStr)
    } catch (err) {
        return null
    }
}
 
 
addEventListener('fetch', e => {
    const ret = fetchHandler(e)
        .catch(err => makeRes('cfworker error:\n' + err.stack, 502))
    e.respondWith(ret)
})
 
 
/**
 * @param {FetchEvent} e
 */
async function fetchHandler(e) {
    const req = e.request
    const urlStr = req.url
    const urlObj = new URL(urlStr)
    let path = urlObj.searchParams.get('q')
    if(path)
    {
        return Response.redirect('https://' + urlObj.host + '/' + path, 301)
    }
    // cfworker 会把路径中的 `//` 合并成 `/`
    path = urlObj.href.substr(urlObj.origin.length + 1).replace(/^https?:\/+/, 'https://')
    const exp = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:releases|archive)\/.*$/
    const exp2 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:blob)\/.*$/
    const exp3 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:info|git-upload-pack).*$/
    if (path.search(exp)===0) {
        return httpHandler(req, path)
    }else if(path.search(exp2)===0) {
        const newUrl = path.replace('/blob/', '@').replace(/^(?:https?:\/\/)?github\.com/, 'https://cdn.jsdelivr.net/gh')
        return Response.redirect(newUrl, 302)
    }else if (path.search(exp3)===0){
        const newUrl = path.replace(/^(?:https?:\/\/)?github\.com/, 'https://github.com.cnpmjs.org')
        return Response.redirect(newUrl, 302)
    } else {
        return fetch(ASSET_URL + path)
    }
}
 
 
/**
 * @param {Request} req
 * @param {string} pathname
 */
function httpHandler(req, pathname) {
    const reqHdrRaw = req.headers
 
    // preflight
    if (req.method === 'OPTIONS' &&
        reqHdrRaw.has('access-control-request-headers')
    ) {
        return new Response(null, PREFLIGHT_INIT)
    }
 
    let rawLen = ''
 
    const reqHdrNew = new Headers(reqHdrRaw)
 
    const refer = reqHdrNew.get('referer')
 
    let urlStr = pathname
    if (urlStr.startsWith('github')) {
        urlStr = 'https://' + urlStr
    }
    const urlObj = newUrl(urlStr)
 
    /** @type {RequestInit} */
    const reqInit = {
        method: req.method,
        headers: reqHdrNew,
        redirect: 'follow',
        body: req.body
    }
    return proxy(urlObj, reqInit, rawLen, 0)
}
 
 
/**
 *
 * @param {URL} urlObj
 * @param {RequestInit} reqInit
 */
async function proxy(urlObj, reqInit, rawLen) {
    const res = await fetch(urlObj.href, reqInit)
    const resHdrOld = res.headers
    const resHdrNew = new Headers(resHdrOld)
 
    // verify
    if (rawLen) {
        const newLen = resHdrOld.get('content-length') || ''
        const badLen = (rawLen !== newLen)
 
        if (badLen) {
            return makeRes(res.body, 400, {
                '--error': `bad len: ${newLen}, except: ${rawLen}`,
                'access-control-expose-headers': '--error',
            })
        }
    }
    const status = res.status
    resHdrNew.set('access-control-expose-headers', '*')
    resHdrNew.set('access-control-allow-origin', '*')
 
    resHdrNew.delete('content-security-policy')
    resHdrNew.delete('content-security-policy-report-only')
    resHdrNew.delete('clear-site-data')
 
    return new Response(res.body, {
        status,
        headers: resHdrNew,
    })
}
```
* index2.js
```javascript
'use strict'
 
/**
 * static files (404.html, sw.js, conf.js)
 */
const ASSET_URL = 'https://hunshcn.github.io/gh-proxy'
 
/** @type {RequestInit} */
const PREFLIGHT_INIT = {
    status: 204,
    headers: new Headers({
        'access-control-allow-origin': '*',
        'access-control-allow-methods': 'GET,POST,PUT,PATCH,TRACE,DELETE,HEAD,OPTIONS',
        'access-control-max-age': '1728000',
    }),
}
 
/**
 * @param {any} body
 * @param {number} status
 * @param {Object<string, string>} headers
 */
function makeRes(body, status = 200, headers = {}) {
    headers['access-control-allow-origin'] = '*'
    return new Response(body, {status, headers})
}
 
 
/**
 * @param {string} urlStr
 */
function newUrl(urlStr) {
    try {
        return new URL(urlStr)
    } catch (err) {
        return null
    }
}
 
 
addEventListener('fetch', e => {
    const ret = fetchHandler(e)
        .catch(err => makeRes('cfworker error:\n' + err.stack, 502))
    e.respondWith(ret)
})
 
 
/**
 * @param {FetchEvent} e
 */
async function fetchHandler(e) {
    const req = e.request
    const urlStr = req.url
    const urlObj = new URL(urlStr)
    let path = urlObj.searchParams.get('q')
    if(path)
    {
        return Response.redirect('https://' + urlObj.host + '/' + path, 301)
    }
    // cfworker 会把路径中的 `//` 合并成 `/`
    path = urlObj.href.substr(urlObj.origin.length + 1).replace(/^https?:\/+/, 'https://')
    const exp = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:releases|archive|info|git-upload-pack).*$/
    const exp2 = /^(?:https?:\/\/)?github\.com\/.+?\/.+?\/(?:blob)\/.*$/
    if (path.search(exp)===0) {
        return httpHandler(req, path)
    }else if(path.search(exp2)===0){
        const newUrl = path.replace('/blob/', '@').replace(/^(?:https?:\/\/)?github\.com/, 'https://cdn.jsdelivr.net/gh')
        return Response.redirect(newUrl, 302)
    } else {
        return fetch(ASSET_URL + path)
    }
}
 
 
/**
 * @param {Request} req
 * @param {string} pathname
 */
function httpHandler(req, pathname) {
    const reqHdrRaw = req.headers
 
    // preflight
    if (req.method === 'OPTIONS' &&
        reqHdrRaw.has('access-control-request-headers')
    ) {
        return new Response(null, PREFLIGHT_INIT)
    }
 
    let rawLen = ''
 
    const reqHdrNew = new Headers(reqHdrRaw)
 
    const refer = reqHdrNew.get('referer')
 
    let urlStr = pathname
    if (urlStr.startsWith('github')) {
        urlStr = 'https://' + urlStr
    }
    const urlObj = newUrl(urlStr)
 
    /** @type {RequestInit} */
    const reqInit = {
        method: req.method,
        headers: reqHdrNew,
        redirect: 'follow',
        body: req.body
    }
    return proxy(urlObj, reqInit, rawLen, 0)
}
 
 
/**
 *
 * @param {URL} urlObj
 * @param {RequestInit} reqInit
 */
async function proxy(urlObj, reqInit, rawLen) {
    const res = await fetch(urlObj.href, reqInit)
    const resHdrOld = res.headers
    const resHdrNew = new Headers(resHdrOld)
 
    // verify
    if (rawLen) {
        const newLen = resHdrOld.get('content-length') || ''
        const badLen = (rawLen !== newLen)
 
        if (badLen) {
            return makeRes(res.body, 400, {
                '--error': `bad len: ${newLen}, except: ${rawLen}`,
                'access-control-expose-headers': '--error',
            })
        }
    }
    const status = res.status
    resHdrNew.set('access-control-expose-headers', '*')
    resHdrNew.set('access-control-allow-origin', '*')
 
    resHdrNew.delete('content-security-policy')
    resHdrNew.delete('content-security-policy-report-only')
    resHdrNew.delete('clear-site-data')
 
    return new Response(res.body, {
        status,
        headers: resHdrNew,
    })
}
```
完成搭建
---
复制好之后，把他粘贴到Worker编辑页面左侧的`Script`栏里面，然后点击`Save and Deploy`。  
你可以在这个页面的左上角看到 worker 的域名（四级域名），你可以修改第四级的域名前缀，然后通过这个域名访问你的 worker，打开后应该是这个页面。  
![gh-preview](https://pan.johnsonran.cn/AliDrive/Blog-IMG/CF-GitHub/gh-preview.png)  
尝试使用
---
原作者给出的合法链接实例是这样的：
>分支源码：https://github.com/hunshcn/project/archive/master.zip
release 源码：https://github.com/hunshcn/project/archive/v0.1.0.tar.gz
release 文件：https://github.com/hunshcn/project/releases/download/v0.1.0/example.zip
分支文件：https://github.com/hunshcn/project/blob/master/filename

我感觉写得不太明确，容易撞墙，所以在这里列出一个简易的方式：**用鼠标右键复制你要下载的链接**。
例如仓库里的某一个文件，或者一个 Release 里的附件（Assets），直接右键复制链接。  
如果是要直接下载整个仓库（一个分支），或者一个分支里的文件，则是这样的格式：
```html
<!-- master 可以替换成其他分支的名字 -->
https://github.com/<user>/<repo>/archive/master.zip
https://github.com/<user>/<repo>/blob/master/<filename>
```
