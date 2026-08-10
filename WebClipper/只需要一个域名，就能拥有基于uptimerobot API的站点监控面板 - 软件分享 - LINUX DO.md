> 感谢 JIeJaitt 佬的指路：[【求助】如何榨干render免费的部署 - #8，来自 JIeJaitt 25](https://linux.do/t/topic/48227/8)  
> 教程参考自：[https://mari0er.club/post/monitor.html/ 14](https://mari0er.club/post/monitor.html/)  
> 使用的开源项目：[GitHub - imsyy/site-status: 📺 一个基于 UptimeRobot API 的在线状态面板 | 站点监测 | 状态检测 | An online status panel based on the UptimeRobot API | UptimeRobot, status, site 61](https://github.com/imsyy/site-status)

监控面板有很多种，我之前就分享过两个，今天这个是使用 Uptime Robot服务的。  
原教程在部署过程中有一些坑，我踩完后把经验分享一下。

[](#uptime-robot-1)注册 Uptime Robot 账号
-------------------------------------

只需要邮箱验证即可

### [](#h-2)添加站点监控

*   **New Monitor**
*   可以看到支持Http(s)、关键字、端口、Ping等监控方式，根据需求自行进行选择  
    
    [![](https://cdn.linux.do/uploads/default/optimized/3X/c/5/c50597d2ef69be6772fac4d4db4d34e194fe2459_2_411x500.png)
    ](https://cdn.linux.do/uploads/default/original/3X/c/5/c50597d2ef69be6772fac4d4db4d34e194fe2459.png "image")
    

### [](#api-keys-3)获取API Keys

### [](#fork-4)fork面板项目并修改配置

*   听说这个二开面板更加好看。点击fork，到你的账号下  
    [GitHub - imsyy/site-status: 📺 一个基于 UptimeRobot API 的在线状态面板 | 站点监测 | 状态检测 | An online status panel based on the UptimeRobot API | UptimeRobot, status, site 61](https://github.com/imsyy/site-status)
*   编辑 .env 配置文件  
    VITE\_API\_KEY = 刚刚记下的Read-Only Key
*   提交编辑

### [](#cloudflare-worker-5)通过cloudflare worker部署面板

*   主页选择 Workers 和 Pages
*   创建应用  
    ![](https://cdn.linux.do/uploads/default/original/3X/6/d/6d681bf0c80713ed3b3dfb14aef5886ba2f64f77.png)
    
*   选择pages并连接之前fork的项目  
    
    [![](https://cdn.linux.do/uploads/default/optimized/3X/1/d/1def5f22b4dbadfeb5f3f12be7f24341d7ad8db5_2_542x499.png)
    ](https://cdn.linux.do/uploads/default/original/3X/1/d/1def5f22b4dbadfeb5f3f12be7f24341d7ad8db5.png "image")
    
*   按照下图 保存并部署，这里注意，**构建输出目录需要更改为dist**  
    
    [![](https://cdn.linux.do/uploads/default/optimized/3X/9/3/933848774eeb7796a0bde20425e37a25166bfd08_2_385x500.png)
    ](https://cdn.linux.do/uploads/default/original/3X/9/3/933848774eeb7796a0bde20425e37a25166bfd08.png "image")
    

### [](#h-6)修改自定义域

*   部署成功后就可以换自己的域名了  
    
    [![](https://cdn.linux.do/uploads/default/original/3X/f/2/f25415f9db2125caa51e7ba641557df314ab25c4.png)
    ](https://cdn.linux.do/uploads/default/original/3X/f/2/f25415f9db2125caa51e7ba641557df314ab25c4.png "image")
    
*   输入自己的域名加前缀，自动就会把CNAME添加到域名的DNS解析中
*   然后我又去DNS中把解析的小黄云给关了

### [](#uptimerobot-7)代理uptimerobot

*   听过存在API跨域问题，所以需要用worker代理一下
*   创建一个worker脚本：uptimerobot
*   编辑代码：

```null
const handleRequest = async ({ request }) => {
  let url = new URL(request.url);
  let response = await fetch('https://api.uptimerobot.com' + url.pathname, request);
  response = new Response(response.body, response);
  response.headers.set('Access-Control-Allow-Origin', '*');
  response.headers.set('Access-Control-Allow-Methods', '*');
  response.headers.set('Access-Control-Allow-Credentials', 'true');
  response.headers.set('Access-Control-Allow-Headers', 'Content-Type,Access-Token');
  response.headers.set('Access-Control-Expose-Headers', '*');
  return response;
}

addEventListener('fetch', (event) => {
  event.respondWith(handleRequest(event));
});

```

*   部署后应该可以直接用得到的dev域名作为代理url
*   但是我还是习惯性的去弄了一个dns解析 + worker路由
*   添加一条DNS A记录，前缀我写了uptimerobot，IP从 [http://ip.flares.cloud/ 16](http://ip.flares.cloud/) 或者 [https://stock.hostmonit.com/CloudFlareYes 8](https://stock.hostmonit.com/CloudFlareYes) 选一个延迟低的
*   点击workers路由，添加路由，输入 `uptimerobot. 域名/*` ，worker选刚刚创建的uptimerobot

### [](#h-8)配置代理

*   回到你之前fork的项目仓库，编辑.env文件
*   VITE\_GLOBAL\_API = “[https://uptimerobot.域名/v2/getMonitors 4](https://uptimerobot.xn--eqrt2g/v2/getMonitors)” # 第三方反代
*   提交
*   这时你会发现在cf worker中的面板应用自动重新部署了  
    
    [![](https://cdn.linux.do/uploads/default/original/3X/4/4/441af5321e163f49678d68d88b025f3ce74a4669.png)
    ](https://cdn.linux.do/uploads/default/original/3X/4/4/441af5321e163f49678d68d88b025f3ce74a4669.png "image")
    

### [](#api-url-9)测试 API 和 代理url

API 是实时请求更新的，可以通过post请求看看返回值

```null
curl -X POST  https://api.uptimerobot.com/v2/getMonitors?api_key=你的Read-Only Key

```

这里你也可以把[https://api.uptimerobot.com换成你之前的代理域名测试一下 7](https://api.uptimerobot.xn--com-s18d1b4wo9as2aw6qbyem5mny4asud9y9bmdvv9iys8g/)

十分钟就能复刻 ![](https://cdn.linux.do/images/emoji/apple/wink.png?v=12)

不错 都不需要服务器了 ![](https://cdn.linux.do/images/emoji/apple/+1.png?v=12)

话说大佬有没有那种可以分组的监控面板，这种都是挂在一个界面，服务多了看起来有点乱七八糟的

[![](https://cdn.linux.do/uploads/default/original/3X/c/5/c5061543056a4a5de4ae1cd692d164ceca6da090.png)
](https://cdn.linux.do/uploads/default/original/3X/c/5/c5061543056a4a5de4ae1cd692d164ceca6da090.png "image")

请问这种情况怎么解决

[![](https://cdn.linux.do/uploads/default/optimized/3X/4/8/48461022e8d8b215a7f8e3f460c417fd3b4f95e6_2_643x500.png)
](https://cdn.linux.do/uploads/default/original/3X/4/8/48461022e8d8b215a7f8e3f460c417fd3b4f95e6.png "image")

大佬，我uptimerobot添加 A记录，小黄云要开启吗？我用 cf 自带的 dev 没有报跨域，自定义域就跨域了

好像他那个网站自带pages  

[![](https://cdn.linux.do/uploads/default/optimized/3X/0/3/03047d77b22c75c46e0cc1126a3af70850131a49_2_422x500.jpeg)
](https://cdn.linux.do/uploads/default/original/3X/0/3/03047d77b22c75c46e0cc1126a3af70850131a49.jpeg "IMG_20240514_144720")

还自带软件，不错不错

[![](https://cdn.linux.do/uploads/default/optimized/3X/0/0/0098354ef41550d86508a823a97c5a91a2cc0a18_2_244x500.jpeg)
](https://cdn.linux.do/uploads/default/original/3X/0/0/0098354ef41550d86508a823a97c5a91a2cc0a18.jpeg "IMG_20240514_145739")

谢谢大佬。  
关键步骤都在，简明扼要。已经搭建了一个。

不建议直接编辑.env文件，可在CF的Workers-设置-环境变量里面，配置环境变量然后重新部署。

Fork的仓库可以安装[pull插件 4](https://github.com/apps/pull)，上游仓库更新后，即可自动更新。