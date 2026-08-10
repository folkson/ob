## 链接 
 [https://blog.csdn.net/m0_61060696/article/details/138199101](https://blog.csdn.net/m0_61060696/article/details/138199101) 


# 【ARM版银河麒麟安装windows应用程序】


#### 文章目录

- [前言](https://blog.csdn.net/m0_61060696/article/details/138199101#_6)
- [一、简介](https://blog.csdn.net/m0_61060696/article/details/138199101#_22)
- [2.1 Wine/Wine64](https://blog.csdn.net/m0_61060696/article/details/138199101#21_WineWine64_23)
- [2.2 Box86/Box64](https://blog.csdn.net/m0_61060696/article/details/138199101#22_Box86Box64_27)
- [二、配置运行环境](https://blog.csdn.net/m0_61060696/article/details/138199101#_33)
- - [2.1 安装aarch64运行库](https://blog.csdn.net/m0_61060696/article/details/138199101#21_aarch64_34)
    - [2.2 安装armhf运行库](https://blog.csdn.net/m0_61060696/article/details/138199101#22_armhf_41)
    - - [2.2.1 添加32位armhf架构支持](https://blog.csdn.net/m0_61060696/article/details/138199101#221_32armhf_42)
        - [2.2.2 检查运行库依赖](https://blog.csdn.net/m0_61060696/article/details/138199101#222__51)
        - [2.2.3 解决依赖冲突](https://blog.csdn.net/m0_61060696/article/details/138199101#223__72)
        - [2.2.4安装运行库](https://blog.csdn.net/m0_61060696/article/details/138199101#224_158)
- [三、安装Box64](https://blog.csdn.net/m0_61060696/article/details/138199101#Box64_225)
- [四、安装Wine](https://blog.csdn.net/m0_61060696/article/details/138199101#Wine_260)
- [五、初始化wine配置](https://blog.csdn.net/m0_61060696/article/details/138199101#wine_333)
- [总结](https://blog.csdn.net/m0_61060696/article/details/138199101#_360)

---

## 前言

银河麒麟是基于Linux发行版Ubuntu开发的自主可控操作系统，为我国信息基础建设提供了重要支撑。截至目前，银河麒麟V10的软件仓库已经提供了大量国产软件，但在特定情况下，我们可能还是希望使用一些Windows软件，本文介绍了如何在Arm版本银河麒麟V10上安装windows位软件，用于技术交流，以备不时之需，但希望读者继续支持国产软件，支持自主可控软件生态。

虽然麒麟软件商城提供了[Crossover](https://www.codeweavers.com/crossover)商业平台，可以安装部分Windows软件，但是其版本较低，软件兼容性有限，本文介绍的方法主要思路与Crossover类似，利用开源[Box64](https://github.com/ptitSeb/box64)+[Wine64](https://www.winehq.org/)以及[Box86](https://github.com/ptitSeb/box86)+[Wine](https://www.winehq.org/)分别实现Arm版银河麒麟安装64位和32位Windows软件，需要注意的是，该方案是在麒麟系统中实现了Windows的调用库，与基于[Qemu](https://www.qemu.org/)的虚拟机完全不同，因此其运行性能远高于虚拟机。

这里只介绍box64+wine64的安装和配置方法，可以安装运行64位Windows软件，Box86+Wine的安装方法与其类似，不再赘述，如有问题可以留言。

除了本文给的方案，Wine还提供了开源项目[Hangover](https://github.com/AndreRH/hangover)，给出了基于Fex、Qemu和Box的异构平台方案，其中Box方案实质上是把Box64/Box86作为库文件集成到Wine，但不幸的是，本机测试无法运行，而Qemu虚拟机方案性能较差，Fex方案存在大量Bug。

本文测试环境为Arm版银河麒麟Kylin-Desktop-V10-SP1-General-Release-2303-ARM64，CPU 为飞腾D2000。

---

`警告：本安装过程可能替换系统运行库，操作不慎可能导致麒麟UKUI桌面无法运行，请备份系统后谨慎操作`

## 一、简介

## 2.1 Wine/Wine64

官网介绍：Wine （“Wine Is Not an Emulator” 的首字母缩写）是一个能够在多种 POSIX-compliant 操作系统（诸如 Linux，macOS 及 BSD 等）上运行 Windows 应用的兼容层。Wine 不是像虚拟机或者模拟器一样模仿内部的 Windows 逻辑，而是將 Windows API 调用翻译成为动态的 POSIX 调用，免除了性能和其他一些行为的内存占用，让你能够干净地集合 Windows 应用到你的桌面。

简单地说，就是Wine在非Windows系统上，实现了底层的Windows的API，从而可以安装和运行Windows应用。但是，Wine并没有实现CPU架构的转换，也就是X86/X64架构的系统只能运行X86/X64架构的Windows应用，而Arm架构的系统只能运行Arm架构的Windows软件，因此我们需要Box86/Box64实现CPU架构的转换。

## 2.2 Box86/Box64

Github项目介绍：Box64 可以在非 x86_64 Linux 系统（比如 ARM64）上运行 x86_64 Linux 程序（比如游戏），注意主机系统需要是 64 位小端。由于 Box64 使用一些“系统”库的原生版本，如 libc、libm、SDL 和 OpenGL 等，因此很容易与大多数应用程序集成和使用，并且在许多情况下性能会相当不错。可以在[这里](https://box86.org/2021/06/game-performances/)查看一些性能测试的样例。

Box64 集成了适用于 ARM64 和 RV64 平台的 DynaRec（动态重编译器），速度可以比纯解释模式快 5 到 10 倍。可以在[这里](https://box86.org/2021/07/inner-workings-a-high%E2%80%91level-view-of-box86-and-a-low%E2%80%91level-view-of-the-dynarec/)找到有关 DynaRec 工作原理的一些信息。

一些 x64 内部操作码使用 “Realmode X86 Emulator Library” 的部分内容，有关版权详细信息，请参见 [License](https://github.com/ptitSeb/box64/blob/main/LICENSE)。

## 二、配置运行环境

### 2.1 安装aarch64运行库

直接执行以下命令进行安装：

```c
sudo apt-get install libasound2:arm64 libc6:arm64 libglib2.0-0:arm64 libgstreamer-plugins-base1.0-0:arm64 libgstreamer1.0-0:arm64 libldap-2.4-2:arm64 libpcap0.8:arm64 libvkd3d1:arm64 libx11-6:arm64 libxext6:arm64 libasound2-plugins:arm64 ocl-icd-libopencl1:arm64 libncurses6:arm64 libncurses5:arm64 libcap2-bin:arm64 libdbus-1-3:arm64 libfontconfig1:arm64 libfreetype6:arm64 libglu1-mesa:arm64 libglu1:arm64 libgnutls30:arm64 libgssapi-krb5-2:arm64 libkrb5-3:arm64 libodbc1:arm64 libosmesa6:arm64 libsdl2-2.0-0:arm64 libv4l-0:arm64 libxcomposite1:arm64 libxcursor1:arm64 libxfixes3:arm64 libxi6:arm64 libxinerama1:arm64 libxrandr2:arm64 libxrender1:arm64 libxxf86vm1 libc6:arm64
```

本机测试未出现依赖冲突。

### 2.2 安装armhf运行库

#### 2.2.1 添加32位armhf架构支持

输入以下命令添加armhf架构：

```c
sudo dpkg --add-architecture armhf
```

输入以下命令更新apt缓存：

```c
sudo apt update
```

#### 2.2.2 检查运行库依赖

感谢博主GreatNXY的实测，将麒麟apt源更换为2403版本后，安装下列运行库不会出现依赖冲突，建议更换apt源后直接安装下列运行库。（2024.08.26更新）

2403版用户请直接输入以下命令安装运行库。（2024.12.10更新）

```c
sudo apt-get install -y libasound2:armhf libc6:armhf libglib2.0-0:armhf libgphoto2-6:armhf libgphoto2-port12:armhf \
libgstreamer-plugins-base1.0-0:armhf libgstreamer1.0-0:armhf libldap-2.4-2:armhf libopenal1:armhf libpcap0.8:armhf \
libpulse0:armhf libsane1:armhf libudev1:armhf libusb-1.0-0:armhf libvkd3d1:armhf libx11-6:armhf libxext6:armhf \
libasound2-plugins:armhf ocl-icd-libopencl1:armhf libncurses6:armhf libncurses5:armhf libcap2-bin:armhf libcups2:armhf \
libdbus-1-3:armhf libfontconfig1:armhf libfreetype6:armhf libglu1-mesa:armhf libglu1:armhf libgnutls30:armhf \
libgssapi-krb5-2:armhf libkrb5-3:armhf libodbc1:armhf libosmesa6:armhf libsdl2-2.0-0:armhf libv4l-0:armhf \
libxcomposite1:armhf libxcursor1:armhf libxfixes3:armhf libxi6:armhf libxinerama1:armhf libxrandr2:armhf \
libxrender1:armhf libxxf86vm1 libc6:armhf libcap2-bin:armhf
```

2303版用户请输入以下命令查看运行库是否存在依赖冲突：

```c
sudo apt-get install libasound2:armhf libc6:armhf libglib2.0-0:armhf libgstreamer-plugins-base1.0-0:armhf libgstreamer1.0-0:armhf libldap-2.4-2:armhf libpcap0.8:armhf libvkd3d1:armhf libx11-6:armhf libxext6:armhf libasound2-plugins:armhf ocl-icd-libopencl1:armhf libncurses6:armhf libncurses5:armhf libcap2-bin:armhf libdbus-1-3:armhf libfontconfig1:armhf libfreetype6:armhf libglu1-mesa:armhf libglu1:armhf libgnutls30:armhf libgssapi-krb5-2:armhf libkrb5-3:armhf libodbc1:armhf libosmesa6:armhf libsdl2-2.0-0:armhf libv4l-0:armhf libxcomposite1:armhf libxcursor1:armhf libxfixes3:armhf libxi6:armhf libxinerama1:armhf libxrandr2:armhf libxrender1:armhf libxxf86vm1 libc6:armhf
```

`注意：这里没有列出libopenal1:armhf（用于跨平台声音输出）、libgphoto2-6:armhf、libgphoto2-port12:armhf、libsane1:armhf （用于图像扫描软件如扫描仪软件）、libudev1:armhf（与libSDL2库功能相同） libusb-1.0-0:armhf（USB直连，通常不需要）、libcups2（用于连接打印机），因为在本机测试可能会导致UKUI冲突，无法进入桌面，这些库不安装可能导致相关应用无法运行`  
如果没有错误提示，那么恭喜你，基本后续安装不会出现什么问题，但是大部分情况下，运行库可能出现依赖冲突，下一节，以本机实测的安装过程为例，介绍如何进行解决。

#### 2.2.3 解决依赖冲突

本机实测在执行2.2.2节所述安装命令时，出现以下提示：

> 注意，选中 ‘libglu1-mesa:armhf’ 而非 ‘libglu1:armhf’  
> libxxf86vm1 已经是最新版 (1:1.1.4-1build1)。  
> libxxf86vm1 已设置为手动安装。  
> 有一些软件包无法被安装。如果您用的是 unstable 发行版，这也许是  
> 因为系统无法达到您要求的状态造成的。该版本中可能会有一些您需要的软件  
> 包尚未被创建或是它们已被从新到(Incoming)目录移出。  
> 下列信息可能会对解决问题有所帮助：  
> 下列软件包有未满足的依赖关系：  
> libfontconfig1:armhf : 依赖: libuuid1:armhf (>= 2.16) 但是它将不会被安装  
> libglib2.0-0:armhf : 依赖: libmount1:armhf (>= 2.28) 但是它将不会被安装  
> E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。

提示很清楚， libfontconfig1:armhf和libglib2.0-0:armhf库存在依赖冲突。

对于libglib2.0-0:armhf，提示依赖: libmount1:armhf (>= 2.28) 但是它将不会被安装，输入以下命令看一下是哪个库导致libmount1:armhf无法安装：

```c
sudo apt install libmount1:armhf
```

提示如下

> 下列软件包有未满足的依赖关系：  
> libmount1:armhf : 依赖: libblkid1:armhf (>= 2.17.2) 但是它将不会被安装  
> E: 无法修正错误，因为您要求某些软件包保持现状，就是它们破坏了软件包间的依赖关系。

输入以下命令查看libblkid1和libblkid1:armhf的版本：

```c
apt policy libblkid1 libblkid1:armhf
```

提示如下：

> libblkid1:  
> 已安装：2.34-0.1kylin18k0.2  
> 候选： 2.34-0.1kylin19k0.1  
> 版本列表：  
> 2.34-0.1kylin19k0.1 500  
> 500 http://archive.kylinos.cn/kylin/KYLIN-ALL 10.1-2303-updates/main arm64  
> 500 http://archive.kylinos.cn/kylin/KYLIN-ALL 10.1-2303-updates/universe ar  
> *** 2.34-0.1kylin18k0.2 100  
> 100 /var/lib/dpkg/status  
> 2.34-0.1kylin16~rc1 500  
> 500 http://archive.kylinos.cn/kylin/KYLIN-ALL 10.1/main arm64 Packages  
> libblkid1:armhf:  
> 已安装：(无)  
> 候选： 2.34-0.1kylin18k0.2  
> 版本列表：  
> 2.34-0.1kylin18k0.2 500  
> 500 http://archive.kylinos.cn/kylin/KYLIN-ALL 10.1-2303-updates/main armhf  
> 2.34-0.1kylin16~rc1 500  
> 500 http://archive.kylinos.cn/kylin/KYLIN-ALL 10.1/main armhf Packages

这里可以发现，libblkid1:armhf无法安装的可能原因是，apt包管理器默认安装最新版本，并且与libblkid1一致，但libblkid1的最新版本是2.34-0.1kylin19k0.1，本机并未安装，且libblkid1:armhf没有对应的2.34-0.1kylin19k0.1版本，从而导致安装失败。

由于libblkid1:armhf可用的版本为2.34-0.1kylin18k0.2，与libblkid1当前已安装版本一致，因此直接指定libblkid1:armhf版本号进行安装：

```c
sudo apt install -y libblkid1:armhf=2.34-0.1kylin18k0.2
```

类似的，首先输入以下命令查看libmount1和libmount1:armhf版本信息：

```c
apt policy libmount1 libmount1:armhf
```

提示与libblkid1和libblkid1:armhf的版本类似，这里不再赘述，直接输入以下命令安装libmount1:armhf

```c
sudo apt install -y libmount1:armhf=2.34-0.1kylin18k0.2
```

最后直接安装libglib2.0-0:armhf

```c
sudo apt install -y libglib2.0-0:armhf
```

对于libfontconfig1:armhf，采用与上述相同的方法安装指定版本，如果还有其它冲突，请参考[本文](https://blog.csdn.net/qinglingls/article/details/106315288)进行解决。

`本机测试时，发现libopenal1:armhf（音频支持库）、libdbus-1-3:armhf（简单的进程间消息系统）、libasound2-plugins:armhf（蓝牙音频支持库）等运行库需要更新麒麟SDK库libkysdk-config:armhf和libkysdk-utils:armhf，但是出现以下错误，强制安装则导致系统崩溃，暂时未找到解决方案，因此本机未安装这几个库，可能导致特定Windows软件无法运行。`

> dpkg: 处理归档 /var/cache/apt/archives/libkysdk-config_2.0.0.0-0k0.6_armhf.deb (–unpack)时出错：  
> 尝试覆盖共享的 ‘/usr/lib/kysdk/kysdk-base/libkyconf.so.2.0.0’, 它与软件包 libkysdk-config:armhf 中的其他实例不同  
> 准备解压 …/libkysdk-utils_2.0.0.0-0k0.6_armhf.deb …  
> 正在解压 libkysdk-utils:armhf (2.0.0.0-0k0.6) …  
> dpkg: 处理归档 /var/cache/apt/archives/libkysdk-utils_2.0.0.0-0k0.6_armhf.deb (–unpack)时出错：  
> 尝试覆盖共享的 ‘/usr/lib/kysdk/kysdk-base/libkydatastruct.so.2.0.0’, 它与软件包 libkysdk-utils:armhf 中的其他实例不同  
> 在处理时有错误发生：  
> /var/cache/apt/archives/libkysdk-config_2.0.0.0-0k0.6_armhf.deb  
> /var/cache/apt/archives/libkysdk-utils_2.0.0.0-0k0.6_armhf.deb  
> E: Sub-process /usr/bin/dpkg returned an error code (1)  
> Failed to install libopenal1:armhf version 1:1.19.1-1

#### 2.2.4安装运行库

这里提供了一个安装脚本，会逐个安装麒麟官方源中提供的最高版本armhf运行库，使用sudo bash 执行该脚本进行安装：  
`注意，如果某个运行库安装时，提示需要卸载UKUI相关运行库或重要的系统库，请选择N不要安装，否则将导致无法进入UKUI桌面或系统崩溃`

```c
#!/bin/bash  
  
# 定义要安装的armhf包列表  
armhf_packages=("libasound2:armhf" "libc6:armhf" "libglib2.0-0:armhf" "libgstreamer-plugins-base1.0-0:armhf" "libgstreamer1.0-0:armhf" "libldap-2.4-2:armhf" "libpcap0.8:armhf" "libvkd3d1:armhf" "libx11-6:armhf" "libxext6:armhf" "ocl-icd-libopencl1:armhf" "libncurses6:armhf" "libncurses5:armhf" "libcap2-bin:armhf" "libfontconfig1:armhf" "libfreetype6:armhf" "libglu1-mesa:armhf" "libglu1:armhf" "libgnutls30:armhf" "libgssapi-krb5-2:armhf" "libkrb5-3:armhf" "libodbc1:armhf" "libosmesa6:armhf" "libsdl2-2.0-0:armhf" "libv4l-0:armhf" "libxcomposite1:armhf" "libxcursor1:armhf" "libxfixes3:armhf" "libxi6:armhf" "libxinerama1:armhf" "libxrandr2:armhf" "libxrender1:armhf" "libxxf86vm1") # 省略其他包以简化示例  

# 函数：查询并返回armhf包的最新版本  
get_armhf_version() {  
    local pkg="$1"  
    local version=$(apt-cache madison "$pkg" | tail -n 1 | awk '{print $3}')  
    echo "$version"  
}  


# 函数：尝试安装特定版本的armhf包  
attempt_install() {  
    local pkg="$1"  
    local version="$2"  
    sudo apt-get install "$pkg"="$version" && {  
        echo "Successfully installed $pkg version $version"  
        return 0  
    } || {  
        echo "Failed to install $pkg version $version"  
        return 1  
    }  
}
  

# 遍历armhf包列表，并尝试安装对应版本的armhf包  
for pkg in "${armhf_packages[@]}"; do  
    # 获取arm64包名（去掉:armhf）  
    arm64_pkg="${pkg%:armhf}"  
    echo "Processing" $arm64_pkg
      
    # 检查arm64包是否已安装  
    if dpkg -s "$arm64_pkg" &> /dev/null; then  
      # 获取arm64包的版本号  
      arm64_version=$(dpkg -s "$arm64_pkg" | grep '^Version:' | awk '{print $2}')  
      echo "arm64 version is" $arm64_version
    fi

    # 检查armhf包是否已安装  
    if ! dpkg -s "$pkg" &> /dev/null; then  
      # 如果不存在，尝试找到可用的armhf包版本  
      armhf_versions=$(apt-cache madison "$pkg" | awk '{print $3}')  
      if [[ -n "$armhf_versions" ]]; then  
          # 从最高版本开始尝试安装
          for armhf_version in $armhf_versions; do  
              if attempt_install "$pkg" "$armhf_version"; then  
                  break  # 安装成功，跳出循环  
              fi  
          done  
          if [[ -z "$armhf_version" ]]; then  
              echo "No suitable version of $pkg found for armhf"  
          fi  
      else  
          echo "No versions of $pkg available for armhf"  
      fi  
    else
      armhf_version=$(dpkg -s "$pkg" | grep '^Version:' | awk '{print $2}')  
      echo "armhf version is" $armhf_version
    fi 
done
```

## 三、安装Box64

部分64位windows软件需要box86才能正常安装，因此需要同时安装box64和box86。（2024.12.10更新）  
运行以下bash脚本安装最新版的box64，也可以通过[源码编译安装](https://github.com/ptitSeb/box64/blob/main/docs/COMPILE.md)：

```c
# check if .list file already exists
if [ -f /etc/apt/sources.list.d/box64.list ]; then
  sudo rm -f /etc/apt/sources.list.d/box64.list || exit 1
fi
# check if .sources file already exists
if [ -f /etc/apt/sources.list.d/box64.sources ]; then
  sudo rm -f /etc/apt/sources.list.d/box64.sources || exit 1
fi
# download gpg key from specified url
if [ -f /usr/share/keyrings/box64-archive-keyring.gpg ]; then
  sudo rm -f /usr/share/keyrings/box64-archive-keyring.gpg
fi
sudo mkdir -p /usr/share/keyrings
wget -qO- "https://pi-apps-coders.github.io/box64-debs/KEY.gpg" | sudo gpg --dearmor -o /usr/share/keyrings/box64-archive-keyring.gpg
# create .sources file
echo "Types: deb
URIs: https://Pi-Apps-Coders.github.io/box64-debs/debian
Suites: ./
Signed-By: /usr/share/keyrings/box64-archive-keyring.gpg" | sudo tee /etc/apt/sources.list.d/box64.sources >/dev/null
sudo apt update
sudo apt install box64-generic-arm -y
```

这里也提供32位的Box86安装方法，如有需要可以输入以下命令安装最新版的Box86，也可以通过[源码编译安装](https://github.com/ptitSeb/box86/blob/master/docs/COMPILE.md)：

```c
sudo wget https://itai-nelken.github.io/weekly-box86-debs/debian/box86.list -O /etc/apt/sources.list.d/box86.list
wget -qO- https://itai-nelken.github.io/weekly-box86-debs/debian/KEY.gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/box86-debs-archive-keyring.gpg
sudo apt update && sudo apt install box86 -y
```

如果无法添加源，也可以直接在[下载页面](https://github.com/ryanfortner/box64-debs/tree/master/debian)下载最新的.deb安装包，然后通过dpkg -i安装。

## 四、安装Wine

`注意：这里需要安装X64版本的Wine64和X86版本的Wine，不能安装Arm版本的Wine64，Arm版的Wine64只能安装Arm版的Windows软件`  
将以下shell脚本保存至本地，根据需要修改branch分支信息和version版本信息，然后sudo bash执行以下[shell脚本](https://github.com/ptitSeb/box64/blob/main/docs/X64WINE.md)安装Wine64:

```c
	
	# NOTE: Can only run on aarch64 (since box64 can only run on aarch64)
	# box64 runs wine-amd64, box86 runs wine-i386.

	### User-defined Wine version variables ################
	# - Replace the variables below with your system's info.
	# - Note that we need the amd64 version for Box64 even though we're installing it on our ARM processor.
	# - Note that we need the i386 version for Box86 even though we're installing it on our ARM processor.
	# - Wine download links from WineHQ: https://dl.winehq.org/wine-builds/
  
	branch="staging" #example: devel, staging, or stable (wine-staging 4.5+ requires libfaudio0:i386)
	version="9.7" #example: "7.1"
	id="ubuntu" #example: debian, ubuntu
	dist="focal" #example (for debian): bullseye, buster, jessie, wheezy, ${VERSION_CODENAME}, etc 
	tag="-1" #example: -1 (some wine .deb files have -1 tag on the end and some don't)

  ########################################################

	# Clean up any old wine instances
	wineserver -k # stop any old wine installations from running
	rm -rf ~/.cache/wine # remove any old wine-mono/wine-gecko install files
	rm -rf ~/.local/share/applications/wine # remove any old program shortcuts

	# Backup any old wine installs
	rm -rf ~/wine-old 2>/dev/null; mv ~/wine ~/wine-old 2>/dev/null
	rm -rf ~/.wine-old 2>/dev/null; mv ~/.wine ~/.wine-old 2>/dev/null
	sudo mv /usr/local/bin/wine /usr/local/bin/wine-old 2>/dev/null
	sudo mv /usr/local/bin/wine64 /usr/local/bin/wine-old 2>/dev/null
	sudo mv /usr/local/bin/wineboot /usr/local/bin/wineboot-old 2>/dev/null
	sudo mv /usr/local/bin/winecfg /usr/local/bin/winecfg-old 2>/dev/null
	sudo mv /usr/local/bin/wineserver /usr/local/bin/wineserver-old 2>/dev/null

	# Wine download links from WineHQ: https://dl.winehq.org/wine-builds/
	# LNKA="https://dl.winehq.org/wine-builds/${id}/dists/${dist}/main/binary-amd64/" #amd64-wine links
	LNKA="https://mirrors.tuna.tsinghua.edu.cn/wine-builds/${id}/dists/${dist}/main/binary-amd64/" #amd64-wine links
	DEB_A1="wine-${branch}-amd64_${version}~${dist}${tag}_amd64.deb" #wine64 main bin
	DEB_A2="wine-${branch}_${version}~${dist}${tag}_amd64.deb" #wine64 support files (required for wine64 / can work alongside wine_i386 main bin)
		#DEB_A3="winehq-${branch}_${version}~${dist}${tag}_amd64.deb" #shortcuts & docs
	# LNKB="https://dl.winehq.org/wine-builds/${id}/dists/${dist}/main/binary-i386/" #i386-wine links
	LNKB="https://mirrors.tuna.tsinghua.edu.cn/wine-builds/${id}/dists/${dist}/main/binary-i386/" #i386-wine links
	DEB_B1="wine-${branch}-i386_${version}~${dist}${tag}_i386.deb" #wine_i386 main bin
	DEB_B2="wine-${branch}_${version}~${dist}${tag}_i386.deb" #wine_i386 support files (required for wine_i386 if no wine64 / CONFLICTS WITH wine64 support files)
	#DEB_B3="winehq-${branch}_${version}~${dist}${tag}_i386.deb" #shortcuts & docs

	# Install amd64-wine (64-bit) alongside i386-wine (32-bit)
	echo -e "Downloading wine . . ."
	wget -q ${LNKA}${DEB_A1} 
	#echo ${LNKA}${DEB_A1} 
	wget -q ${LNKA}${DEB_A2} 
	wget -q ${LNKB}${DEB_B1} 
	#wget -q ${LNKB}${DEB_B2} # 32-bits wine
	echo -e "Extracting wine . . ."
	dpkg-deb -x ${DEB_A1} wine-installer
	dpkg-deb -x ${DEB_A2} wine-installer
	dpkg-deb -x ${DEB_B1} wine-installer
	#dpkg-deb -x ${DEB_B2} wine-installer # 32-bits wine
	echo -e "Installing wine . . ."
	mv wine-installer/opt/wine* ~/wine
```

安装完成后，执行以下命令创建软链接并添加执行权限：

```c
# Install symlinks
sudo ln -s ~/wine/bin/wine /usr/local/bin/wine
sudo ln -s ~/wine/bin/wine64 /usr/local/bin/wine64
sudo ln -s ~/wine/bin/wineboot /usr/local/bin/wineboot
sudo ln -s ~/wine/bin/winecfg /usr/local/bin/winecfg
sudo ln -s ~/wine/bin/wineserver /usr/local/bin/wineserver
sudo chmod +x /usr/local/bin/wine /usr/local/bin/wine64 /usr/local/bin/wineboot /usr/local/bin/winecfg /usr/local/bin/wineserver
```

## 五、初始化wine配置

执行以下命令对wine进行初始化，可能出现一些错误提示，会影响特定Windows软件运行，暂时不用理会：

```c
wine64 wineboot
```

将自动在当前用户根目录下创建.wine文件夹，里面存放了运行Windows系统必备的库文件。  
安装过程中，终端窗口会有错误提示，暂时不用理会。Wine Mono安装器会有以下提示，需要安装.net运行库，安装时间较长，如果不需要安装基于.net架构的windows软件，可以点击取消，后续需要的时候再安装。![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/09445d3d490e614a684f50fcfc6a9793.png#pic_center)  
运行完成之后，打开~/.wine/drive_c目录，我们可以找到熟悉的Windows系统文件夹，包含了运行Windows应用所必须的库文件。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3a20ad60bc17593dd782c7eb255c5cbe.png#pic_center)  
将64位Windows应用的安装包上传到银河麒麟系统，然后可以通过以下命令直接安装Windwos应用，将${PathToYourProgramName}改为Windows安装包的路径:

```c
wine64 ${PathToYourProgramName}
```

这里只测试了常用的压缩软件7z，其它软件大家可以根据需要进行测试，由于部分运行库尚未安装，可能出现一些运行问题。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6338472bed0d47cfe9c87e9a771fa7fb.png#pic_center)  
这里选择安装目录为C:\Program Files\，也可以选择其它安装位置，安装完成后，进入~/.wine/drive_c/‘Program Files’/目录下，可以看到7-Zip已经安装好了。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/20df0714bbbdc02eb128c0985da133fb.png)  
输入以下命令即可启动7-Zip程序：

```c
wine64 7zFM.exe
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e4a5563328b1bdcbba4b9f65cbb4d6e8.png#pic_center)

应用窗口启动了，但是出现了乱码，这是由于缺少中文字体导致的，参考[前辈的文章](https://blog.csdn.net/hknaruto/article/details/136338425)，直接复制windows 系统c:/windows/fonts 目录到 ~/.wine/drive_c/windows/Fonts ，然后重新启动应用即可解决。

---

## 总结

本文介绍了基于Box64和Wine64实现在国产Arm版本银河麒麟系统上安装64位Windows应用，如果需要安装32位应用，其安装过程类似，只需将第三节更改为安装Box86，将第四节脚本关于wine64的部分注释掉，更改为下载和安装DEB_B1和DEB_B2即可。

由于不可描述的原因，也测试了XX版的银河麒麟，但是存在大量依赖冲突，无法安装，尝试了添加Ubuntu 源，Box64+Wine64安装成功，但是UKUI界面无法使用，无奈安装了slim和xfce桌面，使用上是没问题，但是Ukui是用不了了，如有需要，可以留言交流安装方法。

JC版和GF版的麒麟系统，更换2403源后，也可正常安装，有相关需求的，可以留言。（2024.12.10更新）

在撰写本文之前，查阅了大量前辈的文章，但是截止目前，还没有找到完整的Arm版银河麒麟运行Windows软件的文章，因为不可描述的原因，无奈下定决心解决这一问题，前前后后大概用了半个月的时间，总算有所收获。其实主要问题在于银河麒麟官方源的运行库存在大量冲突，稍有不慎，就可能导致Ukui无法运行，原生的Arm版Ubuntu基本不存在依赖冲突，可以直接安装。
