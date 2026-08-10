从[Cloudflare 优选域名推荐和接入教程, 让Cloudflare不再是大号减速器](https://www.nodeseek.com/post-478198-1)继续讨论：  
上篇教程有人说看不明白, 所以这里写一个更详细的教程

一. 啥是优选, 对我网站的优化明显吗?
--------------------

由于众所周知的原因, Cloudflare 的大部分节点在高峰期的表现的不堪入目. 所以引申出了节点优选. 通常是把特定区域的流量引导至我们想要的 PoP(例如 HKG/NRT/SIN). 优选的节点通常会有更优的线路和性能. 优选的原理如下:

```
用户 (海外或大陆)
         |
         v
第三方 DNS 服务商 (例如阿里云, 华为云的解析服务)
         |
         v
├── 海外用户返回 Cloudflare 分配的IP
└── 大陆用户返回 自定义的 Cloudflare IP
         |
         v
用户访问对应节点, 实现优化

```

至于效果是否明显, 我觉得还是挺明显的. 大部分前端文件都可以被Cloudflare Edge缓存, 最明显的效果就是静态资源和前端页面加载的更快了, 用户只需要等待Cloudflare Edge返回api请求即可.

二. 如何为我的网站配置优选?
---------------

从本段开始就是正式教程了. 只要你按照教程一步一步做, 我就不信还有人能看不明白. (要是还看不明白我就真没招了)

> 情景带入:  
> 我是一个新手, 我的 **主域名 [sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fsin.fan) 在 Cloudflare 上托管**, 但是由于 Cloudflare 太卡了, 我真受不了, 所以我决定让用户从我的**子域名 [abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com)** 访问我的网站, **[abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com)** 是我要进行优选并**接入到 [abab.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.sin.fan)** 的域名.

### 1\. 检查条件

必要条件, 没有的可以不用往下看了

*   一个有支付方式的Cloudflare 账号 (据说此步骤可以卡bug跳过检查, 可自行查找解决方法)
*   至少一个可用的主域绑定在Cloudflare 上, 至少一个可添加多个解析的子域名(不同主域). (单域名接入请看我的另一个优选教程)
*   一个支持分地区解析的DNS服务商 (例: 阿里云, 腾讯云, 华为云...)

### 2\. 基础配置

**A. 配置回源**  
根据情景带入, 我的主域是 [sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fsin.fan), 回源域名为 [abab.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.sin.fan). 所以我先为回源添加一个解析:  
![](https://img.sin.fan/saas-pro/%E9%85%8D%E7%BD%AE%E5%9B%9E%E6%BA%90.png)

**B. 配置自定义主机名**  
依据情景带入和上一步添加的回源解析, 我的回源是 [abab.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.sin.fan); 用户要访问的域名是 [abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com). 根据这些信息, 进行配置:

I. 转到配置页面  
查看侧边栏, 点击自定义主机名  
![](https://img.sin.fan/saas-pro/%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%BB%E6%9C%BA%E5%90%8D%E4%BE%A7%E8%BE%B9%E6%A0%8F.png)

II. 配置默认回源  
在主页面配置回源, 回源就是你在步骤A中添加的解析. 我设置的是 [abab.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.sin.fan). 你应该替换为你自己配置的回源.  
![](https://img.sin.fan/saas-pro/%E9%85%8D%E7%BD%AE%E9%BB%98%E8%AE%A4%E5%9B%9E%E6%BA%90.png)

III. 添加自定义主机名  
现在回源配置完毕, 开始添加自定义主机名.  
根据情景带入, 用户应该用 [abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com) 访问我的网站.

首先点击 "添加自定义主机名按钮":  
![](https://img.sin.fan/saas-pro/%E7%82%B9%E5%87%BB%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%BB%E6%9C%BA%E5%90%8D%E6%8C%89%E9%92%AE.png)
  
随后在新的页面中完成添加:  
![](https://img.sin.fan/saas-pro/%E5%BC%80%E5%A7%8B%E6%B7%BB%E5%8A%A0%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%BB%E6%9C%BA%E5%90%8D.png)
  
最后点击添加自定义主机名按钮保存设置.

至此, 你已经完成了本步骤: 基础配置.

### 3\. 开始接入主机名并完成优选

还记得你在步骤 2.B.III 中配置的自定义主机名吗? 当你成功配置后, 自定义主机名主页会有个类似的卡片:  
![](https://img.sin.fan/saas-pro/%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%BB%E6%9C%BA%E5%90%8D%E5%8D%A1%E7%89%87.png)
  
待会需要你根据卡片中的内容, 进行设置

**A.接入支持分地区解析的服务商**  
根据情景带入, [abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com) 是我要接入的域名.  
这里用阿里云作为示例, 你应该根据你使用的服务商自行调整:

I. 添加域名  
将 [abab.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.rikka-ai.com) 添加到阿里云中, 你应该会收到如下提示:  
![](https://img.sin.fan/saas-pro/%E9%98%BF%E9%87%8C%E4%BA%91%E6%8F%90%E7%A4%BA.png)
  
点击 `TXT授权验证` 会打开一个新的卡片:  
![](https://img.sin.fan/saas-pro/%E9%98%BF%E9%87%8C%E4%BA%91%E9%AA%8C%E8%AF%81.png)

根据卡片的描述, 我们需要给 [alidnscheck.rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Falidnscheck.rikka-ai.com) 添加TXT解析,  
这里需要你转到原DNS服务商添加解析, 例如我的 [rikka-ai.com](https://www.nodeseek.com/jump?to=http%3A%2F%2Frikka-ai.com) 托管在 Cloudflare 上, 因此我需要到 Cloudflare 上添加解析:  
![](https://img.sin.fan/saas-pro/%E5%AE%8C%E6%88%90%E9%98%BF%E9%87%8C%E4%BA%91%E5%9F%9F%E5%90%8D%E9%AA%8C%E8%AF%81.png)
  
现在回到阿里云, 点击验证. 等待验证通过.

验证通过后, 进入配置页, 查看阿里云为你分配的名称服务器:  
![](https://img.sin.fan/saas-pro/%E6%9F%A5%E7%9C%8B%E5%88%86%E9%85%8D%E7%9A%84NS%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

转到原服务商, 为子域添加NS解析:  
![](https://img.sin.fan/saas-pro/%E6%B7%BB%E5%8A%A0NS%E8%A7%A3%E6%9E%90.png)
  
添加完成后, 回到阿里云. 刷新页面后应该能看见 `域名的DNS信息配置正确。` 提示.  
![](https://img.sin.fan/saas-pro/NS%E9%85%8D%E7%BD%AE%E6%AD%A3%E7%A1%AE.png)

II. 添加解析  
根据自定义主机名卡片中的要求, 添加以下解析:  
TXT:  
`_acme-challenge`:  
![](https://img.sin.fan/saas-pro/_acme-challenge.png)
  
`_cf-custom-hostname`:  
![](https://img.sin.fan/saas-pro/_cf-custom-hostname.png)

接下来是CNAME解析, 一共两条  
第一条为 Cloudflare 要求你设置的回源解析, 根据情景导入, 我的回源是 [abab.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fabab.sin.fan), 因此我先添加一条**解析请求来源为境外**的 CNAME 解析:  
![](https://img.sin.fan/saas-pro/%E9%98%BF%E9%87%8C%E4%BA%91%E9%85%8D%E7%BD%AE%E5%9B%9E%E6%BA%90.png)
  
第二条为 **为国内流量提供优化的** CNAME 解析, 因此解析请求来源设置为中国地区, 内容为任意的优选域名, 这里我推荐 **[saas.sin.fan](https://www.nodeseek.com/jump?to=http%3A%2F%2Fsaas.sin.fan)**:  
![](https://img.sin.fan/saas-pro/%E9%85%8D%E7%BD%AE%E4%BC%98%E9%80%89.png)

III. 检查是否生效  
现在回到 Cloudflare 的自定义主机名页面, 点击刷新. 如果两个待定均变为有效, 代表你的所有设置均是正确的! 至此本篇教程已经结束.  
![](https://img.sin.fan/saas-pro/%E5%AE%8C%E6%88%90%E6%9C%80%E7%BB%88%E9%85%8D%E7%BD%AE.png)

求帮顶 ![](https://www.nodeseek.com/static/image/sticker/xhj/024.png)