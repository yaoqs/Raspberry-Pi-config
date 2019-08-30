# 让小米路由器变成一个聪明的WIFI(1)：有人回家，它就发短信告诉你

路由器型号：小米路由器mini （129元在官网淘的)

步骤一：首先要开启小米路由器SSH, 获得root

​     这个方面小米是开放的，操作方法网上有，不详述了，见 [点击打开链接](http://www.miui.com/thread-1780840-1-1.html)

骤二：用SSH登录进去, 了解小米路由器是什么样

在你的电脑上输入以下命令，登录小米路由器 （我的路由器IP是192.168.31.1)

`ssh root@192.168.31.1`

然后输入步骤一获得的root的密码（如果密码输入正确后仍看到permission denied,别管它，按回车）进去后会看到

```
BusyBox v1.19.4 (2015-01-22 17:52:04 CST) built-in shell (ash)

Enter 'help' for a list of built-in commands.

------

Welcome to XiaoQiang!

------

root@XiaoQiang:~# 
```

用 df 查看一下存储情况

```
root@XiaoQiang:~# df

Filesystem           1K-blocks      Used Available Use% Mounted on

rootfs                   11008     11008         0 100% /

/dev/root                11008     11008         0 100% /

tmpfs                    62872      2580     60292   4% /tmp

tmpfs                      512         0       512   0% /dev

/dev/mtdblock7            1024       864       160  84% /data

/dev/mtdblock7            1024       864       160  84% /etc

tmpfs                    62872      2580     60292   4% /userdisk/sysapihttpd

/dev/root                 1024       864       160  84% /mnt

/dev/mtdblock7            1024       864       160  84% /mnt

/dev/sda1            976762580 297860468 678902112  30% /extdisks/sda1
```

小米路由器存储的根目录是只读的， 只有 /etc   /data  可写，剩余空间很小(上表显示只有160K)，只能写小程序。 

如果要大空间，可以插个U盘或移动硬盘。像上表显示的这个盘：  /dev/sda1，这是我的移动硬盘

放在 tmpfs 上的所有目录（如： /tmp    /userdisk/sysapihttpd) 都是临时存储，如果把文件写在上面，重启后将全部丢失


查看一下操作系统情况

```
root@XiaoQiang:~# cat /etc/*release

DISTRIB_ID="OpenWrt"

DISTRIB_RELEASE="Attitude Adjustment"

DISTRIB_REVISION="r40348"

DISTRIB_CODENAME="attitude_adjustment"

DISTRIB_TARGET="ramips/mt7620a"

DISTRIB_DESCRIPTION="OpenWrt Attitude Adjustment 12.09.1"
```

操作系统是OpenWrt 12.09.1,   芯片是mt7620a

OpenWrt是为小型设备用的linux


建议你先到各个目录看一下，熟悉熟悉


看一下有哪些命令、程序可以用

用 help 查看一下内置命令

`root@XiaoQiang:~# help`

help将返回一个可用命令列表


查看一下, 有哪些程序可用

```
root@XiaoQiang:~# echo $PATH

/bin:/sbin:/usr/bin:/usr/sbin
```

常用程序大多数都在这几个  bin 目录中， 常用的linux程序大体上都有（如： grep awk wget find 等)


步骤三：写一个Shell脚本，检测某个手机是否连接进了这个路由器

小米路由器有两个WIFI SSID, 一个频率是2.4G Hz的， 一个频率是5G Hz的。

思路是这样的：

1, MAC码是识别手机的标志

2, 手机连进某个WIFI SSID，则WIFI将知道手机的MAC码

3, 检查每个SSID当前的连接用户, 看有没有指定的MAC码。如有，则用 wget 访问一个外部网站URL, 触发一个网页

4, 这个网页再去触发短信通知。我的手机是189的，向我的189手机邮箱发一个邮件，利用189邮箱的短信通知功能，我的手机就会收到一条短信。


先用 ifconfig 看一下有几个网卡

```
root@XiaoQiang:~# ifconfig

br-lan    Link encap:Ethernet  HWaddr 64:09:80:18:7B:C0  

      ...

eth0      Link encap:Ethernet  HWaddr 64:09:80:18:7B:C0  

      ...

pppoe-wan Link encap:Point-to-Point Protocol  

      ...

wl0       Link encap:Ethernet  HWaddr 64:09:80:18:7B:C2  

      inet6 addr: fe80::6609:80ff:fe18:7bc2/64 Scope:Link

      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

      RX packets:1003 errors:0 dropped:0 overruns:0 frame:0

      TX packets:602 errors:0 dropped:0 overruns:0 carrier:0

      collisions:0 txqueuelen:1000 

      RX bytes:176410 (172.2 KiB)  TX bytes:103339 (100.9 KiB)

      Interrupt:13 

wl1       Link encap:Ethernet  HWaddr 64:09:80:18:7B:C1  

      inet6 addr: fe80::6609:80ff:fe18:7bc1/64 Scope:Link

      UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

      RX packets:4042 errors:0 dropped:0 overruns:0 frame:0

      TX packets:2064 errors:0 dropped:0 overruns:0 carrier:0

      collisions:0 txqueuelen:1000 

      RX bytes:869980 (849.5 KiB)  TX bytes:832223 (812.7 KiB)

      Interrupt:4 

```

wl0, wl1 分别是两张WIFI网卡，每个生成一个SSID



