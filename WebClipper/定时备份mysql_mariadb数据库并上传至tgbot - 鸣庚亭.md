                           定时备份mysql/mariadb数据库并上传至tgbot - 鸣庚亭           

[**鸣庚亭**](/)


定时备份mysql/mariadb数据库并上传至tgbot \_

鸣庚 2023年11月9日 上午

2.4k 字 7 分钟 次

定时备份mysql/mariadb数据库并上传至tgbot
=============================

[](#前情提要： "前情提要：")前情提要：[](#前情提要：)
=================================

前文[利用koyeb免费自建bitwarden服务端](https://blog.m-l.cc/2023/03/21/li-yong-koyeb-mian-fei-zi-jian-bitwarden-fu-wu-duan/)可知博主之前已经将bitwarden自建的vaultwarden服务端部署在koyeb,并利用db4free提供的免费数据库实现了数据的永久存储。虽说这样基本不会有什么问题了，但db4free毕竟是别人提供的在线数据库，为了以防万一其跑路或者删数据库，今天特地花时间构思了数据库备份的方法。  
本文分为两节：

*   基本构思
*   定时推送

[](#基本构思 "基本构思")基本构思[](#基本构思)
-----------------------------

### [](#备份数据库 "备份数据库")备份数据库[](#备份数据库)

想要备份mysql/mariadb的数据库，最方便的莫过于mariadb或mysql自带的mysqldump命令了，只需简单一行就可以将数据库备份到指定的文件。

```bash

```

依次填入对应内容后即可备份你的数据库，为了方便记录和存档，这里在生成的备份文件的文件名处添加了`date +%F`来生成带日期的文件名。

### [](#上传至tgbot "上传至tgbot")上传至tgbot[](#上传至tgbot)

其实这里上传到什么地方都可以，你可以选择部署一个rclone将文件上传到你的onedrive、google drive、webdav等任何网盘去，也可以直接扔到github的releases去，但这里为了图方便（只需要一行就能解决它不香吗？），博主就直接选择了通过curl命令将文件上传给tgbot发送给自己。

```bash

```

使用上述命令就可以简单的将你的数据库备份文件上传到你的tgbot处，从而实现云端存储。

[](#定时推送 "定时推送")定时推送[](#定时推送)
-----------------------------

### [](#方案一：vps、机顶盒等 "方案一：vps、机顶盒等")方案一：vps、机顶盒等[](#方案一：vps、机顶盒等)

如果你有一台24小时在线的设备，那么你可以选择直接给上面两条命令写一个sh文件并通过crontab实现定时推送，如果你的设备在国内，可能还需要给[https://api.telegram.org](https://api.telegram.org/)地址前面加一行反代如`https://r.zzy-ac.top/`从而实现在大陆网络环境下的上传。

### [](#方案二：github-actions "方案二：github actions")方案二：github actions[](#方案二：github-actions)

那么如果你就是纯粹的白嫖狗(比如我)，连bitwarden服务端都扔PaaS了，怎么可能还定时服务扔到自己VPS去？给我白嫖！

1.  创建一个github仓库，权限设为私人（Private）

[![](https://img.m-l.cc/2024-08-12-66b9adfeb06ec.webp)
](https://img.m-l.cc/2024-08-12-66b9adfeb06ec.webp "image-20231109195138845")

[image-20231109195138845](https://img.m-l.cc/2024-08-12-66b9adfeb06ec.webp "image-20231109195138845")

2.  点击actions创建workflows文件

[![](https://img.m-l.cc/2024-08-12-66b9ad2904dce.webp)
](https://img.m-l.cc/2024-08-12-66b9ad2904dce.webp "image-20231109195419519")

[image-20231109195419519](https://img.m-l.cc/2024-08-12-66b9ad2904dce.webp "image-20231109195419519")

3.  在yml文件中填入如下内容

```yaml

```

保存之后你就可以白嫖github actions的定时服务自动备份数据库文件了。为了减少服务器压力，建议`- cron: '0 0 * * *'`此处的时间自主设定一个时间，避免大量排队带来的不好体验。

actions运行成功后将会把备份到的数据库文件发送到你的tgbot如下图：

[![](https://img.m-l.cc/2024-08-12-66b9ae25e3a26.webp)
](https://img.m-l.cc/2024-08-12-66b9ae25e3a26.webp "image-20231109200104152")

[image-20231109200104152](https://img.m-l.cc/2024-08-12-66b9ae25e3a26.webp "image-20231109200104152")

[![](https://img.m-l.cc/2024-08-12-66b9acaade0d5.webp)
](https://img.m-l.cc/2024-08-12-66b9acaade0d5.webp "image-20231109200125616")

[image-20231109200125616](https://img.m-l.cc/2024-08-12-66b9acaade0d5.webp "image-20231109200125616")

至此，自动定时备份数据库的功能就实现完毕了，在需要还原数据库时可以使用db4free自带的phpMyAdmin将数据库备份文件导入，或通过命令导入。

* * *

[bitwarden](/categories/bitwarden/)

[网页部署](/tags/%E7%BD%91%E9%A1%B5%E9%83%A8%E7%BD%B2/)

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！

[通过cf origin rules端口反代实现家宽建站 上一篇](/2023/12/01/tong-guo-cf-origin-rules-duan-kou-fan-dai-shi-xian-jia-kuan-jian-zhan/)

[再游湖 下一篇](/2023/10/01/zai-you-hu/)

昵称

邮箱

网址(可选)

#### 预览:

[](https://guides.github.com/features/mastering-markdown/ "Markdown Guide")

0  字

登录提交

8 评论

![](https://seccdn.libravatar.org/avatar/02a2b50ffd762cbea4412fef72ae2ea2)

Yomuko2024-05-03

Edge124.0Windows 10

[db4free.net:3306](http://db4free.net:3306) 是不是备份不了  
github action  
mysqldump: Got error: 2002: “Can’t connect to server on ‘[db4free.net](http://db4free.net)’ (115)” when trying to connect

![](https://seccdn.libravatar.org/avatar/02a2b50ffd762cbea4412fef72ae2ea2)

Yomuko2024-05-03

Edge124.0Windows 10

[@Yomuko](#6634912791d2fe228f052f48): 鉴定为抽风

![](https://seccdn.libravatar.org/avatar/8452d2d30fb3f2b46be307b8d910fb16)

[兔哥博客](https://uuzi.net)2024-04-02

Chrome123.0Mac OS 10.15.7

博主，我又来了。我想问一下，电报机器人接受文件的大小限制是多少？是2G吗？

![](https://seccdn.libravatar.org/avatar/5002dd517ae688e5cc79a4901920a5f3)

[zzy-ac](https://blog.m-l.cc)博主2024-04-05

Edge123.0Windows 10

[@兔哥博客](#660c1409bf3d552f330c08bb): 应该是吧，好像开会员可以不限容量？  
话说你一个数据库的备份文件这么大的吗？这么多数据的话还是好好准备准备备份方案吧

![](https://seccdn.libravatar.org/avatar/d41d8cd98f00b204e9800998ecf8427e)

兔哥2024-03-03

GSA301.0iOS 17.1

Wordpress 网站备份是不是也可以加密存储到电报上？有什么问题吗？前辈。

![](https://seccdn.libravatar.org/avatar/5002dd517ae688e5cc79a4901920a5f3)

[zzy-ac](https://blog.m-l.cc)博主2024-03-03

Chrome120.0Android 13

[@兔哥](#65e4542683bf2c2c452a137f): 不清楚，你可以自己试试

![](https://seccdn.libravatar.org/avatar/b6f439d9d5598c3b1d449414a1ff43b2)

[竹林里有冰](https://zhul.in)2024-01-22

Firefox122.0Linux x86\_64

有意思，自动发送到 TG 确实是个人小型数据库保存的友好方案，我去给我的 umami 数据库也整一个

![](https://seccdn.libravatar.org/avatar/5002dd517ae688e5cc79a4901920a5f3)

[zzy-ac](https://blog.m-l.cc)博主2024-01-22

Edge120.0Windows 10

[@竹林里有冰](#65ade1c5ff5ee4642e6a45a2): 好哇

Powered by [Waline](https://github.com/walinejs/waline) v1.6.0

 目录

1.  [前情提要：](#前情提要：)
    1.  [基本构思](#基本构思)
        1.  [备份数据库](#备份数据库)
        2.  [上传至tgbot](#上传至tgbot)
    2.  [定时推送](#定时推送)
        1.  [方案一：vps、机顶盒等](#方案一：vps、机顶盒等)
        2.  [方案二：github actions](#方案二：github-actions)

[](#)
