# [Raspberry-config](https://yaoqs.github.io/Raspberry-config/)
关于树莓派安装、配置、使用等的技巧、工具 \
About the skills and toolkits of installing, configuring raspbian and handbook for raspberry

[toc]

# 目录及说明
## boot
* 参见boot目录下[README.md ](/boot/README.md)
## os
* 参见os目录下[README.md ](/os/README.md)
## toolkit
* 参见toolkit目录下[README.md ](/toolkit/README.md)
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
      > * free connect your private network from anywhere
      > * FCN[free connect]是一款傻瓜式的一键接入私有网络的工具, fcn利用公共服务器以及数据加密技术实现：
    * [WinSCP.exe 官网](https://winscp.net/eng/index.php) 
      > * WinSCP 是一个 Windows 环境下使用的 SSH 的开源图形化 SFTP 客户端。同时支持 SCP 协议。它的主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件。 
      > * WinSCP is an open source free SFTP client, FTP client, WebDAV client, S3 client and SCP client for Windows. Its main function is file transfer between a local and a remote computer. Beyond this, WinSCP offers scripting and basic file manager functionality. 
    * [putty.exe 官网](https://www.chiark.greenend.org.uk/~sgtatham/putty/) 知名ssh软件
    * [sscom5.13.1.exe 官网](http://www.daxia.com/) 新版安全可靠强大，包含串口调试、tcp及udp通讯调试
  * PortScan.exe 用于在局域网内查找树莓派的IP

## 安装respbian.docx
* 关于树莓派安装、配置、使用等的教程
## 树莓派通过网线直连笔记本电脑共享上网 .docx
* 如题

## Python
* [RPi.GPIO 0.6.5](https://pypi.org/project/RPi.GPIO/)  
 > ```python
 > pip install RPi.GPIO
 > ```

# 树莓派常用命令集合
```
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

# kali in raspberry pi

1. [How to Install Kali Linux on Raspberry Pi? (Complete Guide)](https://raspberrytips.com/use-kali-linux-raspberry-pi/)
2. [How to install Kali Linux on a USB for the RaspberryPi?](https://raspberrypi.stackexchange.com/questions/106762/how-to-install-kali-linux-on-a-usb-for-the-raspberrypi)


# website
* [树莓派实验室 | Raspberry Pi中文资讯站，提供丰富的树莓派使用教程和DIY资讯](http://shumeipai.nxez.com)
* [NanoPi Embedded system for ARM SBC,Computer-on-Module and Custom Design]( http://www.nanopi.org/)
![](http://www.nanopi.org/image/index/top_logo.gif)
* [有哪些对树莓派的有趣改造和扩展应用？](https://www.zhihu.com/question/20697024)
* [raspbian release_notes.txt](http://downloads.raspberrypi.org/raspbian/release_notes.txt)
* [中国石油大学 (华东) Linux 协会 树莓派](https://upclinux.github.io/intro/09/raspberry-pi/)
* [Raspberry Pi用户指南 王伟 许金超 郭栋 梁黎颖 译 出版社：人民邮电出版社 出版日期：2013年8月  	ISBN：9787115323675](http://book.51cto.com/art/201312/420490.htm)
* [树莓派 嵌入式以太网社区](http://www.embed-net.com/forum-64-1.html)

# [Tutorial list](https://github.com/yaoqs/Raspberry-config/tree/master/Tutorial%20list)
个人收藏的raspberry pi/树莓派文章及教程

# License 许可证 & Copyright
* 版权声明：Copyright © 2019-2021 要庆生. All rights reserved. 未经本人同意请勿转载。经本人同意后转载时请注明出处。
* https://choosealicense.com/licenses/cc-by-sa-4.0/ ![](https://csdnimg.cn/release/phoenix/images/creativecommons/80x15.png)\
知识共享许可协议 版权声明：署名，允许他人基于本文进行创作，且必须基于与原先许可协议相同的许可协议分发本文([Creative Commons](http://creativecommons.org/licenses/by-sa/4.0/ ))
