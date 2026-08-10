前言
--

前两篇文章[《**Obsidian 各种同步方式体验**》](https://irithys.com/obsidian-sync-exp/)和[《**Obsidian 使用Webdav同步**》](https://irithys.com/obsidian-webdav-1/)分别讲了我在最初使用Obisdian时，使用的一些有点“古早”🧐的同步方式的各种体验与自认为很方便的Webdav同步。它们很好，但是有着同步速度略慢，不能实时同步的缺点，但是，现在我发现了最新的、最舒服的Obsidian同步方式，那就是：~花钱使用官方同步~使用LiveSync插件进行同步。

这款插件的名字叫做：**Self-hosted LiveSync**，官方仓库在[Github](https://github.com/vrtmrz/obsidian-livesync?ref=irithys.com)上。为什么叫做LiveSync呢？这是因为在我使用后发现完全可以**实时同步**😮。应该可以和官方的同步服务相媲美了吧？有区别的地方是需要一个CouchDB数据库进行读取写入数据。

> **注意⚠️**：此插件与官方同步服务和iCloud**不兼容**，如果使用了官方同步服务就...还有就是一些其他的同步插件如Remote save尽可能停止定时同步，防止出现两个插件同步冲突等奇奇怪怪的同步问题。此服务最大的特点是**实时同步**，并**不能**完全代替Webdav的备份功能。

你可以在任意平台或在自己服务器部署一个CouchDB数据库，然后可以直接跳过**使用fly.io**章节，直接阅读**网页数据库配置**。但是如果你是小白或没有设置服务器，那么请接着继续看，放宽心，步骤写的很详细🌟，一步步来就能完成。

💡

嘿，现在是@2023年7月26日。实际上，使用LiveSync进行同步，只是需要一个远程的CouchDB数据库让插件进行操作，所以可以不拘泥于本文中提到的fly.io服务。[****Alwaysdata****](https://www.alwaysdata.com/?ref=irithys.com)与[****IBM****](https://www.ibm.com/cloud/cloudant/pricing?ref=irithys.com)都可以。本文提到的fly.io近日已发送邮件将升级CPU，升级后是否依然免费处于未知状态。

> @2022年10月5日 目前又找到[**IBM**](https://www.ibm.com/cloud/cloudant/pricing?ref=irithys.com)提供免费1GB的CouchDB数据库，但是**依然**需要信用卡注册。

使用fly.io
--------

这次使用的是[**fly.io**](https://fly.io/?ref=irithys.com)的免费计划，fly.io是一个SAAS（是（Platform as a Service）的缩写，是指平台即服务）平台，可以搭建如静态博客、Nextjs、Nuxtjs、Deno、Go、Python等底层的各种各样的服务。但首先需要自己注册一个账号，这里可以直接使用Github登录。

![](https://i.imgtg.com/2023/02/08/cbtDU.webp)

> **注意**：fly.io的使用需要绑卡，如果没有绑卡会在**创建应用**章节出现**Error**提示。绑卡过程请在[**fly.io面板**](https://fly.io/dashboard?ref=irithys.com)中**Billing**进行。正常使用国内的双币卡就可以，注意请**如实**填写信息。没有卡的朋友可以去各银行办一张（超好过的😗）或试试虚拟卡？（虚拟卡只是博主想到的一种方案，**没有**试过）

### 安装flyctl

Windows用户在本地打开PowerShell或Windows终端💻，输入：

> 注意：CMD中不支持上面的命令，如果电脑中只有CMD，或许你需要安装[**PowerShell**](https://github.com/PowerShell/PowerShell/releases?ref=irithys.com)（选择latest版本）或[**Windows终端**](https://aka.ms/terminal?ref=irithys.com)。

### 本地登录

会自动打开浏览器进行验证账户操作。

### 创建应用

在本地任意位置创建一个fly.io的工作目录（其实就是创建个能找到的文件夹，你不会放桌面上了吧😂？）。

进入couchdb目录后输入命令：

这一步将会启动一个向导，按自己的需求进行选择。

![](https://i.imgtg.com/2023/02/08/cb46Y.webp)

**Select region**的意思是选择一个位置，尽量选择**靠近**自己的位置。

### 配置卷大小

![](https://i.imgtg.com/2023/02/08/cbHkv.webp)

我输入的配置卷大小命令为：

**nrt**的意思是东京地区，你需要改变为和**上面选择的位置区域**一样的位置代码。这一行命令的意思是：在东京地区创建一个1G大小的卷。

### 调整配置信息

打开应用根目录的`fly.toml`文件，添加或修改如下信息：

> 根目录为在**创建应用**章节中创建的新文件夹**couchdb**

这是我修改的所有内容：

博主是全部配置完毕后写这篇文章的😎，所有可能有遗漏或一些错误？为了保证严谨性，把自己的配置全部贴一份做对照吧：

请对比上面提供的配置文件修改自己的**fly.toml**文件。

🤨

请注意，现在@2023年12月25日fly.io的专用ipv4地址已经开始收费，我被扣了几刀才知道额。费用是每月两刀。如果想要删除ipv4地址，请执行flyctl ip list列出当前使用的ip，然后执行flyctl ip release yourip释放即可。

### 设置密码

在终端中输入命令：

> **注意**：密码使用大小写字母与数字，**不要**使用特殊字符。实际测试后发现使用特殊字符时无法识别密码，会无法登录数据库。

如果想要修改密码，可以再次运行上面的命令。

### 部署

💡

注意，如果提示****Services defined at indexes: 0 require a dedicated IP address. You currently have no dedicated IPs allocated. Please allocate at least one dedicated IP before deploying****，那么需要运行fly ips allocate-v4。

在终端中输入命令：

![](https://pic.imgdb.cn/item/63e386e04757feff336cba2d.webp)

稍作等待后提示部署成功🎉。使用下面命令可以打开网页登录：

如果是fly.io部署，那么在网址后加`/_utils/#/setup`，跳转后可以输入用户名与密码。

网页成功登录，好耶🎉

网页数据库配置
-------

这一大章节都在网页中进行。如果不知道自己的地址，可以打开[**fly.io面板**](https://fly.io/dashboard?ref=irithys.com)，点击自己的应用，`Hostname`处为自己应用的网址。

![](https://pic.imgdb.cn/item/63e386ff4757feff336ce3bf.webp)

### 创建数据库

![](https://i.imgs.ovh/2023/12/20/WXRhO.webp)

点击右上角的`Create Database`，创建一个数据库，`Database name`为数据库名字，`Partitioned`请**不要**勾选，然后点`Create`创建。

### 配置其他信息

打开`Setup`选项卡，填写相关信息。

![](https://pic.imgdb.cn/item/63e387584757feff336d67aa.webp)

第一行的`Specify your Admin credentials`为你在上面步骤中配置的用户信息。第二行的`Bind address the node will listen on`意思是监听的访问地址，设置为`0.0.0.0`为允许所有ip访问。第三行的`Port that the node will use`为你在**调整配置信息**这一步中的`fly.toml`文件中配置的端口，如果和我设置的一样，那这里应该是`5984`😚。设置完成后会显示`Apache CouchDB is configured for production usage as a clustered node! Do you want to replicate data?`，代表配置成功。

### 启用CORS

![](https://i.imgs.ovh/2023/12/19/6abKD.webp)

然后打开`Configuration`选项卡中的`CORS`标签，启用**CORS**。完成网页端操作！

💡

下方的****Origin Domains****需要设置为All domains。

Obsidian设置
----------

这一章节的操作都在Obsidian本体软件中进行。首先需要关闭Obsidian中的安全模式，在**插件市场**中搜索`Self-hosted LiveSync`下载并启用。在Github仓库中手动下载安装本插件可能会出现一些问题。

> 在移动端可能会**打不开**插件市场，是众所周知的网络原因，需要自己解决😑。

### 配置连接信息

打开`Remote Database configuration`选项卡。输入自己的数据库网址、用户名、密码与数据库名。

1.  **数据库网址URI**为`https://你的应用.fly.dev`的形式，如果找不到可以使用`flyctl open`或在[**fly.io面板**](https://fly.io/dashboard?ref=irithys.com)中打开自己的应用查看**Hostname**项。
2.  **用户名**与**密码**为在**调整配置信息**时填写的用户名与在**设置密码**中用命令设置的密码。
3.  **数据库名**为在**创建数据库**时创建的数据库名。

![](https://pic.imgdb.cn/item/63e387a54757feff336dd9b8.webp)

### 修复连接

点击`Test Database Connection`，在右上角出现**Connect to 数据库名**，则为连接成功。然后点击`Check database configuration`，会出现一堆日志，逐个点击后面的`fix`按钮修复即可。

![](https://pic.imgdb.cn/item/63e387ba4757feff336dfb28.webp)

修复完成后重新点击`Check database configuration`，没有出现`fix`按钮即为修复成功。

### 同步设置

打开**Sync Settings**选项卡，其中有所有的同步方式设置。**注意**：实时同步(LiveSync)与定时同步(Periodic Sync)互斥，无法同时打开。

![](https://i.imgtg.com/2023/02/08/cbLpq.webp)

可能有朋友会纠结用哪个，在这里强烈推荐使用**LiveSync**（实时同步）方式，毕竟用此插件就是为了这个。因为Periodic Sync（定时同步）中包含有Sync on Save(保存时同步)、Sync on File Open(文件打开时同步)、Sync on Start(打开软件时同步)各种选项，合计一下完全就是变相的实时同步嘛！第二点是当我使用`hugo server`开启本地博客变更监听后发现，Obsidian会自动在输入文字后保存一次，造成**频繁**的Sync on Save(保存时同步)操作。所以，还是使用LiveSync（实时同步）吧👍。

### 其他设置

在**Sync Settings**选项卡中还包含有 `Use Trash for deleted files`（删除文件到回收站）配置强烈建议打开，防止文件意外丢失。

在**Miscellaneous**选项卡（一个小扳手图标）中，有选项`Show staus inside editor`（在编辑器右上角显示当前同步状态），推荐打开。

同步状态将显示在状态栏，状态都有：

*   ⏹️ 准备就绪
*   💤 LiveSync 已启用，正在等待更改
*   ⚡️ 同步中
*   ⚠️ 出现错误

信息解释：

*   ↑ 上传的 chunk 和元数据数量
*   ↓ 下载的 chunk 和元数据数量
*   ⏳ 等待的过程的数量
*   🧩 正在等待 chunk 的文件数量 如果你删除或修改了文件名称，请等待 ⏳ 图标消失。

### 安装于其他设备

在插件**Setup wizard**选项卡中，点击`Copy Setup URI`，弹出的对话框输入你的数据库密码，即可复制当前的配置信息。在其他如Android、iOS设备上安装此插件并点击`Open Setup URI`输入复制的链接即可。

![](https://i.imgtg.com/2023/02/08/cbEYc.webp)

在这里选第一个，意思为**将此设备设置为辅助或后续设备**。稍等一会儿后即可同步完成。而且同步非常快，只需要几秒钟就好。

### 排错日志

如果遇到问题，可以点击Obsidian左侧的**Show log**查看当前的日志信息。

![](https://pic.imgdb.cn/item/63e388f54757feff336fecad.webp)

冲突解决
----

@**2022年10月5日**

写文章时可能会出现一些各设备同时编辑相同文章位置的冲突问题，Obsidian会在各设备中自动弹出有冲突的地方。

![](https://i.imgs.ovh/2023/12/19/6aTn2.webp)

这是PC端弹出的对话框。

![](https://i.imgs.ovh/2023/12/19/6aqbj.webp)

这是iPhone端。这时只需要选择保留哪些内容就好，合并内容插件会自动解决。

推荐方案
----

值得注意的是，**不应该**在Obsidian中存放太多媒体文件，如：Markdown文件直接引用本地图片或音频。不然会显著拖慢**任何**WebDAV或LiveSync的同步速度。做一个对比，我的Obsidian中大概有80篇左右笔记与博文，本地源文件只有600KB，数据库占用只有2MB左右。如果使用本地引用图片，那么一张图片就能抵得上我全部笔记的大小，同步时间将成倍的增长🙄。所以，这些媒体文件**更应该放入图床或对象存储中**，使用`![]()`的形式引用。

自建图床可以参考《**兰空图床，堂堂登场！**》

[

兰空图床，堂堂登场！

自己的图片分门别类有点乱，搭个图床整理下吧。自己博客引用的图片再也不需要和博客一起部署，部署时间显著减少。图片也可以单独进入图床管理，方便不少呀。

![](https://irithys.com/content/images/size/w256h256/2023/12/60x60-1.png)
吕楪

![](https://irithys.com/content/images/2023/12/-------.webp)


](https://irithys.com/lsky/)

当然也可以使用各种免费的图床服务，但是这类服务通常情况下国内访问会有问题，并伴随着各类暂停服务、跑路等情况，这时你又需要修改文章图片引用路径，所以还是推荐自建或各云服务商的对象存储服务。博主😘使用**Picgo**中安装**兰空图床插件**的方案上传图片，如果你在手机上编辑文章也可以使用：Picgo现在也发布了[**移动端**](https://github.com/PicGo/flutter-picgo?ref=irithys.com)。图片可以使用[**Squoosh**](http://squoosh.io/?ref=irithys.com)全部优化为**Webp**格式，显著降低图片占用，访问更快速更友好🥰。

后记
--

fly.io部署的服务体验太好了💖。Obsidian各端同步起来非常快，虽然看面板只是一个256MB的小机器，但是这个任务完全可以胜任。fly.io每个账户的免费资源包括：总共3GB的卷、最多 3 个共享 CPU-1x 256MB 虚拟机、每月160GB 出站数据传输。看起来还能做一些新玩法的样子。在对比使用Remotely save同步后发现，同步速度除了快就是快，刚在电脑上写完一句话，想起有事情准备走，拿起手机打开Obsidian，完全可以接着继续编辑，无缝同步的体验真是太棒了！我宣布这是Obsidian非官方同步服务的**最佳方式**。

如果想看其他两篇有关Obsidian同步文章可以看《[**Obsidian 各种同步方式体验**](https://irithys.com/obsidian-sync-exp/)》和《[**Obsidian 使用Webdav同步**](https://irithys.com/obsidian-webdav-1/)》。