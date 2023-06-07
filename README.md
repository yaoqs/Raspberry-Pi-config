# [Raspberry-config](https://yaoqs.github.io/Raspberry-config/)
<a id="markdown-raspberry-config" name="raspberry-config"></a>


关于树莓派安装、配置、使用等的技巧、工具 \
About the skills and toolkits of installing, configuring raspbian and handbook for raspberry

<!-- TOC -->

- [Raspberry-config](#raspberry-config)
  - [Install](#install)
  - [update](#update)
    - [固件更新](#固件更新)
    - [EEPROM Boot Loader更新](#eeprom-boot-loader更新)
  - [boot](#boot)
    - [参见boot目录下README.md](#参见boot目录下readmemd)
    - [u盘启动](#u盘启动)
  - [os/操作系统](#os操作系统)
    - [安装respbian.docx](#安装respbiandocx)
    - [kali in raspberry pi](#kali-in-raspberry-pi)
    - [Raspbian 操作系统](#raspbian-操作系统)
    - [raspberrypi/linux](#raspberrypilinux)
    - [Windows 10 IoT 系统](#windows-10-iot-系统)
  - [toolkit](#toolkit)
    - [\[树莓派通过网线直连笔记本电脑共享上网 .docx\](./树莓派通过网线直连笔记本电脑共享上网 .docx)](#树莓派通过网线直连笔记本电脑共享上网-docx树莓派通过网线直连笔记本电脑共享上网-docx)
  - [树莓派常用命令集合](#树莓派常用命令集合)
  - [Tutorial list](#tutorial-list)
  - [硬件](#硬件)
  - [集群/cluster](#集群cluster)
  - [软件](#软件)
    - [Python](#python)
  - [服务器管理](#服务器管理)
  - [References](#references)
  - [Recommendation](#recommendation)
  - [News](#news)
    - [树莓派 4 代- 全球最流行的 Linux 小型迷你电脑，性能大幅飙升！(支持4K / USB3.0)](#树莓派-4-代--全球最流行的-linux-小型迷你电脑性能大幅飙升支持4k--usb30)
    - [树莓派 4 代发布](#树莓派-4-代发布)
    - [树莓派 4 硬件配置](#树莓派-4-硬件配置)
    - [树莓派官方宣传片](#树莓派官方宣传片)
    - [树莓派有什么作用和用途？](#树莓派有什么作用和用途)
  - [License 许可证 \& Copyright](#license-许可证--copyright)

<!-- /TOC -->


## Install
<a id="markdown-install" name="install"></a>


现在树莓派提供全新的一键式安装方法：[imager](https://downloads.raspberrypi.org/imager/imager_latest.exe) 大大简化了开发人员的操作，提高了安装效率，降低了安装难度。[more...](https://www.raspberrypi.com/software/)

## update
<a id="markdown-update" name="update"></a>


```shell
getconf LONG_BIT        # 查看系统位数
uname -a            # kernel 版本
/opt/vc/bin/vcgencmd  version   # firmware版本
strings /boot/start.elf  |  grep VC_BUILD_ID    # firmware版本
cat /proc/version       # kernel
cat /etc/os-release     # OS版本资讯
cat /etc/issue          # Linux distro 版本
cat /etc/debian_version     # Debian版本编号
```

### 固件更新
<a id="markdown-%E5%9B%BA%E4%BB%B6%E6%9B%B4%E6%96%B0" name="%E5%9B%BA%E4%BB%B6%E6%9B%B4%E6%96%B0"></a>


sudo rpi-update

### EEPROM Boot Loader更新
<a id="markdown-eeprom-boot-loader%E6%9B%B4%E6%96%B0" name="eeprom-boot-loader%E6%9B%B4%E6%96%B0"></a>


sudo apt-get install rpi-eeprom
sudo rpi-eeprom-update -a

## boot
<a id="markdown-boot" name="boot"></a>


### 参见boot目录下[README.md](/boot/README.md)
<a id="markdown-%E5%8F%82%E8%A7%81boot%E7%9B%AE%E5%BD%95%E4%B8%8Breadme.md" name="%E5%8F%82%E8%A7%81boot%E7%9B%AE%E5%BD%95%E4%B8%8Breadme.md"></a>


### u盘启动
<a id="markdown-u%E7%9B%98%E5%90%AF%E5%8A%A8" name="u%E7%9B%98%E5%90%AF%E5%8A%A8"></a>


在树莓派3从u盘启动之前，需要从设置了使能usb boot mode的sd启动。该设置树莓派芯片上的OTP（一次性可编程内存），这将使得树莓派能够从usb设备启动。一旦设置了该选项，sd卡就不需要了。注意，OTP一旦做出任何改动，都不能复原。

你可以使用运行raspbian或者raspbian lite的sd卡来设置OTP,如果你还没有这样的sd卡，可按常规方式将系统烧入sd卡。
输入以下代码使能usb boot mode

```shell
echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt
```

这行代码将'program_usb_boot_mode=1'插入到/boot/cinfig.txt文件的末尾。使用sudo reboot命令重启树莓派后，用以下命令检查OTP设置：

```shell
$ vcgencmd otp_dump | grep 17:

17:3020000a
```

确保输出是’17:3020000a‘，如果输出不是这个，那就说明OTP没有设置成功。

之后，你可以在sd卡的config.txt末尾去掉program_usb_boot_mode=1这行代码，这样把这张sd用在其他树莓派上就不会设置OTP，注意，config.txt文件结尾不能有空行，你可以使用sudo nano /boot/config.txt命令使用nano 编辑器编辑config.txt文件。

## os/操作系统
<a id="markdown-os%2F%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F" name="os%2F%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F"></a>


树莓派官方的操作系统是Raspberry Pi OS:

树莓派系统下载地址(Raspberry OS download URL)[Download Page](https://www.raspberrypi.com/software/operating-systems/)

1. Raspberry Pi OS with desktop and recommended software

顾名思义，就是说带了图形化桌面系统和常用的推荐软件的版本，小白新手建议安装这个，免去后期单独安装软件的烦恼

2. Raspberry Pi OS with desktop

和第一个版本相比，带了图形化桌面系统，但没有常用的推荐软件，如果你的SD卡比较小，或者进阶者希望自己定义安装哪些软件则可以选择这个版本，后面自行定制安装所需要的软件

3. Raspberry Pi OS Lite

这个版本不带图形化桌面系统，则只有命令行界面(这才是真正的Linux OS的真面目:-)，如果你不需要图形化界面，或者你的设备是Raspberry Pi zero(w,h)等硬件配置比较低（CPU慢，内存小，SD卡特别小）的推荐安装这个版本。

### [安装respbian.docx](./安装respbian.docx)
<a id="markdown-%E5%AE%89%E8%A3%85respbian.docx" name="%E5%AE%89%E8%A3%85respbian.docx"></a>


### kali in raspberry pi
<a id="markdown-kali-in-raspberry-pi" name="kali-in-raspberry-pi"></a>


1. [How to Install Kali Linux on Raspberry Pi? (Complete Guide)](https://raspberrytips.com/use-kali-linux-raspberry-pi/)
2. [How to install Kali Linux on a USB for the RaspberryPi?](https://raspberrypi.stackexchange.com/questions/106762/how-to-install-kali-linux-on-a-usb-for-the-raspberrypi)

### Raspbian 操作系统
<a id="markdown-raspbian-%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F" name="raspbian-%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F"></a>


树莓派官方提供了 **Raspbian** 操作系统，一款基于 [Debian](https://www.iplaysoft.com/debian.html) 优化修改而来的 Linux 发行版，也是最常用的一个版本，专为树莓派而生，通用性强。

![Raspbian 系统](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspbian.jpg)

此外，你也能在官网下载到 [Ubuntu](https://www.iplaysoft.com/ubuntu.html) 类或其他诸如专为播放高清[电影](https://www.iplaysoft.com/tag/电影)而生的 OSMC、LibreELEC 等各种版本的系统，如下：

- Ubuntu MATE
- Ubuntu Core
- Ubuntu Server
- Windows 10 IoT Core
- OSMC
- LibreELEC
- PiNet
- RISC OS
- Weather Station
- IchigoJam RPi

你几乎完全可以将树莓派 4 当做一台完整的台式电脑来使用，而得益于性能的大幅提升，本次升级后，树莓派的应用范围将会又得到了扩展。

### raspberrypi/linux
<a id="markdown-raspberrypi%2Flinux" name="raspberrypi%2Flinux"></a>


[raspberrypi/linux](https://github.com/raspberrypi/linux) Kernel source tree for Raspberry Pi-provided kernel builds. Issues unrelated to the linux kernel should be posted on the community forum at <https://forums.raspberrypi.com/>

### Windows 10 IoT 系统
<a id="markdown-windows-10-iot-%E7%B3%BB%E7%BB%9F" name="windows-10-iot-%E7%B3%BB%E7%BB%9F"></a>


除了 Linux 之外，[微软](https://www.iplaysoft.com/tag/%E5%BE%AE%E8%BD%AF)也已经跟树莓派基金会达成合作以确保 [Windows 10](https://www.iplaysoft.com/windows10.html) 可以适配树莓派新款产品，如今完美适配树莓派 2 / 3 代的 **Windows 10 IoT core 物联网核心版**系统已经「免费」提供给用户下载。截稿为止，4 代似乎还未适配。

![Windows 10 IoT 树莓派](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/win10iot_raspberrypi.png)

[下载 Win10 IoT 物联网系统 for 树莓派](https://www.iplaysoft.com/windows10-iot.html)

## toolkit
<a id="markdown-toolkit" name="toolkit"></a>


- ~~参见toolkit目录下[README.md](/toolkit/README.md)~~
- menu
  - 刷机
    - ~~可从网上查找安全下载地址，或使用本人提供的，亲测安全有效~~
    - ~~[SD Card Formatter.rar](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/SD%20Card%20Formatter.rar) [SDCardFormatterv5_WinEN.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/SDCardFormatterv5_WinEN.zip)~~
    - ~~[usbit.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/usbit.zip)~~
    - ~~[win32diskimager.zip](https://github.com/yaoqs/Raspberry-config/blob/master/toolkit/%E5%88%B7%E6%9C%BA/win32diskimager-v0.9-binary.zip)~~
    - ~~balenaEtcher~~
    - [Raspberry Pi Imager](https://www.raspberrypi.com/software/) is the quick and easy way to install Raspberry Pi OS and other operating systems to a microSD card, ready to use with your Raspberry Pi.
  - 调试终端
    - 建议使用系统自带ssh
    - ~~[FCN.zip 官网](https://github.com/boywhp/fcn)~~
    - ~~[WinSCP.exe 官网](https://winscp.net/eng/index.php) 是一个 Windows 环境下使用的 SSH 的开源图形化 SFTP 客户端。同时支持 SCP 协议。它的主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件。WinSCP is an open source free SFTP client, FTP client, WebDAV client, S3 client and SCP client for Windows. Its main function is file transfer between a local and a remote computer. Beyond this, WinSCP offers scripting and basic file manager functionality.~~
    - [putty.exe 官网](https://www.chiark.greenend.org.uk/~sgtatham/putty/) 知名ssh软件
    - [sscom5.13.1.exe 官网](http://www.daxia.com/) 新版安全可靠强大，包含串口调试、tcp及udp通讯调试
  - PortScan.exe 用于在局域网内查找树莓派的IP
  - **nmap 功能强大的ip、端口扫描工具**

### [树莓派通过网线直连笔记本电脑共享上网 .docx](./树莓派通过网线直连笔记本电脑共享上网 .docx)
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE%E9%80%9A%E8%BF%87%E7%BD%91%E7%BA%BF%E7%9B%B4%E8%BF%9E%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%94%B5%E8%84%91%E5%85%B1%E4%BA%AB%E4%B8%8A%E7%BD%91-.docx" name="%E6%A0%91%E8%8E%93%E6%B4%BE%E9%80%9A%E8%BF%87%E7%BD%91%E7%BA%BF%E7%9B%B4%E8%BF%9E%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%94%B5%E8%84%91%E5%85%B1%E4%BA%AB%E4%B8%8A%E7%BD%91-.docx"></a>


## 树莓派常用命令集合
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E9%9B%86%E5%90%88" name="%E6%A0%91%E8%8E%93%E6%B4%BE%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E9%9B%86%E5%90%88"></a>


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

## [Tutorial list](https://github.com/yaoqs/Raspberry-config/tree/master/Tutorial%20list)
<a id="markdown-tutorial-list" name="tutorial-list"></a>


个人收藏的raspberry pi/树莓派文章及教程

## 硬件
<a id="markdown-%E7%A1%AC%E4%BB%B6" name="%E7%A1%AC%E4%BB%B6"></a>


- [Raspberry Pi PCIe Devices](https://pipci.jeffgeerling.com/)
- [raspberry-pi-pcie-devices](https://github.com/geerlingguy/raspberry-pi-pcie-devices) Raspberry Pi PCI Express device compatibility database
- [pico-usbtest](https://github.com/notro/pico-usbtest) usbtest for Raspberry Pi Pico
- [pico-usb-io-board](https://github.com/notro/pico-usb-io-board) Raspberry Pi Pico USB I/O Board
- [树莓派OLED模块的使用教程大量例程详解](https://www.cnblogs.com/ChuanYangRiver/p/15305203.html)
- [树莓派 使用C对GPIO编程](https://www.cnblogs.com/lxz365/articles/12831076.html)

## 集群/cluster
<a id="markdown-%E9%9B%86%E7%BE%A4%2Fcluster" name="%E9%9B%86%E7%BE%A4%2Fcluster"></a>


- Linux 集群之美
- [多角度展现 Linux 集群之美](https://blog.csdn.net/hzbooks/article/details/121882513)
- [k3s-ansible](https://github.com/k3s-io/k3s-ansible) Build a Kubernetes cluster using k3s via Ansible,Author: <https://github.com/itwars>
- [pi-cluster](https://github.com/geerlingguy/pi-cluster) Raspberry Pi Cluster automation
- [raspberry-pi-dramble](https://github.com/geerlingguy/raspberry-pi-dramble) DEPRECATED - Raspberry Pi Kubernetes cluster that runs HA/HP Drupal 8;A cluster (Bramble) of Raspberry Pis on which Drupal is deployed using Ansible and Kubernetes. [pi-cluster](https://github.com/geerlingguy/pi-cluster) project for active development
- [clusterhat-image](https://github.com/burtyb/clusterhat-image) Converts Raspbian/Raspberry Pi OS images to support Cluster HAT

- [花式玩转Linux集群免密登录](https://juejin.cn/post/7086088679189381134)
- [linux集群服务器搭建](https://blog.csdn.net/manongxianfeng/article/details/113935886)
- [如何搭建一个小型高性能计算集群？](https://www.zhihu.com/question/281519069)

- [mqtt-cluster](https://gitee.com/quickmsg/mqtt-cluster) 一款高性、高吞吐量、高扩展性的物联网mqtt集群broker！支持千万级+链接，同时支持自定义扩展功能，功能非常强大！
- [SMQTT](https://www.smqtt.cc/) 一款高性能&开源的MQTT服务器，支持单机、容器化、集群部署，支持多种协议，具备低延迟，高吞吐量，支持百万TCP连接。MQTT协议提供了一种使用发布/订阅模型执行消息传递的轻量级方法。这使得它适用于物联网消息传递，如低功耗传感器或手机、嵌入式计算机或微控制器等移动设备。SMQTTX是一个开源mqtt消息代理服务器，它实现了MQTT协议版本3.1.1和3.1。SMQTTX是轻量级，适用于从低功耗单板计算机到全服务器的所有设备。SMQTTX非常友好地支持快速配置，对于java应用可以非常容易完成二次开发，并且拥有高性能高吞吐量的mqtt服务

## 软件
<a id="markdown-%E8%BD%AF%E4%BB%B6" name="%E8%BD%AF%E4%BB%B6"></a>


- [internet-pi](https://github.com/geerlingguy/internet-pi) A Raspberry Pi Configuration for Internet connectivity

### Python
<a id="markdown-python" name="python"></a>


- [RPi.GPIO 0.6.5](https://pypi.org/project/RPi.GPIO/)
 >
 > ```python
 > pip install RPi.GPIO
 > ```

## 服务器管理
<a id="markdown-%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AE%A1%E7%90%86" name="%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AE%A1%E7%90%86"></a>


- [iis7服务器管理工具](http://fwqglgj.iis7.net/) mstsc远程桌面、linux、ssh、sftp、vnc、ftp、webshell（批量管理），链接类客户端软件下载。
- wol/wakeonlan
  - [go-wol](https://github.com/sabhiram/go-wol) describes a simple data link layer protocol which tells a listening ethernet interface to power the target machine up.
  - [WakeOnLAN](https://github.com/GramThanos/WakeOnLAN) A simple C program that sends a magic packet
  - [wakemeonlan](https://www.nirsoft.net/utils/wake_on_lan.html)

## References
<a id="markdown-references" name="references"></a>


- [树莓派实验室 | Raspberry Pi中文资讯站，提供丰富的树莓派使用教程和DIY资讯](http://shumeipai.nxez.com)
- [NanoPi Embedded system for ARM SBC,Computer-on-Module and Custom Design]( http://www.nanopi.org/)
![](http://www.nanopi.org/image/index/top_logo.gif)
- [有哪些对树莓派的有趣改造和扩展应用？](https://www.zhihu.com/question/20697024)
- [raspbian release_notes.txt](http://downloads.raspberrypi.org/raspbian/release_notes.txt)
- [中国石油大学 (华东) Linux 协会 树莓派](https://upclinux.github.io/intro/09/raspberry-pi/)
- [Raspberry Pi用户指南 王伟 许金超 郭栋 梁黎颖 译 出版社：人民邮电出版社 出版日期：2013年8月   ISBN：9787115323675](http://book.51cto.com/art/201312/420490.htm)
- [树莓派 嵌入式以太网社区](http://www.embed-net.com/forum-64-1.html)
- [自己制作树莓派3B+ 64位系统（编译内核+rootfs制作）](https://blog.csdn.net/m0_49475727/article/details/109247979)

## Recommendation
<a id="markdown-recommendation" name="recommendation"></a>


- [Raspberry Pi Documentation](https://www.raspberrypi.com/documentation/) official document
- [blackout314/awesome-raspberry-pi](https://github.com/blackout314/awesome-raspberry-pi)

## News
<a id="markdown-news" name="news"></a>

### [树莓派 4 代](https://www.iplaysoft.com/raspberrypi.html)- 全球最流行的 Linux 小型迷你电脑，性能大幅飙升！(支持4K / USB3.0)
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E4%BB%A3--%E5%85%A8%E7%90%83%E6%9C%80%E6%B5%81%E8%A1%8C%E7%9A%84-linux-%E5%B0%8F%E5%9E%8B%E8%BF%B7%E4%BD%A0%E7%94%B5%E8%84%91%EF%BC%8C%E6%80%A7%E8%83%BD%E5%A4%A7%E5%B9%85%E9%A3%99%E5%8D%87%EF%BC%81%E6%94%AF%E6%8C%814k-%2F-usb3.0" name="%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E4%BB%A3--%E5%85%A8%E7%90%83%E6%9C%80%E6%B5%81%E8%A1%8C%E7%9A%84-linux-%E5%B0%8F%E5%9E%8B%E8%BF%B7%E4%BD%A0%E7%94%B5%E8%84%91%EF%BC%8C%E6%80%A7%E8%83%BD%E5%A4%A7%E5%B9%85%E9%A3%99%E5%8D%87%EF%BC%81%E6%94%AF%E6%8C%814k-%2F-usb3.0"></a>


![raspberrypi4_banner](_v_images/20190810214043362_22472.jpg)
被誉为 “世界上最流行最便宜的小型电脑” 的「[树莓派](https://www.iplaysoft.com/go/raspberrypi)」**Raspberry Pi** 是一款性价比超高的迷你电脑主机 (仅有信用卡大小)，深受全球开发者、极客、技术爱好者们的追捧和喜爱。

树莓派可以安装多种 [Linux](https://www.iplaysoft.com/os/linux-platform) 系统发行版 (官方为 [Debian](https://www.iplaysoft.com/debian.html) 的衍生版)，可当[服务器](https://www.iplaysoft.com/tag/%E6%9C%8D%E5%8A%A1%E5%99%A8)搭建各种网站、应用服务来使用，也能用来学习[编程](https://www.iplaysoft.com/tag/编程)、控制硬件或日常[办公](https://www.iplaysoft.com/tag/办公)。由于树莓派的体积很小很轻，并且功能极其丰富强大，这也使得它的应用范围和潜力几乎是无限的……

### 树莓派 4 代发布
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E4%BB%A3%E5%8F%91%E5%B8%83" name="%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E4%BB%A3%E5%8F%91%E5%B8%83"></a>


如今 **Raspberry Pi 4** 「[树莓派 4 代](https://www.iplaysoft.com/go/raspberrypi)」终于正式发布了！！官方定价依然是 $35 美元起不变，但整体性能相比 3 代要提升了三倍之多！多媒体性能为四倍，即使同时外接两台 [4K](https://www.iplaysoft.com/tag/4k) 显示器双屏工作也毫无鸭梨。这么小的体积加上如此强劲的性能，这将是一款再次改变行业规则的产品。

[![树莓派4](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi4.jpg)](https://www.iplaysoft.com/go/raspberrypi)

而且除了 [Linux](https://www.iplaysoft.com/os/linux-platform) 外，树莓派还能运行「[免费的 Win 10 物联网版系统](https://www.iplaysoft.com/windows10-iot.html)」！无论是[学习](https://www.iplaysoft.com/tag/%E5%AD%A6%E4%B9%A0)、办公、[编程](https://www.iplaysoft.com/tag/编程)、搭建智能家居、工控设备、还是用于特定的工作场景，树莓派都是最理想的小型电脑。也是每一个喜欢折腾电脑、折腾数码、折腾程序的朋友的必备玩物。

### 树莓派 4 硬件配置
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E7%A1%AC%E4%BB%B6%E9%85%8D%E7%BD%AE" name="%E6%A0%91%E8%8E%93%E6%B4%BE-4-%E7%A1%AC%E4%BB%B6%E9%85%8D%E7%BD%AE"></a>


**树莓派四代** (Raspberry Pi 4 Model B) 在硬件方面迎来了巨大的[升级](https://www.iplaysoft.com/tag/升级)！首次搭载了 4GB 的内存 (1G / 2G / 4G 可选)，并且引入 **USB 3.0** 接口，同时支持双屏 4K 输出和 H.265 硬件解码；处理器搭载了博通 1.5GHz 的四核 ARM Cortex-A72 处理器，性能提升可谓是质的飞跃。

![树莓派4](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_image.jpg)

接口方面，[树莓派 4](https://www.iplaysoft.com/raspberrypi.html) 支持双频无线 Wi-Fi (802.11ac)、搭载蓝牙 5.0，提供两个 Micro HDMI 2.0 视频输出接口，支持 4K 60FPS；内置千兆以太网口 (支持 PoE 供电)、MIPI DSI接口、MIPI CSI 相机接口、立体声耳机接口、2 个 USB 3.0 和 2 个 USB 2.0，扩展接口则依然是 40 针的 GPIO。供电方面也改成了 5V/3A 的 USB-C 接口供电，**升级可以说是全方位的**。

![树莓派4硬件规格](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi4_model.jpg)

新的**树莓派**几乎可兼容所有以往创建的树莓派项目、配件和应用。同时，其40针扩展 GPIO 接口使其能够添加更多传感器、连接器及扩展板或智能设备，前26针引脚与A型板和B型板保持一致，可 100% 向后兼容，无需担心软硬件和配件的生态问题。

### 树莓派官方宣传片
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE%E5%AE%98%E6%96%B9%E5%AE%A3%E4%BC%A0%E7%89%87" name="%E6%A0%91%E8%8E%93%E6%B4%BE%E5%AE%98%E6%96%B9%E5%AE%A3%E4%BC%A0%E7%89%87"></a>


如果你的[工作](https://www.iplaysoft.com/tag/工作)大多可以在 Linux 下完成的话，比如开发，或者用 [WPS for Linux](https://www.iplaysoft.com/wps-for-linux.html) 写文档、上网、收发邮件等，那么直接将树莓派随身携带，上下班通勤或出差时，也许会比带一个笨重的笔记本要轻松方便得多。

### 树莓派有什么作用和用途？
<a id="markdown-%E6%A0%91%E8%8E%93%E6%B4%BE%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E7%94%A8%E5%92%8C%E7%94%A8%E9%80%94%EF%BC%9F" name="%E6%A0%91%E8%8E%93%E6%B4%BE%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E7%94%A8%E5%92%8C%E7%94%A8%E9%80%94%EF%BC%9F"></a>


起初，树莓派是为鼓励孩子们学习[编程](https://www.iplaysoft.com/tag/%E7%BC%96%E7%A8%8B)和计算机知识而推出的奇趣硬件。但如今，除了教育领域，树莓派在硬件编程、智能家居、极客和计算机技术爱好者中的受欢迎程度完全超出想象。

![树莓派桌面](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_desktop.jpg)

随着新版本硬件性能的提升，以及全球极其大量玩家们的青睐，树莓派的玩法和实用性已经丰富到无法统计的地步了。直接当[办公](https://www.iplaysoft.com/tag/办公)电脑使用、丢在家里当 NAS、[离线下载](https://www.iplaysoft.com/bt-cloud-download.html)、做代理服务器、VP那个N、搭建个人[网站](https://www.iplaysoft.com/tag/网站)、私有[网盘](https://www.iplaysoft.com/tag/网盘)、搭建智能家居中枢、小型影音播放机，使用各种[开源](https://www.iplaysoft.com/tag/开源) Linux 程序给局域网提供服务等都是非常常见的用途。

![树莓派应用](https://img.iplaysoft.com/wp-content/uploads/2019/raspberrypi/raspberrypi_game.jpg)

总之，树莓派不仅会为**学习编程**带来更好的体验；给专业人士带来更强大高效稳定的硬件平台；对于爱好者们，新的树莓派也提供了更大的发挥空间——因为它完全就能一台性能充足的台式电脑那样，可以做到几乎任何事情！
当然，这也是一个需要脑洞大开的硬件，你可以把它玩成[神器](https://www.iplaysoft.com/tag/神器)，也能让它积灰几尺厚，这需要有想象力或自身有确切的需求。

## License 许可证 & Copyright
<a id="markdown-license-%E8%AE%B8%E5%8F%AF%E8%AF%81-%26-copyright" name="license-%E8%AE%B8%E5%8F%AF%E8%AF%81-%26-copyright"></a>


- 版权声明：Copyright © 2019-2023 要庆生. All rights reserved. 未经本人同意请勿转载。经本人同意后转载时请注明出处。
- <https://choosealicense.com/licenses/cc-by-sa-4.0/> ![](https://csdnimg.cn/release/phoenix/images/creativecommons/80x15.png)\
知识共享许可协议 版权声明：署名，允许他人基于本文进行创作，且必须基于与原先许可协议相同的许可协议分发本文([Creative Commons](http://creativecommons.org/licenses/by-sa/4.0/ ))
