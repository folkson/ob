[国内无法访问下载Docker镜像的多种解决方案 - 如有乐享](https://51.ruyo.net/18687.html) 

 [国内无法访问下载Docker镜像的多种解决方案 - 如有乐享](https://51.ruyo.net/18687.html) 

 [国内无法访问下载Docker镜像的多种解决方案 - 如有乐享](https://51.ruyo.net/18687.html) 

 ![](https://b.bdstatic.com/comment/vu5WqQNgL2UEadZ0YSzjrg10ef8e7db84bf6021c9012fba2324888.png)

2023年5月中旬， [hub.docker.com](https://51.ruyo.net/hub.docker.com)  "不知" 何种原因国内均无法正常访问了。当时只是官网不能访问，但是不影响pull镜像。

2024年6月，国内几家Docker Hub 镜像服务平台均被要求下架，停止服务。

这里不讨论其原因！这里分享几个方法，帮助童鞋如何正常的拉取Docker镜像！

如果童鞋你有更多方式，请留言分享给大家哈~

更多关于：[Docker](https://51.ruyo.net/tag/docker) / [Docker常用命令](https://51.ruyo.net/16766.html) / [Docker Hub](https://51.ruyo.net/tag/docker-hub)

零门槛
---

### 境外镜像

**优点：不需大量修改，只需几个命令**

**缺点：网络可能慢或者不稳定**

有一个难兄难弟的国家：俄罗斯，被美国制裁无法顺利访问Docker Hub。这里可以借助俄罗斯国家的镜像！

```null
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://huecker.io",
        "https://dockerhub.timeweb.cloud",
        "https://noohub.ru"
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker


```

参考文章：[解决Docker Hub国内无法访问方法汇总](https://51.ruyo.net/18416.html)

### 镜像导出导入

**优点：项目部署中可方便使用**

**缺点：镜像文件有时候比较大，复制迁移不方便**

已经Pull过镜像的服务器，导出镜像，然后在需要pull镜像的服务器导入即可！

导出镜像到xxx.tar 

```null
docker save -o <path for generated tar file> <image name>

例子：docker save -o nginx.tar nginx

```

将xxx.tar复制到其他服务器上

```null
docker load -i <path to image tar file>
示例：docker load -i nginx.tar

```

### Yandex镜像

**优点：适合单独pull镜像使用**

**缺点：不支持配置到 daemon.json**

官方镜像：[https://mirror.yandex.ru/](https://mirror.yandex.ru/)

使用方式：docker pull cr.yandex/mirror/nginx

低门槛
---

### Docker使用HTTP代理

**优点：不修改Docker加速镜像，可从官方直接拉取**

**缺点：使用完毕后一定要关闭HTTP代理**

首先，代理需要能访问Docker Hub！这个软件不多说了我测试使用的是 V2**N。该软件支持局域网内访问。

![](https://b.bdstatic.com/comment/vu5WqQNgL2UEadZ0YSzjrg42de5b82881cc218ddba1438a79b178a.png)

这部分内容需要结合历史文章一起查看！

[局域内网的服务器利用跳板机访问互联网](https://51.ruyo.net/17717.html)

[GitHub国内加速：Linux虚拟机利用局域网代理优化访问](https://51.ruyo.net/18304.html)

上面俩篇文章提到给服务器在Shell窗口添加代理环境变量，但是Shell窗口代理环境变量对于Docker Pull镜像的话根本不会生效！

这里主要介绍如何让服务器的Docker能走代理！

1，新建目录

```null
mkdir -p /etc/systemd/system/docker.service.d


```

2，新建文件，粘贴并内容，并保存！

```null
vim /etc/systemd/system/docker.service.d/http-proxy.conf

```

以下粘贴内容，IP一定要换成你代理软件运行的电脑的内网IP，通过ipconfig可以查看

端口一定要是代理软件设置的局域网端口！

```null
[Service]
Environment="HTTP_PROXY=http://192.168.8.125:10819"
Environment="HTTPS_PROXY=http://192.168.8.125:10819"
Environment="NO_PROXY=your-registry.com,10.10.10.10,*.example.com"


```

3，重启Docker

```null
systemctl daemon-reload
systemctl restart docker

```

4，检查环境变量是否生效

```null
systemctl show 

```

高门槛
---

### Cloudflare反向代理

**优点：只需有CF账号就行，自己专属，不用自己签发证书**

**缺点：CF在国内有DNS污染，可能无法正常访问**

简要步骤：

1，登录到CF[https://dash.cloudflare.com/](https://dash.cloudflare.com/)

2，控制台面板 -> 左侧 Workers 和 Pages  ->  创建应用程序  -> 创建 Worker ->  点击保存 -> 点击完成 -> 编辑代码

worker.js 内容

```null
import HTML from './docker.html';
export default {
  async fetch(request) {
    const url = new URL(request.url);
    const path = url.pathname;
    const originalHost = request.headers.get("host");
    const registryHost = "registry-1.docker.io";
    if (path.startsWith("/v2/")) {
      const headers = new Headers(request.headers);
      headers.set("host", registryHost);
      const registryUrl = `https://${registryHost}${path}`;
      const registryRequest = new Request(registryUrl, {
        method: request.method,
        headers: headers,
        body: request.body,
        redirect: "follow",
      });
      const registryResponse = await fetch(registryRequest);
      console.log(registryResponse.status);
      const responseHeaders = new Headers(registryResponse.headers);
      responseHeaders.set("access-control-allow-origin", originalHost);
      responseHeaders.set("access-control-allow-headers", "Authorization");
      return new Response(registryResponse.body, {
        status: registryResponse.status,
        statusText: registryResponse.statusText,
        headers: responseHeaders,
      });
    } else {
      return new Response(HTML.replace(/{{host}}/g, originalHost), {
        status: 200,
        headers: {
          "content-type": "text/html"
        }
      });
    }
  }
}

```

新建文件 docker.html 粘贴下面内容

```null
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>镜像使用说明</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        .header {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: #fff;
            padding: 20px 0;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        .container {
            max-width: 800px;
            margin: 40px auto;
            padding: 20px;
            background-color: #fff;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
        .content {
            margin-bottom: 20px;
        }
        .footer {
            text-align: center;
            padding: 20px 0;
            background-color: #333;
            color: #fff;
        }
        pre {
            background-color: #272822;
            color: #f8f8f2;
            padding: 15px;
            border-radius: 5px;
            overflow-x: auto;
        }
        code {
            font-family: 'Source Code Pro', monospace;
        }
        a {
            color: #4CAF50;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
        @media (max-width: 600px) {
            .container {
                margin: 20px;
                padding: 15px;
            }
            .header {
                padding: 15px 0;
            }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Source+Code+Pro:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>
    <div class="header">
        <h1>镜像使用说明</h1>
    </div>
    <div class="container">
        <div class="content">
            <p>为了加速镜像拉取，你可以使用以下命令设置 registry mirror:</p>
            <pre><code>sudo tee /etc/docker/daemon.json &lt;&lt;EOF
{
    "registry-mirrors": ["https://{{host}}"]
}
EOF</code></pre>
            <p>为了避免 Worker 用量耗尽，你可以手动 pull 镜像然后 re-tag 之后 push 至本地镜像仓库:</p>
            <pre><code>docker pull {{host}}/library/alpine:latest # 拉取 library 镜像
docker pull {{host}}/coredns/coredns:latest # 拉取 coredns 镜像</code></pre>
        </div>
    </div>
    <div class="footer">
        <p>Powered by Cloudflare Workers</p>
    </div>
</body>
</html>

```

3，点击部署即可

4，绑定自定义域名

设置 -> 触发器 -> 自定义域 -\> 点击【添加自定义域】

没有域名？访问博客历史文章：[免费域名](https://51.ruyo.net/tag/%e4%be%bf%e5%ae%9c%e5%9f%9f%e5%90%8d)

**演示地址：dockerhub.o0o.us.kg**

### Nginx反向代理

**优点：需要有境外服务器**

**缺点：网络可能慢或者不稳定**

这种方法需要自己有一台境外服务器，签发域名证书。按下面配置即可！

这里博主并未测试，下面内容仅供参考

```null
server {
            listen 443 ssl;
            server_name 域名;

            ssl_certificate 证书地址;
            ssl_certificate_key 密钥地址;

            proxy_ssl_server_name on; 

            ssl_session_timeout 24h;
            ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;

            location / {
                    proxy_pass https://registry-1.docker.io;  

                    proxy_set_header Host registry-1.docker.io;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header X-Forwarded-Proto $scheme;

                    
                    proxy_buffering off;

                    
                    proxy_set_header Authorization $http_authorization;
                    proxy_pass_header  Authorization;

                    
                    proxy_intercept_errors on;
                    
                    recursive_error_pages on;
                    
                    

                    error_page 429 = @handle_too_many_requests;
            }
            
            location @handle_redirect {
                    resolver 1.1.1.1;
                    set $saved_redirect_location '$upstream_http_location';
                    proxy_pass $saved_redirect_location;
            }
            
            location @handle_too_many_requests {
                    proxy_set_header Host 替换为在CloudFlare Worker设置的域名;  
                    proxy_pass http://替换为在CloudFlare Worker设置的域名;
                    proxy_set_header Host $http_host;
            }
    }


```

### 其他开源项目

[https://github.com/NoCLin/LightMirrors](https://github.com/NoCLin/LightMirrors)

[https://github.com/bboysoulcn/registry-mirror](https://github.com/bboysoulcn/registry-mirror)

最后总结
----

1，如临时使用，建议参与零门槛几个方案，方便快捷

2，据说后面pip源可能也会受到影响，可用采用HTTP代理方式

3，国内阿里云，腾讯云均提供私有镜像服务，大家可用尝试看看

**本文部分参考自 [blog.lty520.faith](https://blog.lty520.faith/%E5%8D%9A%E6%96%87/%E8%87%AA%E5%BB%BAdocker-hub%E5%8A%A0%E9%80%9F%E9%95%9C%E5%83%8F/)，除了Nginx部分内容均亲测可用！**