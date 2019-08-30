# 树莓派开发系列教程6——树莓派做wifi热点

原理：Pi使用有线连入网络，然后接USB无线网卡作为热点，提供Wifi接入。

1、USB无线网卡驱动

如果接上USB无线网卡，使用ifconfig命令，能直接看到wlan0，那么恭喜你，可以直接跳过这一步。

如果没有请查询一下树莓派支持的USB无线网卡型号，可参考以下网址：

http://elinux.org/RPi_VerifiedPeripherals#USB_Wi-Fi_Adapters


2、修改wlan0为静态IP

相当于设置路由器lan口IP,即我们访问路由器通常使用的:192.168.1.1
sudo vim /etc/network/interfaces
把原来关于wlan0的注释掉：（可能跟这个不一样，跟wlan0有关的注释掉即可）

    #auto wlan0
    #iface wlan0 inet dhcp
    #wpa-ssid "360WiFi-li"
    #wpa-psk "xiaolizi"

添加下面的：

    iface wlan0 inet static
    address 192.168.0.1
    netmask 255.255.255.0
    
    gateway 192.168.0.1
    
    完成之后需要重启

3、安装hostapd

官方的hostapd不支持8188CUS，后面需要重新卸载安装新的

笔者测试貌似这里必须先装旧的，然后后面卸了装新的，否则也不能用

sudo apt-get install hostapd

1）编辑hostapd 默认配置文件：

    sudo vim /etc/default/hostapd
    
    找到#DAEMON_CONF= ""，修改为：
    
    DAEMON_CONF="/etc/hostapd/hostapd.conf"

2）然后编辑：sudo vim /etc/hostapd/hostapd.conf

增加以下代码：

    # Basic configuration
     
    interface=wlan0
    ssid=raspberrywifi
    channel=1
    #bridge=br0
     
    # WPA and WPA2 configuration
     
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=3
    wpa_passphrase=12345678
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP
     
    # Hardware configuration
     
    driver=rtl871xdrv
    ieee80211n=1
    hw_mode=g
    device_name=RTL8192CU
    manufacturer=Realtek
    
    修改wifi名和密码
    
    ssid=raspberrywifi
    
    wpa_passphrase=12345678

3）保存退出，然后重启服务：

    sudo service hostapd restart

或者执行以下命令生效

    sudo hostapd -dd /etc/hostapd/hostapd.conf

4）如果你使用的网卡提示一下信息
Configuration file: /etc/hostapd/hostapd.conf
nl80211: 'nl80211' generic netlink not found
Failed to initialize driver 'nl80211'
rmdir[ctrl_interface]: No such file or directory

那么，还是要使用第三方的hostapd。


4、安装新的hostapd
1)删除原来的hostapd（笔者测试，不卸载貌似也行）

    sudo apt-get autoremove hostapd

2)下载第三方驱动并安装

    wget https://github.com/jenssegers/RTL8188-hostapd/archive/v1.1.tar.gz
    
    tar -zxvf v1.1.tar.gz

3)编译：

    cd RTL8188-hostapd-1.1/hostapd
    sudo make
    sudo make install

4)然后再重启服务，应该提示成功：

    $ sudo service hostapd restart
    
    [ ok ] Stopping advanced IEEE 802.11 management: hostapd.
    
    [ ok ] Starting advanced IEEE 802.11 management: hostapd.

5)将hostapd加入开机自启动

    sudo service hostapd start
    
    sudo update-rc.d hostapd enable

笔者这里提示的还是失败，但是重启后网络确实建立成功，用手机可以搜到这个网络


5、安装DHCP服务

以上步骤建立起了wifi热点，但是无法自动获取ip，需要以下步骤

sudo apt-get install udhcpd
1)编辑配置文件：
sudo vim /etc/udhcpd.conf //修改以下信息，start和end是重点，注意跟第一步的静态ip在一个网段

    start 192.168.0.20
    end 192.168.0.200
    interface wlan0

2)接下来编辑/etc/default/udhcpd并且将下面这行注释掉，以使DHCP Server正常工作：

    #DHCPD_ENABLED="no"

3)启动dhcp服务器

    sudo service udhcpd start
    
    sudo update-rc.d udhcpd enable

经过此步手机已经可以接入wifi网络，并且自动获取ip


6、配置路由转发

理论上是经过这一步，手机可以通过共享树莓派的无线网络上网了，但是笔者一直没有成功
1)设置路由映射规则

    sudo iptables -F
    sudo iptables -X
    
    sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
    sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
    
    sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
    sudo bash -c iptables-save > /etc/iptables.up.rules

2)编辑：sudo vim /etc/network/if-pre-up.d/iptables
添加下面两行代码：

    #!/bin/bash
    /sbin/iptables-restore < /etc/iptables.up.rules

保存退出，然后修改iptables权限：

    sudo chmod 755 /etc/network/if-pre-up.d/iptables

4)开起内核转发：

    sudo vim /etc/sysctl.conf

  找到下面两行：

    #Uncomment the next line to enable packet forwarding for IPv4
    #net.ipv4.ip_forward=1
    把net.ipv4.ip_forward 前面的#去掉，保存退出。

  然后:sudo sysctl -p

7、其它问题
最近经常发现无线网卡配置的DHCP不能发挥作用，经过排查发现给无线网卡指定的静态IP失败了，也就是说无线网卡没有IP导致DHCP无法工作，将/etc/default/ifplugd的内容修改配置如下：

    INTERFACES="eth0"
    
    HOTPLUG_INTERFACES="eth0"
    
    ARGS="-q -f -u0 -d10 -w -I"
    
    SUSPEND_ACTION="stop"
 ———————————————— 
版权声明：本文为CSDN博主「老徐拉灯」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xdw1985829/article/details/38845533