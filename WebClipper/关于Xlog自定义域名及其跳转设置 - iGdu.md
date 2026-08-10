Xlog 的博主，云集各路高手，包括熟悉 Web3、区块链技术、加密钱包等技术大牛，也包括只是想分享内容的网络技术小白。

作为网络技术小白，在自定义 Xlog 博客的域名时，发现了网址跳转存在一些问题。Xlog 博客对于裸域跳转似乎没有教程。（本博文写完后，看到了[这个教程](https://lenband.com/Qimen-Dunjia)，但也没有裸域跳转的设置）我自己按照 ChatGPT 的教程并请教 Diygod 大佬，最终得以解决。此处予以记录，同时希望能够帮助其他 Xlog 用户。自定义域名的步骤如下：

1.Xlog 子域名配置和完善[#](#1xlog-子域名配置和完善)
-----------------------------------

或许，刚开始注册 Xlog 时，会随便写个 Xlog 子域名；但子域名是可以修改的，建议修改一个简短好记或令人印象深刻子域名。例如，我就将我的 Xlog 子域名修改为：[https://igdu.xlog.app](https://igdu.xlog.app/) 。修改方法：进入 Xlog 控制台，点击设置 —— 域名 —— 设置成你理想的域名，保存即可。

2\. 设置自定义域名[#](#2-设置自定义域名)
--------------------------

Xlog 免费提供了自定义域名方案。此处以我的 Xlog 博客[https://igdux.com](https://igdux.com/) 为例。当我想要将[https://igdu.xlog.app](https://igdu.xlog.app/) 的域名设置自定义域名[https://igdux.com](https://igdux.com/) 时，设置方法如下：进入 Xlog 控制台，点击设置 —— 域名 —— 自定义域名，输入你的域名，如 [igdux.com](https://igdux.com/igdux.com)（不要输入[https:// 或 http://](https://xn--http-475h//) ），此时，Xlog 会让你验证域名的主权。

如图：

![](https://ipfs.crossbell.io/ipfs/QmVEhCbMqTDz6rt9qNEC3sTUXeFC1FB16T4QnQkkPFX2VX?img-quality=75&img-format=auto&img-onerror=redirect&img-width=1920)

由于我的域名托管在大善人 Cloudflare 那里，因而，验证域名主权时，就需要到托管了 igdux.com 这个域名的 Cloudflare 账号服务中设置域名验证的值：包括一个 CNAME 值和一个 TXT 值。按照要求填好即可。

如图：

![](https://ipfs.crossbell.io/ipfs/QmPoqocmk6wNWHtBZLnDEfMUzcKwwoJbs8BwHtL2B5ij78?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

如果设置好了，稍等片刻，即可通过自定义域名（也可以通过原分配的 xlog.app 子域名）访问你的博客。

3\. 设置 301 裸域跳转[#](#3-设置-301-裸域跳转)
----------------------------------

所谓裸域跳转，即当访问 www.igdux.com 域名时，会跳转到不带 www 的域名 igdux.com。而 301 即设置永久跳转，不是临时的，方便探索引擎识别和标记。

通常博客平台后台就有域名跳转的选项，如 Google Blogger（注意：Blogger 默认是从不带 www 的域名跳转到带 www 的域名）。而 Xlog 后台没有设置域名跳转选项。此时，就需要你到域名托管商如 Cloudflare 那里设置域名跳转。

### 3.1 设置 www 域名解析[#](#31-设置-www-域名解析)

www 域名解析的设置方法有两种，**只能二选一**。Diygod 选择的是方法一，我测试方法二也成功了。

设置方法一：加一个带 www. 的域名的 IP 解析，A 记录值可以填写任意的标准 IP 值（不一定是 Cloudflare 的 IP），如 192.0.2.1 或 108.162.192.233。设置好之后，如图所示：

![](https://ipfs.crossbell.io/ipfs/QmdmrqiLDVFhPkZnxwederyKQjW6XPr6gtT2ZFqnqvcK5r?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

设置方法二：加一个带 www. 的域名的 CNAME 解析，解析的值可以是你的 Xlog.app 的子域名。以我的为例：

![](https://ipfs.crossbell.io/ipfs/QmXxxhrnbHH9jVyZkykD6WCApnpusykHLDRfKrmvK3pDoc?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

### 3.2 设置 301 跳转规则[#](#32-设置-301-跳转规则)

在 Cloudflare 账户下，选择你绑定了 xlog.app 子域名的自定义域名，例如，我的自定义域名为 igdux.com。选中域名 —— 规则 —— 页面规则，设置如图：

![](https://ipfs.crossbell.io/ipfs/QmTXNEuPfZAqgBRJZWfHk4uiXzETYU8z42oDyqD5yoKh3N?img-quality=75&img-format=auto&img-onerror=redirect&img-width=3840)

URL：你的域名 /* (如 google.com/*)

Pick a Setting: Forward URL； Select Status Code：301

Enter Destination URL（目标网址）：[https:// 你的域名 /$1](https://xn--6qqv7i2xdt95b/$1)

其中 /\* 和 /$1 都是网络技术通用语言，按图设置即可。

当你设置好 3.1 和 3.2 的步骤之后，你就可以通过你的域名访问你博客，同时，还可以实现带裸域跳转：以我的域名为例，当我访问[https://www.igdux.com](https://www.igdux.com/) 时，会自动跳转到[https://igdux.com](https://igdux.com/) 。如果设置了 3.2 域名 301 跳转规则，没有设置 3.1 设置 www 域名解析，就会出现 HTTP 502 错误。

说明：[#](#说明)
-----------

Google Blogger 后台虽然带域名跳转，不过，Blogger 默认是从不带 www 的域名跳转到带 www 的域名；如果 Blogger 博主想设置裸域跳转，在 Blogger 后台就不要设置从不带 www 的域名跳转带 www 域名选项。同时，参照本文的设定，实现域名的裸域 301 跳转。

致谢：[#](#致谢)
-----------

感谢[Diygod](https://diygod.cc/)和 ChatGPT。出现问题后，在 ChatGPT 的帮助下和 Diygod 大佬的指点下，成功地解决了域名跳转问题。

————

敬请关注：[#](#敬请关注)
---------------

关于 Xlog，之前写的几篇文章，敬请关注：

*   [我为什么选择 xlog 博客？](https://igdux.com/xlog)
    
*   [谈谈我对 xLog 的理解](https://igdux.com/xlog1)
    
*   [Xlog，让你 5 分钟便可免费拥有一个现代风格的博客](https://igdux.com/xlog2)