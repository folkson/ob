**Matrix 首页推荐**

[Matrix](https://sspai.com/matrix) 是少数派的写作社区，我们主张分享真实的产品体验，有实用价值的经验与思考。我们会不定期挑选 Matrix 最优质的文章，展示来自用户的最真实的体验和观点。

文章代表作者个人观点，少数派仅对标题和排版略作修改。

* * *

大家好，我是飘雷。

对于咱们玩 NAS 的老油条来说，异地组网、外网远程访问一直是个绝对的刚需。以前很多朋友可能都折腾过 Tailscale、ZeroTier One，或者为了追求全权掌控，硬着头皮买云服务器去搭建 Headscale，但是，这些方案多多少少都有点让人难受的痛点。

像我之前在用的 ZeroTier One，以及 Tailscale，它们的官方服务器全在国外，国内握手连接不仅有时候会比较慢，而且成功率难有保障，一旦遇到复杂的网络环境打洞失败，就会被迫走限速极其严重的 DERP 中继服务器，那速度简直让人抓狂。

而一些朋友选择自建 Headscale 的话，门槛又太高，像是买云服务器、配域名、搞证书、命令行操作等等等等，对很多只想安安静静用 NAS 的朋友来说，只是看看教程就会被劝退了。幸好最近我在 GitHub 上挖到了一款基于 Rust 语言编写的去中心化异地组网神器——[EasyTier](https://github.com/EasyTier/EasyTier)。

![](https://cdnfile.sspai.com/2026/05/24/article/549b03975d6a22eb96f1c27b5aa8c518.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

[https://github.com/EasyTier/EasyTier](https://github.com/EasyTier/EasyTier)

在我的威联通 NAS 上试用了一段时间后，只能说这玩意儿完全可以作为比 Zerotier One 更好用的替代品，真的很香。

今天，飘雷就和大家深入聊聊 EasyTier，并且分享在NAS、PC和手机上异地组建虚拟内网的教程。

![](https://cdnfile.sspai.com/2026/05/24/article/d2ae72577d652db5e128fd13af35f089.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

老规矩，强烈建议大家先点赞、收藏本文，以防后面折腾的时候找不到路！

为什么我极力推荐 EasyTier？
------------------

简单给大家盘一盘 EasyTier 的核心优势，看完你就知道它为什么在 GitHub 能拿到 11.5K 的收藏。

1\. 此前市面上以 Zerotier One 为代表的绝大多数组网软件是中心化的，必须通过中心服务器进行握手才可以实现配对。而 EasyTier 可以实现纯粹的去中心化架构，不区分服务器和客户端，节点之间全部是对等联网，只要一个设备可以与虚拟网中任意一个节点通信，就可以加入虚拟网。

![](https://cdnfile.sspai.com/2026/05/24/article/ba5fd02ac7bfab1cad739621c32176b0.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

2\. EasyTier 更适合墙内的网络环境，采用 UDP+TCP 双通道，对称型 NAT 突破率达 98%，优于 ZeroTier 的 UDP 单通道，也避免了 ZeroTier 默认中继服务器在国外导致的高延迟、不稳定问题。

![](https://cdnfile.sspai.com/2026/05/24/article/1588386ce13eb0dca963ea904c62fd17.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

3\. 极简部署，不用去国外网站注册账号，也不用折腾什么网页授权，提供网页、客户端、命令行三种操作方式，满足不同用户需求，小白也能轻松搞定，而且完全免费，没有组网设备的数量限制。

4\. 更灵活的协议与传输机制，原生支持 TCP/UDP/WireGuard，还可将 TCP 流量转为 KCP/QUIC 协议，提升高丢包环境下的传输稳定性，并且具有全链路零拷贝技术，数据面性能更强，尤其适合大文件传输。

5\. EasyTier 还有更强的扩展性与兼容性，支持 WireGuard 客户端直接接入 EasyTier 网络，实现与现有 WireGuard 设备的互通，同时节点可共享子网供其他节点访问，轻松实现跨子网通信。而且它还支持 Win/MacOS/Linux/FreeBSD/Android/ 鸿蒙等多个平台，兼容 X86/ARM/MIPS 架构，适配路由器、NAS 等多种设备。

NAS 部署流程
--------

这里我们来展示如何在威联通 NAS 上使用 Docker 形式部署 EasyTier，用到的设备是威联通最新的 8 盘位旗舰新品 Qu805。

![](https://cdnfile.sspai.com/2026/05/24/article/77e982e6d02c9a20e25e84399047bf84.jpeg?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

这里我强烈推荐大家使用 Docker Compose 的方式来部署，一键复制粘贴代码即可，参数修改清晰可见，非常方便。其他配置方法可以参考 EasyTier 的官网文档，讲得很清晰：

打开威联通 Container Station，点击左侧的「应用程序」，然后点击右侧黑色「创建」按钮，输入以下代码：

```null
services:
  
  watchtower:
    image: nickfedor/watchtower
    container_name: watchtower
    restart: unless-stopped
    environment:
      - TZ=Asia/Shanghai
      - WATCHTOWER_NO_STARTUP_MESSAGE=true
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --interval 3600 --cleanup --label-enable


  easytier:
    image: easytier/easytier:latest
    hostname: easytier
    container_name: easytier
    labels:
      
      com.centurylinklabs.watchtower.enable: 'true'
    restart: unless-stopped
    network_mode: host
    
    
    cap_add:
      - NET_ADMIN
      - NET_RAW
    environment:
      - TZ=Asia/Shanghai
    devices:
      - /dev/net/tun:/dev/net/tun
    volumes:
      
      - /etc/machine-id:/etc/machine-id:ro
    
    command: >
      -i 10.144.144.10
      --hostname Qu805
      --network-name piaolei
      --network-secret 123
      -p tcp:
      -n 192.168.10.0/24
```

![](https://cdnfile.sspai.com/2026/05/24/article/747b2a94696e06b2d0471cbaf621c3f4.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

部署完成后，可以在 Contain Station 中看到正在运行的应用程序和容器：

![](https://cdnfile.sspai.com/2026/05/24/article/fa6702917e9756301c1f6d0d431912fa.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)
![](https://cdnfile.sspai.com/2026/05/24/article/e5087684a28721b9601baab2246f9a6a.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

由于参数较多，在 yaml 最后一段的 command 部分，我们着重做一下说明。

这是你给 NAS 手动指定的虚拟内网中的固定 IP，其中 10.144.144.X 是我随便选的内网网段，大家可以根据自己的需求设置一个好记的。EasyTier 默认使用的是 -d 参数，虚拟 IP 会自动设置，不过个人觉得 NAS 这种设备还是用一个固定 IP 比较好记忆，所以选择了手动指定的方式。

这是用于标识此设备的主机名，方便我们在节点列表中精准识别。

这是用于标识这个虚拟内网络的网络名称，这里我随便将其命名为piaolei，大家在使用的时候要自己修改一下。

这是入网密码，用于验证此节点属于虚拟内网，我随便写了个 123，大家可以根据实际需求设置一个复杂一点儿的。

这是用于握手的公共节点地址，大家也可以去 EasyTier 提供的公共服务器列表中进行查询替换：

当然如果自己的入网设备有比较固定的公网 IP 或者 DDNS 地址的话，那就可以在这里设置成自己的节点地址，实现完全的去中心化。

这是对 NAS 用户来说非常香的子网代理功能，这里将其设置成 NAS 所在的实际物理局域网网段，比如你家主路由后台是 192.168.50.1，那就改成 192.168.50.0/24，配置好后，外网设备就能直接穿透进内网，远程管理路由器、PC 等其他设备，实现一机入网，全屋畅通。

PC 客户端虚拟组网
----------

异地组网的意义就在于，我们可以把在外办公、或者在咖啡厅里摸鱼的电脑也拉进这个虚拟局域网里。这里我们以 Windows 电脑为例，官方提供了非常漂亮的 GUI 桌面图形版，对小白极其友好，Linux、MacOS 电脑的操作流程也很类似。

首先咱们前往 EasyTier 的 GitHub Release 页面，下载最新的 Windows 客户端 easytier-gui\_2.6.4\_x86-setup.exe。

![](https://cdnfile.sspai.com/2026/05/24/article/ff865db770b093d2c490dc1d13c8a942.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

安装并打开 EasyTier GUI，软件界面一目了然，非常直观，配置信息后点击「运行网络」即可加入虚拟内网。

![](https://cdnfile.sspai.com/2026/05/24/article/8547c8d0f3cae94fc291dac4b9869fc2.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

虚拟 IPv4 地址处，可以手动填一个同网段的未分配 IP，例如 10.144.144.2；如果不填，EasyTier 也会通过 DHCP 自动为你分出一个。

网络名称、网络密码、初始节点等处，与 yaml 代码中最后 command 一段的参数意思相同，这里不再赘述。

此外在这里还有更多更详细的高级设置、端口转发和访问控制选项，大家可以根据自己的实际需要来选择是否勾选。

![](https://cdnfile.sspai.com/2026/05/24/article/2b94b186e921964c777c34e9599083ae.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

加入虚拟内网之后，可以在这里看到在网的其他节点的状态：

![](https://cdnfile.sspai.com/2026/05/24/article/75b65f79419fbc672fa0ed9602fc10b2.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

ping 一下刚才给 NAS 设置的虚拟内网 IP，实测可以完美连接：

![](https://cdnfile.sspai.com/2026/05/24/article/6ad3c21d31023ddf56ee9e8420548477.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

手机等移动客户端虚拟组网
------------

手机的安装与设置方式与 PC 端没啥区别，同样需要先去 EasyTier 的 GitHub Release 页面下载对应的安装包，比如安卓手机下载 app-arm64-release.apk，安装后即可进入配置页面，需要填写的信息也和 PC 端没啥差别。鸿蒙手机则可以去 AppGallery 里直接下载安装。

![](https://cdnfile.sspai.com/2026/05/24/article/034ed12717710f8eed6f733a4e8ea0fd.jpeg?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

入网成功后，同样能看到这个虚拟内网中其他节点的信息，这一点比 ZeroTier 直观多了。

![](https://cdnfile.sspai.com/2026/05/24/article/c89b8c2aad64b42cef9ef2301f110e63.jpeg?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

此外，别忘了我们在 yaml 代码里加入了子网代理参数：

现在，见证奇迹的时刻到了，即便我的手机正在使用 SIM 卡流量，此时在浏览器里直接输入我家软路由的实际 IP（比如 http://192.168.10.1），直接瞬间秒开，就在家里使用 WiFi 是一个效果。

有了这个功能，NAS 就可以替我们把外网所有发往家里内网的流量做好路由中转，出差在外想改个路由配置、控制下家里的智能家居，或者从办公室直接打印家里书房的文件，体验简直不要太爽。

![](https://cdnfile.sspai.com/2026/05/24/article/7f135837b7ffdd5fa0639044e6f766f8.jpeg?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

用入网的电脑 ping 一下使用 SIM 卡流量的手机，延迟还是挺低的：

![](https://cdnfile.sspai.com/2026/05/24/article/2ed458ac2a9b45ace0465535c033b746.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

总结
--

折腾到这里，整个教程其实就差不多了。

如果你在老家、办公室、新房都有 NAS 或者可以使用 Docker 的设备，完全可以如法炮制，实现异地组建虚拟内网，安全且高效。

另外提醒一下，为了让 EasyTier 的 P2P 直连成功率更高，比较建议大家登录自己家里的主路由器后台，把 NAT 类型修改为 FullCone NAT 全锥形 NAT） 或开启 DMZ / UPnP。一旦两端设备实现 P2P 直连，异地拷贝文件的速度就只取决于你家宽带的上传极限了。

以上就是本期的全部内容，如果大家在部署过程中遇到什么问题，欢迎在评论区留言交流，我们下期再见！

\> [中断久坐、科学多动](https://sspai.com/series/385?source=ppfv4au3&source_platform=sspai)，直接跟练让身体更健康 💪🏻

\> 简单、好用、[专注的写作软件](https://sspai.com/item/49?source=ppfv4au3&source_platform=sspai)，少数派为你呈现 🚀