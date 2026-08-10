[总结国内还能用的 \[Docker.io![](https://linux.do/images/emoji/apple/whale.png?v=12)
 & Podman mirrors\]镜像 6.13更新：国内源+1 & 国外源+1 ](/t/topic/108170)   
=============================================================================================================================================

[配置调优](/c/optimize/7)

[docker](/tag/docker)

您已选择 **0** 个帖子。

全选

取消选择

6.3k 浏览量 80 赞 19 链接 37 用户

 [![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
  6](/u/updownup "updownup") 

 [![](https://linux.do/letter_avatar_proxy/v4/letter/u/cab0a1/96.png)](/u/user5 "user5") 

 [![](https://linux.do/user_avatar/linux.do/handsome/96/155286_2.png)](/u/handsome "handsome") 

 [![](https://linux.do/user_avatar/linux.do/qinambayi/96/16553_2.png)](/u/qinambayi "qinambayi") 

 [![](https://linux.do/user_avatar/linux.do/lovemoon51/96/59416_2.png)](/u/lovemoon51 "lovemoon51") 

6 月 9 日

1 / 42

6 月 9 日

4 天

​

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

15

[6 月 9 日](/t/topic/108170?u=dongfeng "发布日期")

镜像如下：

```


`{ 
"registry-mirrors" : 
[ 
"https://docker.m.daocloud.io", 
"https://docker.jianmuhub.com",
"https://huecker.io",
"https://dockerhub.timeweb.cloud",
"https://dockerhub1.beget.com",
"https://noohub.ru"
] 
}` 
```

  
  
[docker.m.daocloud.io 129](http://docker.m.daocloud.io) 走的阿里云，还能用的话，首选这个镜像!  
jianmuhub 为北京BGP  
![](https://linux.do/uploads/default/original/3X/9/c/9c92afdd291e899d5e690a42b6d19f138e8fd307.png)
  
  
  
  
![](https://linux.do/uploads/default/original/3X/4/0/403dfab20f49164329f022f28bf43298fc72c56a.png)
Docker 一键配置脚本：

```


`([ -f /etc/docker/daemon.json ] || mkdir -p /etc/docker) && echo '{ "registry-mirrors" : [ "https://docker.m.daocloud.io", "https://docker.jianmuhub.com",  "https://huecker.io", "https://dockerhub.timeweb.cloud", "https://dockerhub1.beget.com", "https://noohub.ru" ] }' > /etc/docker/daemon.json && sudo systemctl restart docker && sleep 1 && docker info | grep -A 6 "Registry Mirrors"` 
```

* * *

**如果添加配置无效，可以使用下面的方法免配置拉取镜像：**   
以拉取alpine镜像为例：  
yandex 的docker镜像需要使用以下方式使用:

```


`docker pull cr.yandex/mirror/alpine` 
```

  
  
aws([CloudFront，可优选IP 10](https://monitor.gacjie.cn/page/cloudfront/cname.html))（移动联通首选）:  
[Amazon ECR Public Gallery - Docker 24](https://gallery.ecr.aws/docker/)

```


`docker pull public.ecr.aws/docker/library/alpine:edge` 
```

* * *

**6.13更新：**   
暂时失效（已调整镜像顺序，不排除后续恢复）：  
[https://noohub.ru 58](https://noohub.ru)  
补充地址：  
[https://dockerhub1.beget.com 102](https://dockerhub1.beget.com)  
[https://docker.jianmuhub.com 95](https://docker.jianmuhub.com) 感谢 Ethan9527 提供

![](https://linux.do/uploads/default/original/3X/a/1/a14b7471465036a5d59723e27f6c455ef02d12fb.png)
Podman 配置文件（路径：/etc/containers/registries.conf）

```


`# unqualified-search-registries = ["registry.fedoraproject.org", "registry.access.redhat.com", "registry.redhat.io", "docker.io"]
unqualified-search-registries = ["docker.io"]

[[registry]]
prefix = "docker.io"
location = "docker.m.daocloud.io"
[[registry.mirror]]
location = "docker.jianmuhub.com"
[[registry.mirror]]
location = "dockerhub.timeweb.cloud"
[[registry.mirror]]
location = "dockerhub1.beget.com"
[[registry.mirror]]
location = "huecker.io"
[[registry.mirror]]
location = "noohub.ru"` 
```

* * *

资料来源：  
[Использование Docker из РФ ![](https://linux.do/images/emoji/apple/ru.png?v=12)
 |arty.dev (artydev.ru) 43](https://artydev.ru/posts/docker/)  
  
  
  
  
  
附yandex其他镜像:  
[Index of / (yandex.ru) 95](https://mirror.yandex.ru/)

高级解决方案：  
修改docker源码，添加ECH、分片

![](https://linux.do/images/emoji/apple/heart.png?v=12)

heart

![](https://linux.do/images/emoji/apple/+1.png?v=12)

+1

64

回复

*   [很多佬说了如何pull镜像，我来说一下如何Build镜像并Push吧19](https://linux.do/t/topic/108759)
*   [已解决 国内服务器安装docker4](https://linux.do/t/topic/134146/10)
*   [阿里云服务器ecs部署chat2api好象失败1](https://linux.do/t/topic/130832/2)

6.3k 浏览量 80 赞 19 链接 37 用户

 [![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
  6](/u/updownup "updownup") 

 [![](https://linux.do/letter_avatar_proxy/v4/letter/u/cab0a1/96.png)](/u/user5 "user5") 

 [![](https://linux.do/user_avatar/linux.do/handsome/96/155286_2.png)](/u/handsome "handsome") 

 [![](https://linux.do/user_avatar/linux.do/qinambayi/96/16553_2.png)](/u/qinambayi "qinambayi") 

 [![](https://linux.do/user_avatar/linux.do/lovemoon51/96/59416_2.png)](/u/lovemoon51 "lovemoon51") 

[![](https://linux.do/user_avatar/linux.do/handsome/96/155286_2.png)
](/u/handsome)

[大帅哥](/u/handsome)[handsome](/u/handsome)种子用户 ![](https://linux.do/images/emoji/apple/rage.png?v=12) 

[6 月 9 日](/t/topic/108170/2?u=dongfeng "发布日期")

感谢。

回复

[![](https://linux.do/user_avatar/linux.do/magelaile/96/58341_2.png)
](/u/magelaile)

[马哥-扫地僧](/u/magelaile)[magelaile](/u/magelaile)文化宣导员 ![](https://linux.do/images/emoji/apple/speech_balloon.png?v=12) 

[6 月 9 日](/t/topic/108170/3?u=dongfeng "发布日期")

感谢分享

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/d/a88e4f/96.png)
](/u/depression)

[depression](/u/depression)海纳百川

[6 月 9 日](/t/topic/108170/4?u=dongfeng "发布日期")

感谢分享，docker registry mirrors 有时候真抽风…

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/h/aeb1de/96.png)
](/u/houlangs)

[houlangs](/u/houlangs)浴火重生 ![](https://linux.do/images/emoji/apple/speech_balloon.png?v=12) 

[6 月 10 日](/t/topic/108170/5?u=dongfeng "发布日期")

加个，[docker.hlyun.org 230](http://docker.hlyun.org)

1 个回复

![](https://linux.do/images/emoji/apple/clap.png?v=12)

clap

2

回复

[![](https://linux.do/user_avatar/linux.do/wsli/96/41148_2.png)
](/u/wsli)

[nil](/u/wsli)[wsli](/u/wsli)海纳百川 ![](https://linux.do/images/emoji/apple/cn.png?v=12) 

[6 月 13 日](/t/topic/108170/6?u=dongfeng "发布日期")

感谢分享，刚需

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/w/46a35a/96.png)
](/u/wu_wenyao)

[wuwenyao](/u/wu_wenyao)浴火重生

[6 月 13 日](/t/topic/108170/7?u=dongfeng "发布日期")

感谢，正需要

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/k/ed655f/96.png)
](/u/kn1ght)

[kn1ght](/u/kn1ght)浴火重生

[6 月 13 日](/t/topic/108170/8?u=dongfeng "发布日期")

感谢

回复

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

[6 月 13 日](/t/topic/108170/9?u=dongfeng "发布日期")

已新加两个源

回复

[![](https://linux.do/user_avatar/linux.do/haven/96/110609_2.png)
](/u/Haven)

[黑神话·悟空](/u/Haven)[Haven](/u/Haven)文化宣导员 ![](https://linux.do/images/emoji/apple/watermelon.png?v=12) 

[6 月 13 日](/t/topic/108170/10?u=dongfeng "发布日期")

docker和podman什么区别 那个更好用 更适合生产

1 个回复

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/l/3d9bf3/96.png)
](/u/limars)

[limars](/u/limars)

[6 月 13 日](/t/topic/108170/11?u=dongfeng "发布日期")

顺便问个问题，有没有什么办法能不重启就能换源？总感觉我那一堆容器运行的好好的，一换源都要重启一溜，不舒服。

1 个回复

回复

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

 ![](https://linux.do/user_avatar/linux.do/haven/48/110609_2.png)
 黑神话·悟空

[6 月 13 日](/t/topic/108170/12?u=dongfeng "发布日期")

~个人感觉podman更好一些~  
适合就好，生产不都用k8s，openshift之类的吗  
![](https://linux.do/uploads/default/original/3X/c/1/c15ef8654e6a63e4c099397514bb839361542037.png)

回复

[![](https://linux.do/user_avatar/linux.do/yilong/96/8427_2.png)
](/u/yilong)

[YL](/u/yilong)[yilong](/u/yilong)浴火重生

[6 月 13 日](/t/topic/108170/13?u=dongfeng "发布日期")

mark

![](https://linux.do/images/emoji/apple/open_mouth.png?v=12)

open\_mouth

1

回复

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

2

 ![](https://linux.do/letter_avatar_proxy/v4/letter/l/3d9bf3/48.png)
 limars

[6 月 13 日](/t/topic/108170/15?u=dongfeng "发布日期")

~有的，先把配置写进去（/etc/docker/daemon.json），然后执行下面的指令就行了~

```


`sudo kill -SIGHUP $(pidof dockerd)` 
```

dockerd 作用见文档： [dockerd | Docker Docs 4](https://docs.docker.com/reference/cli/dockerd/)  
  
但我没试过，正式环境别用

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/l/4bbf92/96.png)
](/u/lianglu)

[lianglu](/u/lianglu)

[6 月 13 日](/t/topic/108170/16?u=dongfeng "发布日期")

感谢分享

2 个回复

回复

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

2

 ![](https://linux.do/letter_avatar_proxy/v4/letter/l/4bbf92/48.png)
 lianglu

[6 月 13 日](/t/topic/108170/17?u=dongfeng "发布日期")

建议先开台虚拟机测试一下

回复

[![](https://linux.do/user_avatar/linux.do/updownup/96/84254_2.png)
](/u/updownup)

[updownup](/u/updownup)活跃用户 ![](https://linux.do/uploads/default/original/3X/5/3/535b871b10c649510280825c98e80ee1122613ec.png?v=12) 

4

 ![](https://linux.do/letter_avatar_proxy/v4/letter/l/4bbf92/48.png)
 lianglu

[6 月 13 日](/t/topic/108170/19?u=dongfeng "发布日期")

好像可以，刚刚忘记重新放文件进去了，注意信号量一定是`SIGHUP`

参考文章：  
[Docker Essentials \[01\]: Configuration and control for client and daemon operations | by Mikolaj Maslanka | Medium 7](https://medium.com/@Mikolaj_Maslanka/docker-essentials-01-configuration-and-control-for-client-and-daemon-operations-93d5684b06fa)  
[Docker搭建私有仓库\_给 dockerd 发送 sighup 信号-CSDN博客 7](https://blog.csdn.net/qq_42910468/article/details/128815039)

1

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/z/cc9497/96.png)
](/u/zfc)

[zfc](/u/zfc)活跃用户

[6 月 13 日](/t/topic/108170/20?u=dongfeng "发布日期")

有的人还说Docker被Podman取代了，以此为由认为DockerHub被q没什么，结果现在Podman的服务也被q了？  
那些人明显就是没看出来主要矛盾，Docker和Podman根本就是同一种东西，它们之间的关系是竞争关系，而要q掉它们的服务，完全就是另一回事。今天能q掉Docker，明天也能q掉Podman。

1

回复

[![](https://linux.do/user_avatar/linux.do/yh2386/96/6744_2.png)
](/u/yh2386)

[Neo迷弟](/u/yh2386)[yh2386](/u/yh2386)种子用户 ![](https://linux.do/images/emoji/apple/speech_balloon.png?v=12) 

[6 月 14 日](/t/topic/108170/21?u=dongfeng "发布日期")

好用，随后就把基础镜像传到harbor了，防止被扼住咽喉

回复

[![](https://linux.do/user_avatar/linux.do/kiko923/96/74696_2.png)
](/u/kiko923)

[yionchi](/u/kiko923)[kiko923](/u/kiko923)

[6 月 17 日](/t/topic/108170/22?u=dongfeng "发布日期")

提供自己的一份力 我的docker镜像加速站点  
[地址①:dockerpull.com 178](https://dockerpull.com/)

[地址②:dockerproxy.cn 98](https://dockerproxy.cn/)

6 个回复

![](https://linux.do/images/emoji/apple/heart.png?v=12)

heart

![](https://linux.do/images/emoji/apple/clap.png?v=12)

clap

![](https://linux.do/images/emoji/apple/+1.png?v=12)

+1

9

回复

[![](https://linux.do/user_avatar/linux.do/luckyx/96/29386_2.png)
](/u/luckYx)

[luckYx](/u/luckYx) ![](https://linux.do/uploads/default/original/3X/d/3/d3347ade2d4baf8092696b33c3523e634923b4fe.png?v=12) 

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[6 月 19 日](/t/topic/108170/23?u=dongfeng "发布日期")

用上了，感谢佬友

回复

[![](https://linux.do/user_avatar/linux.do/biuaxia/96/47628_2.png)
](/u/biuaxia)

[biuaxia](/u/biuaxia)浴火重生 ![](https://linux.do/images/emoji/apple/cyclone.png?v=12) 

[6 月 25 日](/t/topic/108170/24?u=dongfeng "发布日期")

Mark

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/w/8491ac/96.png)
](/u/willl)

[willl](/u/willl)

[6 月 26 日](/t/topic/108170/25?u=dongfeng "发布日期")

mark

回复

[![](https://linux.do/user_avatar/linux.do/zmj604315581/96/112859_2.png)
](/u/zmj604315581)

[路过看看](/u/zmj604315581)[zmj604315581](/u/zmj604315581)活跃用户

[6 月 26 日](/t/topic/108170/26?u=dongfeng "发布日期")

这是看到为数不多的Linux帖子之一 ![](https://linux.do/uploads/default/original/3X/1/6/16baef70ba80d438e4bb1907ec0c354d680e09df.png?v=12)

回复

[![](https://linux.do/user_avatar/linux.do/cx330-0012/96/113841_2.png)
](/u/cx330-0012)

[cx330-0012](/u/cx330-0012)

[6 月 26 日](/t/topic/108170/27?u=dongfeng "发布日期")

mark

回复

15 天后

[![](https://linux.do/letter_avatar_proxy/v4/letter/a/bc8723/96.png)
](/u/amwps290)

[amwps290](/u/amwps290)浴火重生

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[7 月 11 日](/t/topic/108170/28?u=dongfeng "发布日期")

感谢感谢

回复

[![](https://linux.do/user_avatar/linux.do/363829273/96/105310_2.png)
](/u/363829273)

[老Lu](/u/363829273)[363829273](/u/363829273)

[7 月 12 日](/t/topic/108170/29?u=dongfeng "发布日期")

非常感谢

回复

[![](https://linux.do/user_avatar/linux.do/lovemoon51/96/59416_2.png)
](/u/lovemoon51)

[LingXi](/u/lovemoon51)[lovemoon51](/u/lovemoon51)种子用户 ![](https://linux.do/images/emoji/apple/dizzy.png?v=12) 

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[7 月 12 日](/t/topic/108170/30?u=dongfeng "发布日期")

感谢

回复

[![](https://linux.do/user_avatar/linux.do/1996cyj/96/4915_2.png)
](/u/1996cyj)

[james](/u/1996cyj)[1996cyj](/u/1996cyj)种子用户

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[7 月 12 日](/t/topic/108170/31?u=dongfeng "发布日期")

这域名牛啊!

1

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/l/3e96dc/96.png)
](/u/lingkongkong)

[灵空空](/u/lingkongkong)[lingkongkong](/u/lingkongkong)

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[7 月 12 日](/t/topic/108170/32?u=dongfeng "发布日期")

感谢哦 ![](https://linux.do/images/emoji/apple/smiling_face_with_three_hearts.png?v=12)

回复

[![](https://linux.do/user_avatar/linux.do/wxyshine/96/161_2.png)
](/u/wxyShine)

[若如初见](/u/wxyShine)[wxyShine](/u/wxyShine)种子用户

[7 月 12 日](/t/topic/108170/33?u=dongfeng "发布日期")

收集的目前可用 [Docker镜像加速器，国内可用源 - 人生若只如初见 112](https://www.wxy97.com/archives/b5b225b6-7741-4560-be2f-2e6a4f671d9b)

1

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/k/97f17d/96.png)
](/u/kang114)

[kang](/u/kang114)[kang114](/u/kang114)

[30 天](/t/topic/108170/34?u=dongfeng "发布日期")

感谢大佬

回复

[![](https://linux.do/user_avatar/linux.do/renjian666/96/105550_2.png)
](/u/RenJian666)

[荆轲🛡](/u/RenJian666)[RenJian666](/u/RenJian666)种子用户

[25 天](/t/topic/108170/35?u=dongfeng "发布日期")

感谢分享，用上了。

回复

[![](https://linux.do/user_avatar/linux.do/xingyuluo/96/80826_2.png)
](/u/xingyuluo)

[luoxingyu](/u/xingyuluo)[xingyuluo](/u/xingyuluo)

[24 天](/t/topic/108170/36?u=dongfeng "发布日期")

收藏了希望不会用上

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/z/4da419/96.png)
](/u/zuo)

[zuo](/u/zuo)

[21 天](/t/topic/108170/37?u=dongfeng "发布日期")

非常感谢，解决了大问题

回复

[![](https://linux.do/user_avatar/linux.do/qin/96/17205_2.png)
](/u/qin)

[始皇](/u/qin)[qin](/u/qin)

[21 天](/t/topic/108170/38?u=dongfeng "发布日期")

马克，备用

回复

11 天后

[![](https://linux.do/user_avatar/linux.do/uuck/96/140411_2.png)
](/u/uuck)

[uuck](/u/uuck)

[10 天](/t/topic/108170/39?u=dongfeng "发布日期")

感谢

回复

[![](https://linux.do/user_avatar/linux.do/chengf/96/137023_2.png)
](/u/Chengf)

[Chengf](/u/Chengf)

[9 天](/t/topic/108170/40?u=dongfeng "发布日期")

感谢分享 一直拉不下来 总算解决了

回复

[![](https://linux.do/user_avatar/linux.do/qinambayi/96/16553_2.png)
](/u/qinambayi)

[qinambayi](/u/qinambayi)

[8 天](/t/topic/108170/41?u=dongfeng "发布日期")

Mark.

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/o/ce7236/96.png)
](/u/onlymate)

[牵着妞去散步](/u/onlymate)[onlymate](/u/onlymate)

[6 天](/t/topic/108170/42?u=dongfeng "发布日期")

[https://docker.m.daocloud.io 18](https://docker.m.daocloud.io) 这个太赞了，解决我一直拉取不了镜像的问题

回复

[![](https://linux.do/user_avatar/linux.do/leeorz/96/94552_2.png)
](/u/leeorz)

[leeorz](/u/leeorz)浴火重生

 ![](https://linux.do/user_avatar/linux.do/kiko923/48/74696_2.png)
 yionchi

[5 天](/t/topic/108170/43?u=dongfeng "发布日期")

老哥牛逼

回复

[![](https://linux.do/letter_avatar_proxy/v4/letter/u/cab0a1/96.png)
](/u/user5)

[大厨先生](/u/user5)[user5](/u/user5)

 ![](https://linux.do/letter_avatar_proxy/v4/letter/h/aeb1de/48.png)
 houlangs

[4 天](/t/topic/108170/44?u=dongfeng "发布日期")

感谢 这个确实有用

回复

分享 加入书签 举报 回复

已取消置顶

​

此话题已对您取消置顶；它将以常规顺序显示

常规

​

您会在别人 @ 您或回复您时收到通知。

  

*   推荐
*   相关

### 新话题和未读话题

话题列表，带有按钮的列标题可以排序。
| 话题 | 回复 | 浏览量 | 活动 |
| --- | --- | --- | --- |
| [Sing-box realip配置方案](/t/topic/175470) 

[配置调优](/c/optimize/7)

[singbox](/tag/singbox)



 | 10 | 87 | [3 分钟](/t/topic/175470/11) |
| [吃内存 难顶](/t/topic/175431) 

[配置调优](/c/optimize/7)

 | 3 | 112 | [2 小时](/t/topic/175431/4) |

### 相关话题

话题列表，带有按钮的列标题可以排序。
| 话题 | 回复 | 浏览量 | 活动 |
| --- | --- | --- | --- |
| [总结目前国内加速拉取 docker 镜像的几种方法](/t/topic/111276)

[软件分享](/c/softwares/14)

[docker](/tag/docker)



 | 25 | 3.4k | [13 天](/t/topic/111276/28) |
| [【元宇宙】Linux Do 的 Docker 镜像](/t/topic/127608)

[软件分享](/c/softwares/14)

[元宇宙](/tag/元宇宙)



 | 43 | 2.0k | [4 天](/t/topic/127608/44) |
| [DockerHub国内镜像源列表（2024年6月18日 亲测可用）](/t/topic/114516)

[软件分享](/c/softwares/14)

[docker](/tag/docker)



 | 54 | 10.7k | [3 天](/t/topic/114516/55) |
| 

[已解决 国内服务器安装docker](/t/topic/134146)

[快问快答](/c/general/qa/28)

 | 13 | 533 | [1 小时](/t/topic/134146/15) |
| [【技术分享】自建专属Docker镜像源，实现镜像拉取自由，LightMirrors部署教程（python、npm、PyTorch也可用）](/t/topic/113867)

[软件分享](/c/softwares/14)

[docker](/tag/docker)



 | 5 | 612 | [6 月 21 日](/t/topic/113867/6) |

### 有 [8 个未读](/unread)话题 和 [246 个新](/new)话题， 或浏览[配置调优](/c/optimize/7)中的其他话题