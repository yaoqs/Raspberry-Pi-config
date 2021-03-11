* 将本目录下文件放在/boot目录下
   * cmdline.txt 文件为系统初始化配置参数，根据需要修改相关参数，可在开头添加ip=192.168.173.1 来设置eth0以太网静态地址
   * [config.txt](https://www.raspberrypi.org/documentation/configuration/config-txt/README.md) 文件为树莓派参数配置文件，可根据需要修改相关参数，使其适配主板电流、显示器等
      * config_raw.txt 文件为备份的原始树莓派参数配置文件
   * softwaresrc.sh 文件为shell脚本，用于自动修改软件更新源，可自定义
      * 在/boot目录下 cd /boot
      * chmod +x ./softwaresrc.sh
      * sudo ./softwaresrc.sh
   * ssh 文件可使树莓派默认打开ssh功能 
   * wpa_supplicant.conf 文件为设置树莓派的默认网络连接配置文件，可使树莓派开机后，根据所设参数自动连接WIFI
     * windows下cmd命令查看wifi信息
      1. 查看wifi密码 
        ``` shell
        netsh wlan show profile name="SSID" key=clear 
        ```
      2. 查看电脑可以搜索到的所有wifi
        ```
        netsh wlan show networks mode=bssid
        ```
      3. 删除指定的wifi
        ```
        netsh wlan delete profile name="ko"
        ```
* 附录: 
  * /boot分区内都有哪些文件？
    树莓派引导文件存储在SD卡的第一个分区中，即/boot分区，该分区的文件系统是FAT，所以可以在Windows，macOS和Linux设备上读取这个分区。当树莓派通电时，它将从启动分区（/boot）中加载各种文件以启动各种处理器，然后启动Linux内核。当Linux启动后，启动分区将挂载为/boot。
  * /boot分区内的文件都有什么用？
    * bootcode.bin
      这是引导加载程序，由SoC在引导时加载，它执行一些非常基本的设置，然后加载其中一个start*.elf文件。Raspberry Pi 4上未使用bootcode.bin，因为它已由板载EEPROM中的启动代码替换。
    * start.elf，start_x.elf，start_db.elf，start_cd.elf，start4.elf，start4x.elf，start4cd.elf，start4db.elf
      这些是二进制Blob（固件），已加载到SoC中的VideoCore上，然后接管启动过程。 start.elf是基本固件，start_x.elf包括相机驱动程序和编解码器，start_db.elf是固件的调试版本，start_cd.elf是简化版本，不支持编解码器和3D之类的硬件模块，并且在gpu_mem=16中指定时使用config.txt。
      
      start4.elf，start4x.elf，start4cd.elf，和start4db.elf是树莓派4的固件文件。
    * fixup* .dat
      这些是链接器文件，与start*.elf上一节中列出的文件配对。
    * cmdline.txt
      引导时，内核命令行会传递到内核。例如：
      ···
      pi@raspberrypi:/boot $ cat cmdline.txt 
      console=serial0,115200 console=tty1 root=PARTUUID=d9b3f436-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait quiet splash plymouth.ignore-serial-consoles
      ···
    * config.txt
      包含许多用于设置树莓派的配置参数。例如：
      ···
      pi@raspberrypi:/boot $ cat config.txt
      # For more options and information see
      # http://rpf.io/configtxt
      # Some settings may impact device functionality. See link above for details

      # uncomment if you get no picture on HDMI for a default "safe" mode
      #hdmi_safe=1

      # uncomment this if your display has a black border of unused pixels visible
      # and your display can output without overscan
      #disable_overscan=1
      ```
    * issue.txt
      一些基于文本的管家信息，其中包含分发的日期和git commit ID。例如：
      ```
      pi@raspberrypi:/boot $ cat issue.txt 
      Raspberry Pi reference 2019-09-26
      Generated using pi-gen, https://github.com/RPi-Distro/pi-gen, 80d486687ea77d31fc3fc13cf3a2f8b464e129be, stage4
      ```
    * ssh或ssh.txt
      如果存在此文件，则将在启动时启用SSH。内容无关紧要，可以为空。否则默认情况下禁用SSH。
    * wpa_supplicant.conf
      这是用于配置无线网络设置的文件（如果硬件具备此功能）。编辑国家代码和网络部分以适合您的情况。使用方式可以参考这篇文章设置树莓派wifi的几种方式。
    * 设备树文件
      有各种设备树blob文件，其扩展名为.dtb。它们包含树莓派各种模型的硬件定义，并在启动时用于根据检测到的树莓派模型来设置内核。
    * 内核文件
      引导文件夹将包含用于不同树莓派型号的各种内核映像文件：
      |--|--|--|--|
      |文档名称 	|处理器 	|Raspberry Pi模型 	|笔记|
      |kernel.img 	|BCM2835 	|Pi zero，Pi 1 | |	
      |kernel7.img 	|BCM2836，BCM2837 	|Pi 2，Pi 3 	|后来的Pi 2使用BCM2837|
      |kernel7l.img 	|BCM2711 	|Pi 4 	|大型物理地址扩展（LPAE）|
      |kernel8.img 	|BCM2837，BCM2711 	|Pi 2，Pi 3，Pi 4 	|Beta 64位内核1。带有BCM2836的早期Pi 2不支持64位。|
      注：lscpu该架构报告是armv7l，表示用于32位系统（即除了kernel8.img一切），以及aarch64用于64位系统。所述的armv7l情况下指的是被架构小端，而不是LPAE如由所指示l的kernel7l.img文件名。
    * 设备树覆盖
      在overlays子文件夹中包含设备树覆盖。这些用于配置可能连接到系统的各种硬件设备，例如Raspberry Pi Touch Display或第三方音板。使用以下条目中的条目来选择这些覆盖层config.txt。
