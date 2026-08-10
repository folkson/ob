> 字数 1107，阅读大约需 6 分钟

eSIM在国外虽然已经普及很久，但是国内大多数手机依然不支持eSIM，不过别担心，现在我们可以使用eUICC卡来让原本不支持eSIM功能的手机间接支持eSIM，然后轻松的开通国外eSIM卡，xiaoz亲测有效。

![](https://mmbiz.qpic.cn/mmbiz_png/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FC9vibT4vfrTBARvzjN4E06J7E4mDI2oaSWLHe2k8hmZaLkbOicoQ2kglw/640?wx_fmt=png&from=appmsg&watermark=1#imgIndex=0)

f161136ecd2c3d3d.png

### 什么是eUICC卡

普通SIM卡是固定了一家运营商的“身份卡”，而eUICC卡是一张“空卡”，你可以根据需要，随时把其他运营商的“身份信息”写进去使用。

但是eUICC卡也存在一些局限性，比如：

*   • 不支持写入国内运营商
    
*   • 需要一张实体eUICC卡作为载体
    
*   • 兼容性不及原生支持eSIM的手机
    

### 什么是eSIM？

eSIM是手机内部一枚虚拟的SIM卡，你可以把它理解成一个直接写入设备的“电话号码身份”。它的最大好处是，未来需要换套餐或运营商时，无需再插拔实体卡，通过扫码或App就能远程轻松切换。

### 实现原理

通过使用一张实体空白eUICC卡作为载体，然后使用第三方APP将各家海外运营商的eSIM卡信息写入到eUICC卡中，后续也可以通过APP管理新增、删除、切换等操作，一张eUICC卡可以写入多个eSIM信息，具体数量视eUICC卡容量而决定。

### 购买eUICC卡

既然我们已经知道了原理，那么接下来我们只需要搞到eUICC卡 + 写卡软件，就可以让原本不支持eSIM的手机，通过eUICC卡来间接支持eSIM。

**购买eUICC：**  为避免广告嫌疑，博主不推荐具体商家，大家可自行在X宝或X多搜索关键词 **eUICC** 购买，一般不到30元。

**购买注意事项**

eUICC卡是存在容量限制的，这个容量需要用来保存eSIM信息，所以容量越大，能写入的eSIM卡数量越多，大家可根据自己的使用情况选择不同规格。

![](https://mmbiz.qpic.cn/mmbiz_png/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FCUjE8wOn6fibvHvcdCTSYY714wdPiayAaibxxH9iaQoZDiaSQu9QgZR8APBw/640?wx_fmt=png&from=appmsg&watermark=1#imgIndex=1)

dbb432d8636a2aef.png

到手后和SIM卡无异，可以简单理解为就是一张空白的实体SIM卡。

![](https://mmbiz.qpic.cn/mmbiz_jpg/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FCOnhicENpDqKyfxziczicicb9ZuLJ1UJeo9nZQpQHSDiaWOjPMbBvK4LbVvg/640?wx_fmt=jpeg&from=appmsg&watermark=1#imgIndex=2)

92eef76c654318e6.jpg

### 使用软件

**安卓**

安卓用户推荐EasyEUICC或NekokoLPA来管理eUICC的eSIM卡，对应下载地址为：

*   • EasyEUICC：[https://gitea.angry.im/PeterCxy/OpenEUICC/releases/tag/unpriv-v1.4.0](https://gitea.angry.im/PeterCxy/OpenEUICC/releases/tag/unpriv-v1.4.0)
    
*   • NekokoLPA：[https://github.com/iebb/NekokoLPA/releases](https://github.com/iebb/NekokoLPA/releases)
    

在购买eUICC卡之前前，可以先用EasyEUICC这个软件检测下手机是否支持，主要看**OMAPI**和**ISD-R**通道，如果不支持OMAPI的话是无法使用的。

![](https://mmbiz.qpic.cn/mmbiz_jpg/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FCU4ibL2azIV1IvSFnaiaqfTtrLicrJF5D1j2ick8oZib9NwkAg1E0r7Rnhwg/640?wx_fmt=jpeg&from=appmsg&watermark=1#imgIndex=3)

918e7c7c45267fb8.jpg

**PC软件**

如果您需要用电脑写卡，可以使用下面的软件

*   • MiniLPA：[https://github.com/EsimMoe/MiniLPA/releases](https://github.com/EsimMoe/MiniLPA/releases)
    
*   • EasyLPAC：[https://github.com/creamlike1024/EasyLPAC/releases](https://github.com/creamlike1024/EasyLPAC/releases)
    

电脑写卡需要配合读卡器使用，所以你还需要搞一个读卡器，不过博主直接使用的安卓手机写卡，所以不需要读卡器，也没有使用电脑软件 + 读卡器测试过，有需要的可自行测试。

### 开通海外eSIM卡

既然硬件和软件都搞定了，现在手机也通过eUICC来间接支持了eSIM功能，那么我们还需要开通一些实际的海外运营商来测试eSIM功能是否正常，这里博主测试过**RedteaGO的海外流量卡**是没有问题的：[https://blog.xiaoz.org/archives/21854](https://blog.xiaoz.org/archives/21854)

![](https://mmbiz.qpic.cn/mmbiz_jpg/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FCPNriaGhSDrgJu6jUUWZ4U5ZgnEfV5ibc8yywDv6BARc7MOMorspOcJicg/640?wx_fmt=jpeg&from=appmsg&watermark=1#imgIndex=4)

2647fe882b6079a6.jpg

### iPhone下如何使用？

如果要在iPhone下使用，可以使用安卓写卡后将eUICC卡插到iPhone手机上即可，或者配合读卡器 + 电脑软件写卡，然后再插入iPhone使用。

### 遇到的一些问题

问题一：博主每次写卡完毕并切换eSIM后，再打开NekokoLPA APP就提示无法读取信息，但实际eSIM能正常使用，就是无法管理，需要重启一次手机后才能恢复。卖家说是手机兼容性问题，你们可以试试是否存在这个问题。

![](https://mmbiz.qpic.cn/mmbiz_jpg/ogVZhnjVIfL0ACezAWhKVExGoBxTm0FCDXHxPFL7K8AjuI3GExn08utXAzGmBDal6OibNpBkOVaw9e1iafsndhgA/640?wx_fmt=jpeg&from=appmsg&watermark=1#imgIndex=5)

fda6c07b7e9c3ee8.jpg

问题二：安卓写入Ubigi eSIM卡后，有信号，但无法上网，咨询朋友得知是部分安卓实际频段问题导致，而在iPhone下使用则不存在此问题！

### 结语

eUICC卡以一种巧妙的方式，让传统手机也能享受eSIM的便捷。这张成本不到30的小卡片，成功绕过了手机硬件的限制，使你能够自由添加和管理多家海外eSIM运营商套餐。

> RedteaGO：性价比极高的中国大陆漫游上网eSIM流量卡：[https://blog.xiaoz.org/archives/21854](https://blog.xiaoz.org/archives/21854)