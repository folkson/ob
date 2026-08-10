[通过serv00部署frps服务](/archives/serv00-frps)
-----------------------------------------

3月 30, 2024

12 分钟阅读

零.延迟
====

低情商:卡 高情商:犹如国际级服务器hypixel般的体验 勉强能玩吧  
![](https://s2.loli.net/2024/03/30/Fq298BOEM5IgK4X.png)
  
网站倒是还算流畅的  
![](https://s2.loli.net/2024/03/30/zC28jLYSA9B7Huq.png)

一.前戏
====

1.设置端口
------

面板左侧 Port reservation→+Add port (如图)  
![](https://s2.loli.net/2024/03/30/rT8eAROh4jYkt7s.png)
  
一共可以创建三个,这里记作端口A B C,后面会用到

2.打开允许自己的软件
-----------

Additional services 选项卡中找到 Run your own applications 项目，将其设置为 Enabled 即可。如果不开启这一项，自己的用户目录下的所有文件都无法添加可执行权限。  
**如果你是在ssh之后才设置的这个选项,记得要重新ssh链接一遍,不然依然无法添加权限**

3.安装pm2
-------

在 SSH 连接 serv00 之后，直接使用一键脚本安装 pm2+全局调用

```null
bash <(curl -s https://raw.githubusercontent.com/k0baya/alist_repl/main/serv00/install-pm2.sh) && source ~/.bashrc

```

Copy

目前有bug,每次重新ssh不会自动读取.bashrc,所以每次ssh链接都要重新

```null
source ~./bashrc

```

Copy

当然你也可以路径调用

```null
~/.npm-global/bin/pm2

```

Copy

二.好戏开场了
=======

1.安装frp
-------

建议手动下载frp_版本号\_freebsd\_amd64.tar.gz后进去panel左侧的File manager后上传,使用wget可能会更新不及时

```null
wget https://github.com/fatedier/frp/releases/download/v0.56.0/frp_0.56.0_freebsd_amd64.tar.gz #截止2024/3/30版本为0.56.0

```

Copy

解压并重命名并赋予权限

```null
tar -zxvf frp_0.56.0_freebsd_amd64.tar.gz && mv frp_0.56.0_freebsd_amd64 frp && chmod 777 frp

```

Copy

接着在你的本地客户机上下载对应系统版本安装包解压备用

2.编辑服务端配置文件
-----------

这边依然建议在本地写好后上传(才不是因为我不会使用freebsd里的在线编辑呢)  
新建一个文本文档,把下面写入后保存为frps.toml

```null
bindPort = A
vhostHTTPPort = B #如果你不需要http(s)穿透可以只写上面那行
auth.token = "123456" #可选 链接密钥,为string类型,建议加上以防别人链接

```

Copy

然后上传至serv00的那个frp文件夹中(记得先把原来的frps.toml删掉)  
just like this  
![](https://s2.loli.net/2024/03/30/gfbQartwcdlexTh.png)

3.编辑客户端配置文件
-----------

这次要改名为frpc.toml

```null
serverAddr = "114.514.1919.810" #这串端口下面再细说
serverPort = A
auth.token = "123456" #你刚才在frps.toml中设置的密钥

#tcp/udp穿透实例
[[proxies]] #这里理论上可以改,但是改了就连接失败,建议别动
name = "abcde" #这里可以随便写
type = "tcp" #或者改成udp
localIP = "127.0.0.1" #本地地址
localPort = 25565 #本地要穿出去的端口
remotePort = C #如果你要udp,开放C端口时记得选择udp

#http(s)实例
[[proxies]]
name = "web"  
type = "http" 
localIP = "127.0.0.1"
localPort = 8080 #本地网站的端口
customDomains = ["example.serv00.net"] #这个域名下面细说

#你也可以同时穿透多个网站
[[proxies]]
name = "web2"
type = "http"
localPort = 8080 #不写localIP默认为127.0.0.1
customDomains = ["www.yourdomain2.com"] #与上面不同的域名

```

Copy

先说"serverAddr",这个端口在panel点击左侧的SSL,然后点WWW websites,下面的两个ip任选其一  
![](https://s2.loli.net/2024/03/30/PNFdtisbejkDcwM.png)
  
"customDomains"部分,panel左侧点击WWW Websites,然后点击Add new website,然后像我这样写  
![](https://s2.loli.net/2024/03/30/HiZpIfEzkcR9VDP.png)
  
然后放到一个文件夹内  
![](https://s2.loli.net/2024/03/30/UQzGjVkuArtOBD2.png)

### 3.5绑定自己的域名

①用argo隧道(不会)  
②手动绑定  
先在你的域名的dns记录里A解析到serverAddr中填的那个ip,然后在panel面板刚才创建域名中Domain写A解析的你自己的域名,其他不变点添加  
![](https://s2.loli.net/2024/03/31/u75zgr8QpbwsOUL.png)
  
然后在SSL→WWW websites→serverAddr中填的ip的那个manage→serverAddr  
Type选Certificate file的话下面就上传cf的证书,Certificate上传.pem Key上传.key,下面域名选择你自己的那个域名.没有证书就在Type选择Generate Let's Encrypt certificate(但是我会报错)(找到报错原因了,这里申请证书cf小黄云要先关闭)  
![](https://s2.loli.net/2024/03/31/KmpUO9rjMItesWH.png)
  
![](https://s2.loli.net/2024/03/31/a7OV23CWf1vZ9Lt.png)
  
[然后在上面的frpc.toml中的customDomains把域名改成你自己A解析的那个域名这样你就可以访问了.](www.theyouyi.site)  
4.测试  
先在ssh中开启frpc(先cd到frp目录)

```null
./frps -c ./frps.toml

```

Copy

然后在你的客户端中启动frpc(依然先cd到frp目录)

```null
./frpc -c ./frpc.toml #其他系统使用效果相同的命令,比如win:frpc -c frpc.toml

```

Copy

如果成功效果如图  
frps![](https://s2.loli.net/2024/03/30/nTRbiYsxOBw4Z6l.png)
  
frpc![](https://s2.loli.net/2024/03/30/xYZJIfuOLRCwP5a.png)
  
退出就ctrl+c就行

三.完结散花
======

1.开机自启
------

还记得之前装的pm2吗  
先在ssh退出frps  
然后使用PM2监控

```null
pm2 start -x ./frp/frps -n frp -- -c ./frp/frps.toml

```

Copy

在Panel中找到Cron jobs选项卡，使用Add cron job功能添加任务，Specify time选择After reboot，即为重启后运行。Form type选择Advanced，Command写：

```null
/home/你的用户名/.npm-global/bin/pm2 resurrect

```

Copy

然后在ssh保存pm2任务快照

```null
pm2 save

```

Copy

2.定时ssh
-------

### 方法①青龙面板(可能有时连不上,建议ssh链接时间调短一点)

添加Linux依赖 sshpass .添加定时任务 其他随意  
命令

```null
sshpass -p '密码' ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -tt 用户名@地址 "exit"

```

Copy

定时规则 1 1 1 * * .这样每月一号自动ssh

### 方法②自交

进入一个你喜欢的路径(根目录就行,方便就好)新建一个脚本

```null
cat > auto-renew.sh << EOF
#!/bin/bash

while true; do
  sshpass -p '密码' ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -tt 用户名@地址 "exit" &
  sleep 2592000  #30天为2592000秒 60*60*24*30=2592000 记得修改密码和地址
done
EOF
Copy

```

Copy

添加权限

```null
chmod +x auto-renew.sh

```

Copy

pm2自启动

```null
pm2 start ./auto-renew.sh && pm2 save

```

Copy

四.参考
====

[知乎专栏:一文搞懂frp内网穿透并搭建配置使用(部分内容已过时)](https://zhuanlan.zhihu.com/p/682491339)  
[Saika:Serv00搭建各种服务](https://blog.rappit.site/2024/01/27/serv00_logs/)  
[frp官方文档(谢谢,根本看不懂配置)](https://gofrp.org/zh-cn/)

评论

CommentWidget.init( "#comment-content-halo-run-Post-cc4ccf13-ef90-44b2-a815-3d2b47ab5d1e", { group: "content.halo.run", kind: "Post", name: "cc4ccf13-ef90-44b2-a815-3d2b47ab5d1e" } );

©

Built with [Halo](https://www.halo.run/)  
Theme **[Stack](https://github.com/jiewenhuang/halo-theme-stack)** designed by [Jimmy](https://jimmycai.com)

.image { display: inline-block; vertical-align: middle; margin-top: -6px; }          document.addEventListener("DOMContentLoaded", async function () { const extraLanguages = "".split(",").filter((x) => x); for (let i = 0; i < extraLanguages.length; i++) { const lang = extraLanguages\[i\]; if (lang) { await loadScript(`/plugins/PluginHighlightJS/assets/static/languages/${lang}.min.js`); } } document.querySelectorAll("pre code").forEach((el) => { hljs.highlightElement(el); }); }); function loadScript(url) { return new Promise(function (resolve, reject) { const script = document.createElement("script"); script.type = "text/javascript"; script.src = url; script.onload = resolve; script.onerror = reject; document.head.appendChild(script); }); }        [![](https://moe.one/view/img/ico64.png)
 萌ICP备20247520号](https://icp.gov.moe/?keyword=20247520)