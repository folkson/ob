

【复刻】在 Koyeb 上搭建 RSSHub
----------------------

AI 翻译

这篇文章通过AI由英语翻译成中文。查看原文

AI 生成的摘要

验证信用卡后，Koyeb每月提供$5.5的免费额度，可以部署两个nano服务。新建一个项目，填写镜像为docker.io/diygod/rsshub，标签为chromium-bundled。配置环境变量，选择Web服务类型，将端口改为1200。添加以下环境变量：NODE\_ENV为production，CACHE\_TYPE为memory，MEMORY\_MAX为64，ACCESS\_KEY为自定义的ACCESS密钥，HTTP\_BASIC\_AUTH\_NAME为limour，HTTP\_BASIC\_AUTH\_PASS为自定义的HTTP密码。自定义域名后，添加完DNS的CNAME后等待一段时间。更新项目后，点击Redeploy即可。使用方式有两种认证方式：认证方式1为使用[https://rss.limour.top/foreverblog/feeds?key=](https://rss.limour.top/foreverblog/feeds?key=)自定义ACCESS密钥进行认证，认证方式2为使用[https://limour](https://limour/):自定义HTTP密码@rss.limour.top/foreverblog/feeds进行认证。

验证信用卡后，Koyeb 每月提供 $5.5 的免费额度，可以部署两个 nano services。

新建一个项目[#](#user-content-新建一个项目)
-------------------------------

![](https://ipfs.crossbell.io/ipfs/bafkreia6o7kjqrl4fn3mkry3i45lejixfpaf4ljk253gsbboffovblfvfe?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

*   Image 填 `docker.io/diygod/rsshub`
*   Tag 填 `chromium-bundled`

配置环境变量[#](#user-content-配置环境变量)
-------------------------------

![](https://ipfs.crossbell.io/ipfs/bafkreicyr65godcxi43auyu7263a437lj7remq7p6s2jn4w3cx4ubzqkha?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

*   service type 选 Web
*   Port 改成 `1200`
*   添加以下环境变量

```
NODE_ENV: production
CACHE_TYPE: memory
MEMORY_MAX: 64
ACCESS_KEY: 自定义ACCESS密钥
HTTP_BASIC_AUTH_NAME: limour
HTTP_BASIC_AUTH_PASS: 自定义HTTP密码 
```

自定义域名[#](#user-content-自定义域名)
-----------------------------

![](https://ipfs.crossbell.io/ipfs/bafkreiddhgulu27gjwlantqanozmkr7z3eyomge7lucxesl4hz2ahbp76y?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

*   添加完 DNS 的 CNAME 后等一会儿

更新项目[#](#user-content-更新项目)
---------------------------

![](https://ipfs.crossbell.io/ipfs/bafkreihao5oom6dqdp65nv6sxucilbcmr6wh3xifbktxohdkl7ax4llrmi?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

*   点这个 `Redeploy` 就行

使用方式[#](#user-content-使用方式)
---------------------------

*   认证方式 1：`https://rss.limour.top/foreverblog/feeds?key=自定义ACCESS密钥`
*   认证方式 2：`https://limour:自定义HTTP密码@rss.limour.top/foreverblog/feeds`

2.  1\. 新建一个项目
3.  2\. 配置环境变量
4.  3\. 自定义域名
5.  4\. 更新项目
6.  5\. 使用方式

