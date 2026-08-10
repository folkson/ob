AI摘要

本文介绍了通过Cloudflare Tunnel结合优选IP实现国内未备案服务器访问的配置方法。教程强调需准备同一账号下的两个域名，并详细说明了Tunnel设置、自定义主机名配置、DNS记录修改及验证步骤。同时警告该操作违反Cloudflare服务条款，存在封号风险，建议仅小规模自用并做好账号隔离。

本文介绍了通过Cloudflare Tunnel结合优选IP实现国内未备案服务器访问的配置方法。教程强调需准备同一账号

此内容根据文章生成，仅用于文章内容的解释与总结

[](#cl-1)Cloudflare Tunnel配置教程
------------------------------

*   请参考[Cloudflare Tunnel 实现国内未备案服务器通过域名访问 - HuoHuas001's Blog](https://blog.txssb.cn/archives/69/)

[](#cl-2)以下内容搬运自Linux Do的[ccxkai](https://linux.do/u/ccxkai)
------------------------------------------------------------

[](#cl-3)封号警告！！！
----------------

*   Cloudflare更新服务条款明确说明不得将其作为代理使用，也不得优选IP。
*   简单来说，建议仅小规模自用。
*   请做好账号隔离，并随时做好被封号的准备。
    

下面是正文。

[](#cl-4)前期准备
-------------

1.  托管到同一Cloudflare账号下的两个域名，包含一个主力域名（用户访问的域名）和一个辅助域名。
    
    *   **重要提示：**  两个域名必须在同一账号下，否则可能出现问题。  
        [![](https://blog.txssb.cn/usr/uploads/2026/02/3967051781.png)
        ](https://blog.txssb.cn/usr/uploads/2026/02/3967051781.png)
2.  主力域名已经创建好Tunnel。本教程侧重于Tunnel优选配置，因此不对Tunnel的创建过程展开。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/460266955.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/460266955.png)

[](#cl-5)分段教学
-------------

### [](#cl-6)Tunnel 部分设置

1.  点击“添加公共主机名”，对同一个服务创建辅助域名的主机名。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/1741918099.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/1741918099.png)
2.  配置主机名：
    
    *   主力域名（上图所示）
    *   辅助域名（下图所示）  
        [![](https://blog.txssb.cn/usr/uploads/2026/02/1269215689.png)
        ](https://blog.txssb.cn/usr/uploads/2026/02/1269215689.png)

### [](#cl-7)公共主机名设置

1.  进入 **辅助域名** -> **SSL/TLS** -> **自定义主机名**。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/1529924402.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/1529924402.png)
2.  点击“启动Cloudflare for SaaS”。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/3401100498.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/3401100498.png)
3.  **首次启动会要求绑卡，绑卡完成后继续。** 
4.  在“回退源”一栏填入刚刚绑定隧道的辅助域名，然后点击“添加回退源”。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/1487375983.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/1487375983.png)
5.  刷新页面，确保回退源状态显示“有效”。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/2023519842.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/2023519842.png)
6.  接下来点击“添加自定义主机名”。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/3607021101.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/3607021101.png)
7.  填入**主力域名**，其余保持默认。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/4277974184.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/4277974184.png)
8.  保存后刷新，此时会出现“待验证”状态。切换到主力域名的DNS页面进行配置。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/1872365150.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/1872365150.png)
9.  创建两个 `TXT` 记录，分别填入Cloudflare提供的两组值。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/3643520592.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/3643520592.png)  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/368578002.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/368578002.png)
10.  返回辅助域名的自定义主机名页面，刷新确保证书状态显示“有效”即可。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/3516351510.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/3516351510.png)

### [](#cl-8)配置辅助域名的DNS

1.  创建一个 `CNAME` 记录，指向社区维护的优选域名。
2.  这里提供几个示例优选域名：
    
    *   CM大佬：`cf.090227.xyz`
    *   微测网：`cloudflare.182682.xyz`
    *   移动快乐：`cf-saas.asia`
    *   可以看到解析出了不少的IP：  
        [![](https://blog.txssb.cn/usr/uploads/2026/02/3258161981.png)
        ](https://blog.txssb.cn/usr/uploads/2026/02/3258161981.png)
3.  选择一个你认为合适的优选域名作为 `CNAME` 记录的值，**注意要关闭小黄云（代理状态设为“仅DNS”）**。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/842598445.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/842598445.png)

### [](#cl-9)配置主域名的DNS

1.  切换到主域名的DNS页面，修改在创建隧道时自动生成的 `CNAME` 记录的值。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/134522106.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/134522106.png)
2.  使其指向刚才创建的加速辅助域名，如图所示。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/3304343407.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/3304343407.png)
3.  再添加一个 `CNAME` 记录。切换到辅助域名的自定义主机名页面，拉到最下面，找到“自定义主机名的DCV委派”。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/589324991.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/589324991.png)
4.  将里面的内容填入主域名的 `CNAME` 记录里。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/289309352.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/289309352.png)
5.  将 `CNAME` 名称修改为，例如 `main`。**小黄云记得关闭（代理状态设为“仅DNS”）**。
6.  此时再切换到辅助域名的自定义主机名页面，会发现主机名状态已经更新为“有效”了。  
    [![](https://blog.txssb.cn/usr/uploads/2026/02/914362471.png)
    ](https://blog.txssb.cn/usr/uploads/2026/02/914362471.png)

### [](#cl-10)验证环节

首先可以看到，成功解析出了很多IP。  
[![](https://blog.txssb.cn/usr/uploads/2026/02/701855113.png)
](https://blog.txssb.cn/usr/uploads/2026/02/701855113.png)