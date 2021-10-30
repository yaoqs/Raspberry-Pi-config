# boot config introduction

1. [raspberrypi.org > configuration](https://www.raspberrypi.org/documentation/configuration/)

    Configuration

    Some basic guides to configuring your Raspberry Pi.

    Contents

    + [raspi-config](https://www.raspberrypi.org/documentation/configuration/raspi-config.md)
  
        The Raspberry Pi configuration tool in Raspberry Pi OS, which allows you to easily enable features such as the camera, and to change your specific settings such as keyboard layout
    + [config.txt](https://www.raspberrypi.org/documentation/configuration/config-txt/README.md)

        The Raspberry Pi configuration file
    + [TCP/IP networking](https://www.raspberrypi.org/documentation/configuration/tcpip/README.md)

        Configuring the TCP/IP network stack on the Raspberry Pi
    + [Connect to a wireless network](https://www.raspberrypi.org/documentation/configuration/wireless/README.md)

        Configuring your Pi to connect to a wireless network using the Raspberry Pi 4, Raspberry Pi 3 or Pi Zero W's inbuilt wireless connectivity, or a USB wireless dongle
    + [Wireless access point](https://www.raspberrypi.org/documentation/configuration/wireless/access-point-routed.md)

        Configuring your Pi as a wireless access point using the Raspberry Pi 4, Raspberry Pi 3, or Raspberry Pi Zero W's inbuilt wireless connectivity, or a USB wireless dongle
    + [Using a proxy](https://www.raspberrypi.org/documentation/configuration/use-a-proxy.md)

        Setting up your Pi to access the internet via a proxy server
    + [HDMI Config](https://www.raspberrypi.org/documentation/configuration/hdmi-config.md)

        Setting up your HDMI device, including custom settings
    + [Screen Configuration Editor](https://www.raspberrypi.org/documentation/configuration/arandr.md)

        Setting up your display device's resolution, frequency and orientation using the provided graphical editor
    + [Audio config](https://www.raspberrypi.org/documentation/configuration/audio-config.md)

        Switching your audio output between HDMI and the 3.5mm jack
    + [Camera config](https://www.raspberrypi.org/documentation/configuration/camera.md)

        Installing and setting up the Raspberry Pi camera board
    + [External storage config](https://www.raspberrypi.org/documentation/configuration/external-storage.md)

        Mounting and setting up external storage on a Raspberry Pi
    + [Localisation](https://www.raspberrypi.org/documentation/configuration/localisation.md)

        Setting up your Pi to work in your local language and time zone
    + [Default pin configuration](https://www.raspberrypi.org/documentation/configuration/pin-configuration.md)

        Changing the default pin states.
    + [Device Trees config](https://www.raspberrypi.org/documentation/configuration/device-tree.md)

        Device Trees, overlays, and parameters
    + [Kernel command line](https://www.raspberrypi.org/documentation/configuration/cmdline-txt.md)

        Setting options in the kernel command line
    + [UART configuration](https://www.raspberrypi.org/documentation/configuration/uart.md)

        Setting up the on-board UARTs
    + [Firmware warning icons](https://www.raspberrypi.org/documentation/configuration/warning-icons.md)

        Description of warning icons displayed if the firmware detects issues
    + [LED warning flash codes](https://www.raspberrypi.org/documentation/configuration/led_blink_warnings.md)

        Description of LED warning flashes that are shown if a Pi fails to boot or has to shut down
    + [Securing your Raspberry Pi](https://www.raspberrypi.org/documentation/configuration/security.md)

        Some basic advice for making your Raspberry Pi more secure
    + [Screensaver](https://www.raspberrypi.org/documentation/configuration/screensaver.md)

        Configuring the screen saver and screen blanking
    + [The boot folder](https://www.raspberrypi.org/documentation/configuration/boot_folder.md)

        What it's for and what's in it
    + [Network File System (NFS)](https://www.raspberrypi.org/documentation/configuration/nfs.md)

        How to set up a NFS and connect clients to it

2. [Raspberry-Pi-config](https://yaoqs.github.io/Raspberry-Pi-config/)
3. [raspberrypi issues in stackexchange](https://raspberrypi.stackexchange.com/)
4. 树莓派 cmdline.txt 内核参数命令
   + console：定义串行控制台，通常有两项：
     + console=serial0,115200
     + console=tty1
   + root：定义根文件系统的位置，例如root=/dev/mmcblk0p2表示多媒体卡块0分区2。
   + rootfstype：定义rootfs使用的文件系统类型，例如rootfstype=ext4
   + elevator：指定要使用的I/O调度程序。elevator=deadline意味着内核对所有I/O操作施加一个截止日期，以防止请求耗尽。
   + quiet：将默认内核日志级别设置为KERN_WARNING，这将在引导期间抑制除非常严重的日志消息之外的所有日志消息。
   + splash:告诉引导程序通过Plymouth模块使用splash screen。
   + plymouth.ignore_serial_console
   + dwc_otg.lpm_enable:在dwc_otg (On the Go)驱动程序中关闭LPM。
   + dwc_otg.speed:设置USB属性的速度。dwc_otg.speed=1将设置为USBv1.0 speed。
   + smsc95xx.turbo_mode:启用/禁用有线网络驱动程序涡轮模式。smsc95xx.turbo_mode=N关闭涡轮模式。
   + usbhid.mousepoll:指定鼠标轮询间隔。如果您遇到无线鼠标速度慢或不稳定的问题，将其设置为0可能会有所帮助:usbhid.mousepoll=0。
   + ...
  
5. [RPi cmdline.txt](https://elinux.org/RPi_cmdline.txt)

   This file is for passing arguments to the Linux kernel and it can be found in /boot/

+ Default options:
  + dwc_otg.lpm_enable=0
  + console=ttyAMA0,115200
  + kgdboc=ttyAMA0,115200
  + console=tty1
  + root=/dev/mmcblk0p2
  + rootfstype=ext4
  + elevator=deadline
  + rootwait
+ Some common options:
  + dwc_otg.speed=1

        USB v1 speed

  + smsc95xx.turbo_mode=N

        if the raspberrypi freezes after filling the log withe kevent dropped.

  + smsc95xx.macaddr=B8:27:EB:59:EB:25

        it will override the default mac adress with the specified one.

  + dwc_otg.microframe_schedule=1
  + console=ttyAMA0,115200
  + usbhid.mousepoll=8
  + ip=192.168.0.10

        static ip (as seen here)

  + more

# boot config operation/操作
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
  * /boot分区内都有哪些文件？\    
    树莓派引导文件存储在SD卡的第一个分区中，即/boot分区，该分区的文件系统是FAT，所以可以在Windows，macOS和Linux设备上读取这个分区。当树莓派通电时，它将从启动分区（/boot）中加载各种文件以启动各种处理器，然后启动Linux内核。当Linux启动后，启动分区将挂载为/boot。
  * /boot分区内的文件都有什么用？
    * bootcode.bin \
      这是引导加载程序，由SoC在引导时加载，它执行一些非常基本的设置，然后加载其中一个start*.elf文件。Raspberry Pi 4上未使用bootcode.bin，因为它已由板载EEPROM中的启动代码替换。
    * start.elf，start_x.elf，start_db.elf，start_cd.elf，start4.elf，start4x.elf，start4cd.elf，start4db.elf \
      这些是二进制Blob（固件），已加载到SoC中的VideoCore上，然后接管启动过程。 start.elf是基本固件，start_x.elf包括相机驱动程序和编解码器，start_db.elf是固件的调试版本，start_cd.elf是简化版本，不支持编解码器和3D之类的硬件模块，并且在gpu_mem=16中指定时使用config.txt。 \      
      start4.elf，start4x.elf，start4cd.elf，和start4db.elf是树莓派4的固件文件。
    * fixup* .dat \
      这些是链接器文件，与start*.elf上一节中列出的文件配对。
    * cmdline.txt \
      引导时，内核命令行会传递到内核。例如：
      ```
      pi@raspberrypi:/boot $ cat cmdline.txt 
      ip=192.168.124.10 dwc_otg.fiq_fix_enable=2 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext3 rootwait rootflags=noload net.ifnames=0
      ```
      命令行选项
      - ip: 定义静态IP
      - console：定义串行控制台，通常有两项：
        ```
        console=serial0,115200
        console=tty1
        ```
      - root：定义根文件系统的位置，例如root=/dev/mmcblk0p2表示多媒体卡块0分区2。
      - rootfstype：定义rootfs使用的文件系统类型，例如rootfstype=ext4
      - elevator：指定要使用的I/O调度程序。elevator=deadline意味着内核对所有I/O操作施加一个截止日期，以防止请求耗尽。
      - quiet：将默认内核日志级别设置为KERN_WARNING，这将在引导期间抑制除非常严重的日志消息之外的所有日志消息。
      - splash:告诉引导程序通过Plymouth模块使用splash screen。
      - plymouth.ignore_serial_console
      - dwc_otg.lpm_enable:在dwc_otg (On the Go)驱动程序中关闭LPM。
      - dwc_otg.speed:设置USB属性的速度。dwc_otg.speed=1将设置为USBv1.0 speed。
      - smsc95xx.turbo_mode:启用/禁用有线网络驱动程序涡轮模式。smsc95xx.turbo_mode=N关闭涡轮模式。
      - usbhid.mousepoll:指定鼠标轮询间隔。如果您遇到无线鼠标速度慢或不稳定的问题，将其设置为0可能会有所帮助:usbhid.mousepoll=0。
    * config.txt \
      包含许多用于设置树莓派的配置参数。例如：
      ```
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
    * issue.txt \
      一些基于文本的管家信息，其中包含分发的日期和git commit ID。例如：
      ```
      pi@raspberrypi:/boot $ cat issue.txt 
      ```
    * ssh或ssh.txt \
      如果存在此文件，则将在启动时启用SSH。内容无关紧要，可以为空。否则默认情况下禁用SSH。
    * wpa_supplicant.conf \
      这是用于配置无线网络设置的文件（如果硬件具备此功能）。编辑国家代码和网络部分以适合您的情况。
    * 设备树文件 \
      有各种设备树blob文件，其扩展名为.dtb。它们包含树莓派各种模型的硬件定义，并在启动时用于根据检测到的树莓派模型来设置内核。
    * 内核文件 \
      引导文件夹将包含用于不同树莓派型号的各种内核映像文件：      
      |文档名称 	|处理器 	|Raspberry Pi模型 	|笔记|
      | -- | -- | -- | -- |
      |kernel.img 	|BCM2835 	|Pi zero，Pi 1 | |	
      |kernel7.img 	|BCM2836，BCM2837 	|Pi 2，Pi 3 	|后来的Pi 2使用BCM2837|
      |kernel7l.img 	|BCM2711 	|Pi 4 	|大型物理地址扩展（LPAE）|
      |kernel8.img 	|BCM2837，BCM2711 	|Pi 2，Pi 3，Pi 4 	|Beta 64位内核1。带有BCM2836的早期Pi 2不支持64位。|
      
      注：lscpu该架构报告是armv7l，表示用于32位系统（即除了kernel8.img一切），以及aarch64用于64位系统。所述的armv7l情况下指的是被架构小端，而不是LPAE如由所指示l的kernel7l.img文件名。
    * 设备树覆盖 \
      在overlays子文件夹中包含设备树覆盖。这些用于配置可能连接到系统的各种硬件设备，例如Raspberry Pi Touch Display或第三方音板。使用以下条目中的条目来选择这些覆盖层config.txt。
