# Raspberry-config
关于树莓派安装、配置、使用等的技巧、工具

# 目录及说明
## boot
* 参见boot目录下[README.md ](/boot/README.md)
> * 将boot目录下文件放在/boot目录下
>   * cmdline.txt 文件为系统初始化配置参数，根据需要修改相关参数，可在开头添加ip=192.168.173.1 来设置eth0以太网静态地址
>   * config.txt 文件为树莓派参数配置文件，可根据需要修改相关参数，使其适配主板电流、显示器等
>      * config_raw.txt 文件为备份的原始树莓派参数配置文件
>   * softwaresrc.sh 文件为shell脚本，用于自动修改软件更新源，可自定义
>      * 在/boot目录下 cd /boot
>      * chmod +x ./softwaresrc.sh
>      * sudo ./softwaresrc.sh
>   * ssh 文件可使树莓派默认打开ssh功能，由于新版本中树莓派系统默认禁用了 SSH 服务，因此在/boot目录下添加此文本可使树莓派默认打开ssh功能
>      * As of the November 2016 release, Raspbian has the SSH server disabled by default. 
>        * 出错的详细信息为：
>        * ssh: connect to host 192.168.43.220 port 22: Connection refused
>        * 官方的解决方案是：
>        * SSH disabled by default; can be enabled by creating a file with name “ssh” in boot partition 
>      * 或用hdmi对树莓派进行连接，在命令行界面输入打开ssh服务的命令
>        1. sudo systemctl enable ssh
>        2. sudo systemctl start ssh        
>    * wpa_supplicant.conf 文件为设置树莓派的默认网络连接配置文件，可使树莓派开机后，根据所设参数自动连接WIFI
## os
* 参见os目录下[README.md ](/os/README.md)
> # 树莓派系统下载地址（Raspberry OS download URL)
> [Download Page](https://www.raspberrypi.org/downloads/)
## toolkit
* 参见toolkit目录下[README.md ](/toolkit/README.md)
* menu
  * 刷机
    * 可从网上查找安全下载地址，或使用本人提供的，亲测安全有效
    * SD Card Formatter.rar SDCardFormatterv5_WinEN.zip
    * usbit.zip
    * win32diskimager-v0.9-binary.zip
  * 调试终端
    * 建议官网下载
    * [FCN_V3.6_FULL.zip 官网](https://github.com/boywhp/fcn)
      > * free connect your private network from anywhere
      > * FCN[free connect]是一款傻瓜式的一键接入私有网络的工具, fcn利用公共服务器以及数据加密技术实现：
    * [WinSCP-5.13.7-Setup.exe 官网](https://winscp.net/eng/index.php) 
      > * WinSCP 是一个 Windows 环境下使用的 SSH 的开源图形化 SFTP 客户端。同时支持 SCP 协议。它的主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件。 
      > * WinSCP is an open source free SFTP client, FTP client, WebDAV client, S3 client and SCP client for Windows. Its main function is file transfer between a local and a remote computer. Beyond this, WinSCP offers scripting and basic file manager functionality. 
    * [putty.exe 官网](https://www.chiark.greenend.org.uk/~sgtatham/putty/) 知名ssh软件
    * [sscom5.13.1.exe 官网](http://www.daxia.com/) 新版安全可靠强大，包含串口调试、tcp及udp通讯调试
  * PortScan.exe 用于在局域网内查找树莓派的IP

## 安装respbian.docx
* 关于树莓派安装、配置、使用等的教程
## 树莓派通过网线直连笔记本电脑共享上网 .docx
* 如题
