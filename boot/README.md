* 将本目录下文件放在/boot目录下
   * cmdline.txt 文件为系统初始化配置参数，根据需要修改相关参数，可在开头添加ip=192.168.173.1 来设置eth0以太网静态地址
   * config.txt 文件为树莓派参数配置文件，可根据需要修改相关参数，使其适配主板电流、显示器等
      * config_raw.txt 文件为备份的原始树莓派参数配置文件
   * softwaresrc.sh 文件为shell脚本，用于自动修改软件更新源，可自定义
      * 在/boot目录下 cd /boot
      * chmod +x ./softwaresrc.sh
      * sudo ./softwaresrc.sh
   * ssh 文件可使树莓派默认打开ssh功能，由于新版本中树莓派系统默认禁用了 SSH 服务，因此在/boot目录下添加此文本可使树莓派默认打开ssh功能
      * As of the November 2016 release, Raspbian has the SSH server disabled by default. 
        * 出错的详细信息为：
        * ssh: connect to host 192.168.43.220 port 22: Connection refused
        * 官方的解决方案是：
        * SSH disabled by default; can be enabled by creating a file with name “ssh” in boot partition 
      * 或用hdmi对树莓派进行连接，在命令行界面输入打开ssh服务的命令
        1. sudo systemctl enable ssh
        2. sudo systemctl start ssh        
    * wpa_supplicant.conf 文件为设置树莓派的默认网络连接配置文件，可使树莓派开机后，根据所设参数自动连接WIFI
