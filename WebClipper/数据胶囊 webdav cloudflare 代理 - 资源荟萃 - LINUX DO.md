从[中科院推出 20GB 免费云存储：支持 S3 / WebDAV 协议，附挂载为本地硬盘教程](https://linux.do/t/topic/1793719/30)继续讨论：

其实很早之前就写好了（AI真好用），但是后来忙别的项目就忘记了

> 重发一下之前所说的
> 
> 数据胶囊限制了ua，WebDav可以用cf搭个worker做中转  
> s3也尝试了，但是局限性比较大：  
> 需要在服务器端保存用户机密，否则只能以**明文**方式传输

源码（gist）：

[gist.github.com](https://gist.github.com/taozhiyu/309572f9e3213692735dfa672c846337)

#### [https://gist.github.com/taozhiyu/309572f9e3213692735dfa672c846337](https://gist.github.com/taozhiyu/309572f9e3213692735dfa672c846337)

##### cloudflare-webdav-proxy.cloudflare.js

```
/****
 * cloudflare-webdav-proxy
 * --------------------------------------------------
 * 功能：
 * 1. 将 /dav 前缀下的请求透明代理到下文正文第一行的 plantform
 * 2. 自动重写 Host / User-Agent 请求头
 * 3. 对 XML 响应内容中的域名进行回写，保证客户端可正常访问
 * 4. 根路径返回服务状态信息
 *
 * 路由：
```

此文件已被截断。 [显示原始文件](https://gist.github.com/taozhiyu/309572f9e3213692735dfa672c846337)

开箱即用（cloudflare worker）：

| 地址 | [https://webdav.taoproxy.dpdns.org/dav](https://webdav.taoproxy.dpdns.org/dav) |
| --- | --- |
| 用户名 | 你的webdav用户名 |
| 密码 | 你的webdav密码 |

顺便附赠一个 S3 兼容 cloudflare 代理客户端项目

[github.com](https://github.com/backblaze-b2-samples/cloudflare-b2-proxy)

![](https://cdn3.ldstatic.com/optimized/4X/8/9/8/898b7fe920b699781767bb4c2a7995d18e8a262a_2_690x344.png)

### [GitHub - backblaze-b2-samples/cloudflare-b2-proxy: Proxy Backblaze S3 compatible API requests,...](https://github.com/backblaze-b2-samples/cloudflare-b2-proxy)

Proxy Backblaze S3 compatible API requests, optionally sending notifications to a webhook