再用命令 iwinfo 查看一下某个网卡下有多少个连接用户、MAC码分别是什么

```
root@XiaoQiang:~# iwinfo wl1 assoclist

D0:33:AE:5F:63:AE  -50 dBm / -95 dBm (SNR 45)  0 ms ago

RX: 1.0 MBit/s                                     0 Pkts.

TX: 65.0 MBit/s                                    0 Pkts.

7C:E1:D3:EA:81:D5  -57 dBm / -95 dBm (SNR 38)  0 ms ago

RX: 130.0 MBit/s                                   0 Pkts.

TX: 130.0 MBit/s                                   0 Pkts.
```

每三行是一个连接用户的信息：   D0:33:AE:5F:63:AE  这个是MAC码，-50 dBm 这个是它的信号强度(根据这个值可以判定手机与路由器的距离及其变化)


开始编写Shell脚本，保存到 /etc 目录下，文件名为 smart_wifi

```
root@XiaoQiang:~# cd /etc

root@XiaoQiang:/etc # vi smart_wifi
```

脚本内容如下：

    #!/bin/sh
    MAC="E0:19:1D:E4:22:25"
    URL="http://192.168.31.131/miwifi/find.php?mac="
     
    #check duration, in seconds
    interval=2
     
    #To avoid notify continously, last_time is the last time we find the MAC
    last_time=$(date +%y%m%d%H%M%S)
    let last_time=last_time-interval-interval
     
    #Function: Find MAC address in associated users , query the url while matched. return 1 if found, return 0 if not found
    find_mac() {
      #Use ifconfig to find interface which name starts with wl
      ifconfig | grep wl[0-9] | awk '{print $1}' | while read WLAN
      do
         #Use iwinfo to find MAC address of connected users
         iwinfo $WLAN assoclist | grep dBm | awk '{print $1}' | while read MAC1
         do
            #if MAC1 address is the target we want
            if [ $MAC1 = $MAC ]
            then       
              #Calculate time passed since last_time
              this_time=$(date +%y%m%d%H%M%S)
              let time_passed=this_time-last_time
              let interval_min=interval+1
              if [ $time_passed -gt $interval_min ]
              then 
                  #construct the url , append the MAC address to the end
                  QUREY_URL="${URL}${MAC}"
                  #Use wget to query the url
                  wget -q -O web_response $QUREY_URL
                  echo "FIND $MAC"
                  return 1
              fi
            fi
         done
      done
      return 0
    }


​     
    #Main program: it's a dead loop,  exec find_mac() every n seconds(defined by interval)
    while [ 1 -lt 2 ]
    do
        if find_mac
        then
           #if found, update last_time
           last_time=$(date +%y%m%d%H%M%S)
        fi
        sleep $interval 
    done


MAC是要找的MAC地址(即老婆大人的手机MAC码）

URL是外部网站URL,  访问时会把实际MAC作为一个参数送给网站。

脚本中URL为LAN的，实际使用时设在公网上即可


脚本注释用了英文


程序采用了轮询制，每隔一段时间找一次。

主程序是一个死循环，每隔 interval 秒执行一次 find_mac 函数,

interval值定义了每隔多少秒找一次

find_mac() 函数是查找MAC码的函数。如果当前WIFI用户中找到MAC码,  则访问URL(该URL将发出短信)，返回值1. 如找不到，返回0  

如果一个手机持续连接WIFI，则每次查找时均可找到它，如果不断发短信是很烦人的，所以程序中采用了一个变量last_time记录上一次找到MAC码的时间，如果是连续找到，则不会触发网页。


不多解释了，自己看吧


为了让smart_wifi能运行，不要忘记赋予它运行的权限，命令如下：

```
root@XiaoQiang:/etc # chmod +x smart_wifi
```



步骤四：写一个网页（URL）, 用于接收并发送短信 (我用的是PHP)

    <?php
     
    if (isset($_GET['mac'])) {
    	mail("189XXXXXXXX@189.cn","Warning: Your wife is home.", "You know what to do");
    	echo "ok";
    } else 
    	echo "error";
     
    ?>

发短信的实现：我的手机是189的，程序中向我的189手机邮箱发了一个邮件，利用189邮箱的短信通知功能，我的手机就会收到一条短信, 内容如下：

Warning: Your wife is home.




步骤五： 运行Shell脚本，看看实际效果

再回到 小米路由器 SSH中， 运行以下命令

root@XiaoQiang:~# /etc/smart_wifi &

命令中最后这个 "&" 符是让它在后台运行

要停止脚本运行，可以用 ps | grep smart_wifi 命令查看进程，再用 kill 命令干掉它


运行效果：  OK了，老婆一出现，连接上WIFI, 则我的手机将收到一条短信

有点聪明WIFI的感觉了！



最后小结：

1， 实际上，很多路由器都是OpenWrt的，这个脚本同时可以在上面运行，不限于小米路由器。

2， 只要有想像力，这个脚本干点别的什么也是可以的，比如：你一进门，灯就自动开了。你一靠近电视机，电视机就亮了。

3， 上述示例中，如果重启路由器，脚本并不会自动启动。实际运用中，应该再写一个服务，开机自启动 (这个的做法俺卖个关子，不说了。。。)




参考：

http://wiki.openwrt.org/doc/techref/start
 ———————————————— 
版权声明：本文为CSDN博主「JoStudio」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/c80486/article/details/43503301