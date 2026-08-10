[晒晒JDread电子书阅读器 附解决近期批次刷机屏幕漂移及更新其他系统固件的方法_电子书阅读器_什么值得买](https://post.smzdm.com/p/280256/p2/?sort_tab=hot/#comments) 

 ![](https://a.zdmimg.com/201704/05/58e4aa55b8fff5040.jpg_fo742.jpg)

晒晒JDread电子书阅读器 附解决近期批次刷机屏幕漂移及更新其他系统固件的方法
========================================

2017-04-05 16:27:23 43点赞 228收藏 92评论

**追加修改(2017-04-10 14:54:23):**  
增加原版基础上增加adb功能，安装第三方apk： 1、先线刷1104adb版本 2、单刷修复包，本人提取自官方修复包，方法见固件，请刷入system.img，boot.img，kernel.img，重启后恢复为原版系统并带adb，请使用豌豆荚等管理软件安装相关apk，推荐安装elink桌面并始终开启。 固件和相关软件下载地址：[链接](http://pan.baidu.com/s/1skUrlI9) 密码：q5sp  
  

晒晒JDread[电子书阅读器](https://www.smzdm.com/fenlei/dianzishuyueduqi/) 附解决近期批次刷机屏幕漂移及更新其他系统固件的方法

购买理由
----

  

前两天PLUS会员活动领了张599-50的券，发现jdread竟然出了无礼包版，比礼包版便宜了不少，因为plus会员本身就有畅读，所以无礼包版更为适合，plus会员还能优惠30，晒单还送[皮套](https://www.smzdm.com/ju/sp6r9lp/)和[贴膜](https://www.smzdm.com/fenlei/tiemo/)，立马下单拿下。

好价

![](https://y.zdmimg.com/201704/03/58e1a32e8e0c44610.jpg_a200.jpg)

【无礼包版】JDRead京东电子书 阅读器6英寸纯平超高清墨水屏8G内存海量好书 博阅电纸书

￥769

[![](https://am.zdmimg.com/201704/03/58e1af33ccb879199.png_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_2/)

外观展示
----

[![](https://am.zdmimg.com/201704/03/58e1aff6ea5695481.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_3/)

纸质盒子，保护还是比较好，腰带扣得很紧

[![](https://am.zdmimg.com/201704/03/58e1aff4452a54677.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_4/)

京东博阅联合出品，其实就是T63啦，之后的刷机会提到，配置在现有机型里算中上。

[![](https://am.zdmimg.com/201704/03/58e1b108adc578874.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_5/)

主角登场，界面貌似有点不一样（因为照片是之后拍的，已经刷了t63的固件，所以京东阅读那几个字不见了）

[![](https://am.zdmimg.com/201704/03/58e1b1fe70e073365.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_6/)

此时让我吐槽下博阅的做工，jdread右上方的接缝之大真是令人发指。

[![](https://am.zdmimg.com/201704/03/58e21265c04712515.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_7/)

之后的向导就不详述了，无非就是连接wifi输入账号验证，京东阅读的最大优势就是畅读，只要有畅读字样的书籍在会员范围内可以随意观看，但是jdread原版的固件是个封闭系统，虽然是安卓系统但不支持自行安装apk，接下来将详细介绍如何来进行刷机自行安装相关apk。

刷机秘籍
----

在之前的一些文章里也介绍了不少jdread刷机的文章，如果你的jdread是2016年购买的可以参考，但2017年以来根据网友反馈的情况，jdread在硬件上可能发生了一些变化，导致刷[文石

关注

[](javascript:;)

品牌 

粉丝：

*   商品百科
    
*   好价
    
*   社区文章
    





](https://pinpai.smzdm.com/28453/)系统或一些早期的固件会出现屏幕漂移、亮度不正确的情况。为此，本人特地向京东和博阅官方索取了解决相关问题的固件。

**①修复屏幕漂移固件**  
如果想刷其他可安装apk或root的固件，请先刷此固件。若误刷之前的固件出现漂移等情况也可以线刷下面这个修复包，由博阅官方提供，操作方法见里面的文档。

固件下载：[链接](https://pan.baidu.com/s/1o8ExUEQ)

[![](https://am.zdmimg.com/201704/03/58e215775bb5c1390.png_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_8/)

必须注意的是，此时的刷新过程是线刷，需要在关机状态按右翻页并插入usb口，然后软件里接口颜色变为绿色可以进行之后的操作。

[![](https://am.zdmimg.com/201704/03/58e215f205f969923.png_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_9/)

刷新完毕后系统会自动重启完成更新，此时我们可以对jdread进行单刷操作刷新系统，由于单刷保留了系统内核及相关驱动，所以不会产生漂移等硬件问题，下面提高三个固件系统的单刷包。

**②刷文石1.8固件**  
  
单刷文石1.8固件，方法见里面文档，只需刷system和boot，之前请先刷好①修复屏幕漂移固件  
文石1.8单刷固件  
[链接](http://pan.baidu.com/s/1nvDeesp)： 密码：urw8

**③刷博阅T63 root版本 by\_fengcuo**  
  
单刷博阅T63 root版本，提取自fengcuo的线刷包，方法见里面文档，只需刷system和boot，之前请先刷好①修复屏幕漂移固件  
存在问题：亮度方向反但不影响使用  
  
[链接:](https://pan.baidu.com/s/1miNp9Ew) 密码: 55gf  
  
**④刷jdread1.4.1root版本 by\_fengcuo**  
单刷jdread1.4.1root版本，提取自fengcuo的线刷包，方法见里面文档，只需刷system和boot，之前请先刷好①修复屏幕漂移固件  
存在问题：亮度有时候不正常  
  
[链接:](https://pan.baidu.com/s/1slzyMO9) 密码: 957r

[![](https://am.zdmimg.com/201704/03/58e216e1e01c96401.png_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_10/)

特别注意：单刷固件只需刷入system和boot两个img即可，不要搞错切记。另外单刷需要在开机状态插入usb接口，点切换显示“发现MSC设备”就可以刷固件了。

[![](https://am.zdmimg.com/201704/03/58e21822a2f995387.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_11/)

刷完之后，看启动界面变了吧，这里以t63为例

[![](https://am.zdmimg.com/201704/03/58e21894e5ccd5652.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_12/)

系统进去看看，怎么样，就是个安卓的界面吧，可以自己耍各种apk了。我是安装e-ink这个界面，有好多快捷键，用起来还不错。

[![](https://am.zdmimg.com/201704/03/58e2193d45c4a4973.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_13/)

来试试上网，也毫无压力。

[![](https://am.zdmimg.com/201704/03/58e2199a8b84c4976.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_14/)

用扇形控制，可以轻松管理关闭各类软件，因为[电子书](https://www.smzdm.com/fenlei/dianzishuyueduqi/)的[内存](https://www.smzdm.com/fenlei/neicun/)普遍较小，清理工作必须要做好。

[![](https://am.zdmimg.com/201704/03/58e23b8042b5c1324.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_15/)

刷完开放固件后，然后可以自行安装各类apk，自行导入图书阅读，我用的比较多的是fbreader，自定义的选项比较多，字体页面刷新等都可以自定义。

[![](https://am.zdmimg.com/201704/03/58e23c976b3ab5516.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_16/)

速度优先模式

[![](https://am.zdmimg.com/201704/03/58e23c9c4fb828019.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_17/)

画质优先模式

[![](https://am.zdmimg.com/201704/03/58e241cebc7ce2442.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_18/)

另外作为jdread高分辨率300dpi一大卖点，屏幕的画质还是相当棒的，特别是在画质优先模式下，字体平滑无锯齿，看上去非常舒服，看漫画效果也是相当不错。  

[![](https://am.zdmimg.com/201704/03/58e240d3cb3933545.jpg_e1080.jpg)
](https://post.smzdm.com/p/a1vmqd7/pic_19/)

特别推荐es管理器，使用wifi可以从[电脑](https://www.smzdm.com/ju/sp4x11p/)共享或是nas中直接拷贝图书、字体和apk到机器上，非常的方便。

总结
--

优点：

1、价格适中、性能够用、该有的配置基本都有。

2、畅读功能很良心，适合不喜欢折腾的书友。  

3、京东自产自营产品，售后服务相对有保证。

缺点：

1、缺少[耳机](https://www.smzdm.com/fenlei/erji/)和音频设备，无法使用tts听书功能。

2、[外壳](https://www.smzdm.com/ju/sp11vmp/)手感一般，做工还有待提高。  

3、刷机比较麻烦，特别是近期批次的产品，没有专业指导容易刷坏。

  

               

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)

