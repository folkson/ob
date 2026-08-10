最新荣耀任意机型/系统删除Hnaps

![](https://i1.hdslb.com/bfs/face/539682313d2bea0e0b8d57418bc2f58581017c3b.jpg@96w_96h_1c_1s.webp)

连恰三大碗

2024年02月20日 14:19

举报

荣耀magic5PRO

截至2024-02-20前发布的机型/系统都能用此方法删除Hnaps，理论上这个方法是长期有效的。但不排除🐶官方作妖。

**获取值**

![](https://i0.hdslb.com/bfs/new_dyn/67330fd2efef739d615f1f4f6fb7a70838596141.jpg@1192w.webp)

按照以上路径找到此文件并查看

点击classes.dex以dex编辑器++的方式打开，勾选全部dex文件

![](https://i0.hdslb.com/bfs/new_dyn/acb71357becff455005338bd643a5d6738596141.jpg@1192w.webp)

后点击搜索-发起新搜索-查找内容👇

transaction\_setsystemappinstall

搜索类型：代码

点击唯一一个搜索记录并记录以下数值

![](https://i0.hdslb.com/bfs/new_dyn/c5877a22a93cdd8f66e7831fda736d0c38596141.jpg@1192w.webp)

**使用进制转换**

这里贴一个出来不知道会不会过审核

[https://www.sojson.com/hexconvert/16to10.html](https://www.sojson.com/hexconvert/16to10.html)

将刚才的数值填进去并记录十进制数值

![](https://i0.hdslb.com/bfs/new_dyn/032d24e4af3d9b6a1ae6f710784b1f6c38596141.jpg@1192w.webp)

懒人福利 若数值一样就直接用

83=131

84=132

85=133

86=134

接下来将上面转换过后的十进制替换进下面的代码中

**adb shell service call package ［数值］ s16 ［包名］ i32 0 i32 0**

这里我的机型使用的是

**adb shell service call package 131 s16 com.hihonor.android.hnaps i32 0 i32 0**  **命令的使用**

1.这里推荐使用platform-tools，下载链接为：

[https://dl.google.com/android/repository/platform-tools\_r35.0.0-windows.zip](https://dl.google.com/android/repository/platform-tools_r35.0.0-windows.zip)

将其解压到电脑上，按住shift右键解压后的文件夹点击在终端中打开。

执行上面的代码（若报错，请于代码上添加 ./ ）

2.使用在线adb网站，链接为：

[https://adb.http.gs/](https://adb.http.gs/)

3.使用第三方工具箱

**重启**

**！恢复**

将代码部分

adb shell service call package ［数值］ s16 ［包名］ i32 0 i32 0

的第一个0换成1执行即可恢复

分享至

微信扫一扫分享

投诉或建议