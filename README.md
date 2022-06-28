# [Raspberry-config](https://yaoqs.github.io/Raspberry-config/)

关于树莓派安装、配置、使用等的技巧、工具 \
About the skills and toolkits of installing, configuring raspbian and handbook for raspberry

[toc]

## 目录及说明

### boot

* 参见boot目录下[README.md](/boot/README.md)

### os

* 参见os目录下[README.md](/os/README.md)

### toolkit

* 参见toolkit目录下[README.md](/toolkit/README.md)
* menu
  * 刷机
    * 可从网上查找安全下载地址，或使用本人提供的，亲测安全有效
    * [SD Card Formatter.rar](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/SD%20Card%20Formatter.rar) [SDCardFormatterv5_WinEN.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/SDCardFormatterv5_WinEN.zip)
    * [usbit.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/usbit.zip)
    * [win32diskimager.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/win32diskimager-v0.9-binary.zip)
    * balenaEtcher
  * 调试终端
    * 建议官网下载
    * [FCN.zip 官网](https://github.com/boywhp/fcn)
      >
      > * free connect your private network from anywhere
      > * FCN[free connect]是一款傻瓜式的一键接入私有网络的工具, fcn利用公共服务器以及数据加密技术实现：
      >
    * [WinSCP.exe 官网](https://winscp.net/eng/index.php)
      >
      > * WinSCP 是一个 Windows 环境下使用的 SSH 的开源图形化 SFTP 客户端。同时支持 SCP 协议。它的主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件。
      > * WinSCP is an open source free SFTP client, FTP client, WebDAV client, S3 client and SCP client for Windows. Its main function is file transfer between a local and a remote computer. Beyond this, WinSCP offers scripting and basic file manager functionality.
      >
    * [putty.exe 官网](https://www.chiark.greenend.org.uk/~sgtatham/putty/) 知名ssh软件
    * [sscom5.13.1.exe 官网](http://www.daxia.com/) 新版安全可靠强大，包含串口调试、tcp及udp通讯调试
  * PortScan.exe 用于在局域网内查找树莓派的IP

### [安装respbian.docx](./安装respbian.docx)

* 关于树莓派安装、配置、使用等的教程

### [树莓派通过网线直连笔记本电脑共享上网 .docx](./树莓派通过网线直连笔记本电脑共享上网 .docx)

* 如题

### Python

* [RPi.GPIO 0.6.5](https://pypi.org/project/RPi.GPIO/)
 >
 > ```python
 > pip install RPi.GPIO
 > ```

## 树莓派常用命令集合

```bash
 sudo apt-get update
 sudo apt-get upgrade
 sudo apt-get install vim
 cd /etc/apt/
 sudo vim /etc/apt/sources.list
 ping ustc.edu.cn
 sudo passwd
 reboot
 sudo reboot
 su root
 sudo passwd pi
 sudo apt-get install python-pip
 sudo apt-get install fuse-utils ntfs-3g
 modprobe fuse
 sudo modprobe fuse
 sudo vim /etc/fstab
 sudo apt-get install exfat-fuse
 sudo pip install bpython
 sudo apt-get install python-dev
 sudo pip install ipython
 sudo apt-get install nmap
 sudo pip install ipython
 sudo pip install bpython
 sudo pip install virtualenv
 df
 df -lh
 fdisk -l
 sudo fdisk -l
 df -h
 sudo fdisk /dev/mmcblk0
 sudo reboot
 sudo resize2fs /dev/mmcblk0p2
 sudo apt-get install -y dnsmasq
 sudo vim /etc/dnsmasq.conf
 sudo service dnsmasq restart
 dig
 sudo apt-get install dnsutils
 ls
 dig www.baidu.com
 sudo vim /etc/dnsmasq.conf
 sudo service dnsmasq restart
 dig www.baidu.com
 sudo vim /etc/resolv.conf
 sudo service dnsmasq restart
 dig www.baidu.com
 chkconfig dnsmapq on
 find resolv.dnsmapq.conf
 sudo vim /etc/dnsmasq.conf
 netstat -lpnt
 ps -ef
 ifconfig
 sudo service --status-all
 service dnsmasq status
 sudo apt-get install git
 sudo easy_install -U distribute
 sudo pip install rpi.gpio
 alias ll='ls -lh'
 source /etc/profile
 sudo raspi-config
```

## kali in raspberry pi

1. [How to Install Kali Linux on Raspberry Pi? (Complete Guide)](https://raspberrytips.com/use-kali-linux-raspberry-pi/)
2. [How to install Kali Linux on a USB for the RaspberryPi?](https://raspberrypi.stackexchange.com/questions/106762/how-to-install-kali-linux-on-a-usb-for-the-raspberrypi)

## website

* [树莓派实验室 | Raspberry Pi中文资讯站，提供丰富的树莓派使用教程和DIY资讯](http://shumeipai.nxez.com)
* [NanoPi Embedded system for ARM SBC,Computer-on-Module and Custom Design]( http://www.nanopi.org/)
![](http://www.nanopi.org/image/index/top_logo.gif)
* [有哪些对树莓派的有趣改造和扩展应用？](https://www.zhihu.com/question/20697024)
* [raspbian release_notes.txt](http://downloads.raspberrypi.org/raspbian/release_notes.txt)
* [中国石油大学 (华东) Linux 协会 树莓派](https://upclinux.github.io/intro/09/raspberry-pi/)
* [Raspberry Pi用户指南 王伟 许金超 郭栋 梁黎颖 译 出版社：人民邮电出版社 出版日期：2013年8月   ISBN：9787115323675](http://book.51cto.com/art/201312/420490.htm)
* [树莓派 嵌入式以太网社区](http://www.embed-net.com/forum-64-1.html)

## [Tutorial list](https://github.com/yaoqs/Raspberry-config/tree/master/Tutorial%20list)

个人收藏的raspberry pi/树莓派文章及教程

## [树莓派 4 代 - 全球最流行的 Linux 小型迷你电脑，性能大幅飙升！(支持4K / USB3.0)](https://www.iplaysoft.com/raspberrypi.html)

![raspberrypi4_banner](_v_images/20190810214043362_22472.jpg)
被誉为 “世界上最流行最便宜的小型电脑” 的「[树莓派](https://www.iplaysoft.com/go/raspberrypi)」**Raspberry Pi** 是一款性价比超高的迷你电脑主机 (仅有信用卡大小)，深受全球开发者、极客、技术爱好者们的追捧和喜爱。

树莓派可以安装多种 [Linux](https://www.iplaysoft.com/os/linux-platform) 系统发行版 (官方为 [Debian](https://www.iplaysoft.com/debian.html) 的衍生版)，可当[服务器](https://www.iplaysoft.com/tag/%E6%9C%8D%E5%8A%A1%E5%99%A8)搭建各种网站、应用服务来使用，也能用来学习[编程](https://www.iplaysoft.com/tag/编程)、控制硬件或日常[办公](https://www.iplaysoft.com/tag/办公)。由于树莓派的体积很小很轻，并且功能极其丰富强大，这也使得它的应用范围和潜力几乎是无限的……

### 树莓派 4 代发布

如今 **Raspberry Pi 4** 「[树莓派 4 代](https://www.iplaysoft.com/go/raspberrypi)」终于正式发布了！！官方定价依然是 $35 美元起不变，但整体性能相比 3 代要提升了三倍之多！多媒体性能为四倍，即使同时外接两台 [4K](https://www.iplaysoft.com/tag/4k) 显示器双屏工作也毫无鸭梨。这么小的体积加上如此强劲的性能，这将是一款再次改变行业规则的产品。

[![树莓派4](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi4.jpg)](https://www.iplaysoft.com/go/raspberrypi)

而且除了 [Linux](https://www.iplaysoft.com/os/linux-platform) 外，树莓派还能运行「[免费的 Win 10 物联网版系统](https://www.iplaysoft.com/windows10-iot.html)」！无论是[学习](https://www.iplaysoft.com/tag/%E5%AD%A6%E4%B9%A0)、办公、[编程](https://www.iplaysoft.com/tag/编程)、搭建智能家居、工控设备、还是用于特定的工作场景，树莓派都是最理想的小型电脑。也是每一个喜欢折腾电脑、折腾数码、折腾程序的朋友的必备玩物。

### 树莓派 4 硬件配置

**树莓派四代** (Raspberry Pi 4 Model B) 在硬件方面迎来了巨大的[升级](https://www.iplaysoft.com/tag/升级)！首次搭载了 4GB 的内存 (1G / 2G / 4G 可选)，并且引入 **USB 3.0** 接口，同时支持双屏 4K 输出和 H.265 硬件解码；处理器搭载了博通 1.5GHz 的四核 ARM Cortex-A72 处理器，性能提升可谓是质的飞跃。

![树莓派4](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_image.jpg)

接口方面，[树莓派 4](https://www.iplaysoft.com/raspberrypi.html) 支持双频无线 Wi-Fi (802.11ac)、搭载蓝牙 5.0，提供两个 Micro HDMI 2.0 视频输出接口，支持 4K 60FPS；内置千兆以太网口 (支持 PoE 供电)、MIPI DSI接口、MIPI CSI 相机接口、立体声耳机接口、2 个 USB 3.0 和 2 个 USB 2.0，扩展接口则依然是 40 针的 GPIO。供电方面也改成了 5V/3A 的 USB-C 接口供电，**升级可以说是全方位的**。

![树莓派4硬件规格](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi4_model.jpg)

新的**树莓派**几乎可兼容所有以往创建的树莓派项目、配件和应用。同时，其40针扩展 GPIO 接口使其能够添加更多传感器、连接器及扩展板或智能设备，前26针引脚与A型板和B型板保持一致，可 100% 向后兼容，无需担心软硬件和配件的生态问题。

### Raspbian 操作系统

树莓派官方提供了 **Raspbian** 操作系统，一款基于 [Debian](https://www.iplaysoft.com/debian.html) 优化修改而来的 Linux 发行版，也是最常用的一个版本，专为树莓派而生，通用性强。

![Raspbian 系统](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspbian.jpg)

此外，你也能在官网下载到 [Ubuntu](https://www.iplaysoft.com/ubuntu.html) 类或其他诸如专为播放高清[电影](https://www.iplaysoft.com/tag/电影)而生的 OSMC、LibreELEC 等各种版本的系统，如下：

* Ubuntu MATE
* Ubuntu Core
* Ubuntu Server
* Windows 10 IoT Core
* OSMC
* LibreELEC
* PiNet
* RISC OS
* Weather Station
* IchigoJam RPi

你几乎完全可以将树莓派 4 当做一台完整的台式电脑来使用，而得益于性能的大幅提升，本次升级后，树莓派的应用范围将会又得到了扩展。

#### 树莓派官方宣传片

如果你的[工作](https://www.iplaysoft.com/tag/工作)大多可以在 Linux 下完成的话，比如开发，或者用 [WPS for Linux](https://www.iplaysoft.com/wps-for-linux.html) 写文档、上网、收发邮件等，那么直接将树莓派随身携带，上下班通勤或出差时，也许会比带一个笨重的笔记本要轻松方便得多。

### Windows 10 IoT 系统

除了 Linux 之外，[微软](https://www.iplaysoft.com/tag/%E5%BE%AE%E8%BD%AF)也已经跟树莓派基金会达成合作以确保 [Windows 10](https://www.iplaysoft.com/windows10.html) 可以适配树莓派新款产品，如今完美适配树莓派 2 / 3 代的 **Windows 10 IoT core 物联网核心版**系统已经「免费」提供给用户下载。截稿为止，4 代似乎还未适配。

![Windows 10 IoT 树莓派](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/win10iot_raspberrypi.png)

[下载 Win10 IoT 物联网系统 for 树莓派](https://www.iplaysoft.com/windows10-iot.html)

### 树莓派有什么作用和用途？

起初，树莓派是为鼓励孩子们学习[编程](https://www.iplaysoft.com/tag/%E7%BC%96%E7%A8%8B)和计算机知识而推出的奇趣硬件。但如今，除了教育领域，树莓派在硬件编程、智能家居、极客和计算机技术爱好者中的受欢迎程度完全超出想象。

![树莓派桌面](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_desktop.jpg)

随着新版本硬件性能的提升，以及全球极其大量玩家们的青睐，树莓派的玩法和实用性已经丰富到无法统计的地步了。直接当[办公](https://www.iplaysoft.com/tag/办公)电脑使用、丢在家里当 NAS、[离线下载](https://www.iplaysoft.com/bt-cloud-download.html)、做代理服务器、VP那个N、搭建个人[网站](https://www.iplaysoft.com/tag/网站)、私有[网盘](https://www.iplaysoft.com/tag/网盘)、搭建智能家居中枢、小型影音播放机，使用各种[开源](https://www.iplaysoft.com/tag/开源) Linux 程序给局域网提供服务等都是非常常见的用途。

![树莓派应用](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_game.jpg)

总之，树莓派不仅会为**学习编程**带来更好的体验；给专业人士带来更强大高效稳定的硬件平台；对于爱好者们，新的树莓派也提供了更大的发挥空间——因为它完全就能一台性能充足的台式电脑那样，可以做到几乎任何事情！
当然，这也是一个需要脑洞大开的硬件，你可以把它玩成[神器](https://www.iplaysoft.com/tag/神器)，也能让它积灰几尺厚，这需要有想象力或自身有确切的需求。

## License 许可证 & Copyright

* 版权声明：Copyright © 2019-2022 要庆生. All rights reserved. 未经本人同意请勿转载。经本人同意后转载时请注明出处。
* <https://choosealicense.com/licenses/cc-by-sa-4.0/> ![](https://csdnimg.cn/release/phoenix/images/creativecommons/80x15.png)\
知识共享许可协议 版权声明：署名，允许他人基于本文进行创作，且必须基于与原先许可协议相同的许可协议分发本文([Creative Commons](http://creativecommons.org/licenses/by-sa/4.0/ ))
