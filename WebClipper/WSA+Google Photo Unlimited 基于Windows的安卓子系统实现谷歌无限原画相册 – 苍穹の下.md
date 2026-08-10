[WSA+Google Photo Unlimited 基于Windows的安卓子系统实现谷歌无限原画相册 – 苍穹の下](https://www.blueskyxn.com/202311/6882.html) 


目录

Toggle

*   [简介](#%E7%AE%80%E4%BB%8B "简介")
*   [方案优点](#%E6%96%B9%E6%A1%88%E4%BC%98%E7%82%B9 "方案优点")
*   [方案风险](#%E6%96%B9%E6%A1%88%E9%A3%8E%E9%99%A9 "方案风险")
*   [核心工具](#%E6%A0%B8%E5%BF%83%E5%B7%A5%E5%85%B7 "核心工具")
*   [详细说明](#%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E "详细说明")
*   [难点说明](#%E9%9A%BE%E7%82%B9%E8%AF%B4%E6%98%8E "难点说明")
*   [操作步骤](#%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4 "操作步骤")
*   [参考资料](#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99 "参考资料")
*   [人工支持](#%E4%BA%BA%E5%B7%A5%E6%94%AF%E6%8C%81 "人工支持")
*   [谷歌相册的额外说明](#%E8%B0%B7%E6%AD%8C%E7%9B%B8%E5%86%8C%E7%9A%84%E9%A2%9D%E5%A4%96%E8%AF%B4%E6%98%8E "谷歌相册的额外说明")

简介
==

部署难度：低，无需代码编程  
总代码量：2行  
硬件要求：建议Windows11系统，无法确认Windows10的可用性，Windows10以下无需考虑，Server系统未知  
磁盘占用：最低4GB，建议120GB+（指的是WSA占用这么多，不是总的配额）  
内存占用：最低6GB，建议10GB+，推荐16GB（指的是WSA占用这么多，不是总的配额）  
功能：模拟Pixel 一代，支持无需原始画质的图片、视频备份到谷歌相册，不占用谷歌空间

方案优点
====

1，基于WSA，安装难度低  
2，可直接使用Windows文件夹作为数据源，不需要像实体机那样搬运  
3，无限空间存储，永久保存，下载不限速，支持快速导出  
4，相册兼容Alist、Rclone等程序下载（上传仅限Pixel模拟的客户端）  
5，不需要在安卓系统内安装代理程序，直接使用局域网代理源即可

方案风险
====

1，滥用谷歌封号（暂无案例）  
2，硬件配置较高，因为安卓系统比较大便  
3，基于WSA，远程Windows服务器部署较为麻烦，如果没其他需求优先使用家里电脑。

核心工具
====

1，LSPosed\_1.8.6（APK）  
2，LSPosed-v1.9.2-7035-zygisk（框架）  
3，MT管理器  
4，pixelify\_gphotos\_v4.1  
5，magisk+zygisk

详细说明
====

资源包下载  [https://www.aliyundrive.com/s/PiKDnYU76ri](https://www.aliyundrive.com/s/PiKDnYU76ri)

1，pixelify\_gphotos工具可以对安卓软件进行设备模拟，这里需要模拟成Pixel 1代（也就是Pixel XL）  
2，LSP+Magisk是用来启动模拟工具的工具  
3，WSA，在Windows运行安卓系统  
4, MT管理器，手动解决Magisk上LSP框架安装的BUG

难点说明
====

1，WSA镜像，市面上的WSA项目我试了几个，发现全部不可用，无法识别LSP框架导致模拟工具无法部署，最终在网友 @BnqDzj 的介绍下找到了可用的WSA镜像版本，来源是 [https://github.com/BnqDzj/WSA-Script/actions/runs/6744135681](https://github.com/BnqDzj/WSA-Script/actions/runs/6744135681) 。其他项目包括 [https://github.com/YT-Advanced/WSA-Script](https://github.com/YT-Advanced/WSA-Script)和[https://github.com/MustardChef/WSABuilds](https://github.com/MustardChef/WSABuilds)是全部失败，没法用，包括有的人说可以的kernelsu版本，实测全部都不行，原因未知。  
2，Magisk安装LSP框架的重启丢失问题，原ISSUE中说的都是错的，但是其中的原始视频是正确的，只不过只有视频没有说明很多人没看懂。此次我和视频作者一致使用MT管理器  
3，网络连接，在由第三方网友提供的WSA镜像发现LSPosed的VpnDialogs似乎有问题，导致不可用任何代理程序，这里需要ADB连接来设置全局HTTP代理。  
4，第三方WSA解压需要7z23，7z22会导致报错。

操作步骤
====

1，通过网盘下载2个自解压压缩包，**用密码 blueskyxn解压**。其中一个是WSA镜像（解压好的），一个是可能用到的工具。  
2，复制WSA镜像到英文目录的文件夹，运行里面的run.bat，这时候会 自动打开PS并部署，然后会打开一些安卓程序，可以关掉。  
3，然后打开Windows搜索，搜索 Android，找到WSA的管理器，在高级设置中，启用开发人员模式；内存你自己看着办，我是16GB动态；实验性功能本地网络打开，VK驱动系统默认，共享文件夹你自己挑一个新的，后续要在里面放置文件APK和照片。  
4，然后在WSA管理器左边的系统，强制关闭，然后打开文件后会重启，然后安装 MT管理器。  
5，打开Magisk和MT管理器，在Magisk管理器（这里可能叫做 Alpha）首次打开可能要你重启，直接重启，然后管理器首页右上角的设置的Magisk的Zygisk打开后重启，然后打开模块把LSP框架的ZIP载入，然后的页面不要动，也不要重启  
6，打开MT管理器，打开根目录的data的adb的modules\_update文件夹中的文件夹复制到下载目录（本机存储-Download），然后去Magisk重启，重启好之后把刚刚复制的文件夹，覆盖复制到根目录的data的adb的modules文件夹中，这时候Magisk的模块就应该有这个框架了，重启看看还在不在，如果在说明重启清空的问题解决了  
7，安装LSP APK，看看能不能正确识别，如果正确说明运气不错可以继续，其他WSA镜像全部卡在这一步无法识别。  
8，安装pixelify\_gphotos和googlephoto的apk，然后去LSP管理器让这个模拟工具可以控制谷歌相册App。  
9，打开模拟工具，定制机型改为 PixelXL ，然后Force Stop Google Photo ，然后去WSA设置关闭安卓系统，然后点击VIA浏览器等地方启动，现在准备部署代理。  
10，准备好内网的HTTP代理，以及ADB工具，ADB工具在工具包中，放在任意目录然后添加文件夹到系统的Path环境变量内  
11，ipconfig找到 WSL的IPV4地址，比如我是172.24.144.1。  
12，默认情况下ADB是58526端口（注意检查开发者和内网访问，在WSA管理器那边）cd到对应目录，然后使用命令 _.\\adb connect 127.0.0.1:58526_ 即可连接安卓设备  
13，使用命令配置全局HTTP代理，这里走的是局域网，注意权限 _.\\adb shell settings put global http\_proxy 172.24.144.1:10811_ 【IP端口需要符合你的实际情况】  
14，使用VIA浏览器等程序检查谷歌的访问连通性  
15，打开谷歌相册，打开共享文件夹，建议在里面新建个upload文件夹再在里面放上测试图片和视频，然后在谷歌相册使用原画质备份，看看空间使用和文件详情，是不是原画质、无压缩，以及提示免费上传、不占用空间等

后面就没什么好说的了

参考资料
====

[https://www.v2ex.com/t/986836](https://www.v2ex.com/t/986836)  
[https://github.com/MustardChef/WSABuilds/issues/154](https://github.com/MustardChef/WSABuilds/issues/154)  
[https://www.bilibili.com/video/BV1GV4y1v7ys/](https://www.bilibili.com/video/BV1GV4y1v7ys/)  
[https://github.com/BaltiApps/Pixelify-Google-Photos](https://github.com/BaltiApps/Pixelify-Google-Photos)  
[https://www.tjsky.net/tutorial/391](https://www.tjsky.net/tutorial/391)  
[https://github.com/LSPosed/MagiskOnWSALocal/issues/339](https://github.com/LSPosed/MagiskOnWSALocal/issues/339)

人工支持
====

需付费，联系邮 _bluesky艾特000714.xyz_ 或者 wechat _@BlueSky\_XN_

谷歌相册的额外说明
=========

根据谷歌官方文档 [https://support.google.com/photos/answer/6193313](https://support.google.com/photos/answer/6193313)

*   可备份的文件大小：
    *   照片最大为 200 MB 或 2 亿像素。
    *   视频最大为 10 GB。
    *   内容必须大于 256 x 256。
*   可备份的文件类型：
    *   **照片**：.jpg、.heic、.png、.webp、.gif、.avif 和大部分 RAW 文件。
    *   **视频**：.mpg、.mod、.mmv、.tod、.wmv、.asf、.avi、.divx、.mov、.m4v、.3gp、.3g2、.mp4、.m2t、.m2ts、.mts 和 .mkv 文件。

所以视频不能超过10GB，不过实测是AVC、HEVC、AV1编码的视频都能上传，没有其他的问题。

*  