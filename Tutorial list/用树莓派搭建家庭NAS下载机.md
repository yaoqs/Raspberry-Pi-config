## 用树莓派搭建家庭NAS下载机

## 开始

去年双十一的时候入了树莓派，抱着试一试的心情，我买了一堆配件准备玩，结果买回来玩了两天就扔在那里没动了。
 逼乎上有长者曾经说过：“先想好想要做什么，再去买东西做。买完东西，问做什么好的，通常都是[哔——]。”我认为，这是坠好的。
 正好前两天逛咸鱼，发现了一个同城的二手SATA 160G硬盘，成色还不错，犹豫再三，剁了手，又去淘宝买了一个硬盘盒

## 挂载磁盘

![写入速度](http://r5.loli.io/iMvuUz.png)

经过了艰苦的等待，硬盘盒终于到了，连接电脑测试，速度飞起，100M/s的写入速度让我终于感觉到USB3.0的性能，泪流满面，这大概是我用过最快的USB设备了。

但是看看树莓派的USB2.0口和百兆的网口，似乎有点浪费了，不过为了世界和平，就这样吧……

关于磁盘的挂载，是Linux的基本功。一顿猛查资料后，先找到硬盘在/dev/中的地址，我的sata硬盘是/dev/sda1/，在mnt目录下新建一个目录当作挂载点，安卓爪机里常见的那啥sdcard1之类的目录就是挂载点了，其实放啥地方起啥名都行，只要能找得到的话……

```
#新建一个目录
sudo mkdir /mnt/usbdisk
```

硬盘是NTFS格式的，其实本来想格式化成linux用的ext4，但是寻思寻思，玩意我哪天闲的没事需要高速存储数据又把硬盘拔下来插电脑呢？于是就需要ntfs-3g这个软件来让树莓派可以读取NTFS格式。

```
#安装所需软件包
sudo apt-get install fuse-utils ntfs-3g
#加载内核模块
modprobe fuse
#编辑fstab让移动硬盘开机自动挂载
sudo nano /etc/fstab
#在最后一行添加如下内容
/dev/sda1 /mnt/usbdisk ntfs-3g defaults,noexec,umask=0000 0 0
#保存重启，即可生效
```

于是现在就可以通过/mnt/usbdisk访问硬盘了，高中时候买的那两本厚厚的（盗版）鸟哥linux私房菜没白买啊，花了几周自习看完了，虽然记不住具体是啥，但是还记得系统都能干啥，还是挺有意义的。

## samba

接下来要让局域网内可以访问，鸟哥的linux私房菜中重点介绍的samba让我记得挺清楚，这玩意弄吼了，网内设备都可以访问，Exited！

```
#搞一个桑巴出来
sudo apt-get install samba samba-common-bin
#复制一下配置文件留个备份
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
#编辑配置文件
sudo nano /etc/samba/smb.conf
```

编辑配置文件的时候，先在上面去掉security = user前面的注释，来使用用户进行验证，禁止匿名登录，虽然是内网但是肯定是要用户验证的嘛。

在文件的最后按照上面的格式写下自己的配置，配置项都很简单易懂，不知道比那些一堆一堆英文的文档高到哪里去了！（大拇指

```
[name]
comment = Test share
Path = /mnt/usbdisk
valid users = @nekotora
force group = users
create mask = 0660
directory mask = 0771
read only = no
```

配置好以后重启一下samba服务

```
service samba restart
```

然后添加一下上面配置的用户

```
useradd nekotora -m -G users
```

给新用户设置密码

```
passwd nekotora
```

让samba认识一下这个用户，并且配置他的samba访问用户名密码

```
smbpasswd -a nekotora
```

完成！
 ![访问NAS](http://r5.loli.io/i26fmi.png)

在windows或者安卓设备上就能找到树莓派了，赞赞赞。
 实测内网速度还是很令人满意的，读取速度能跑满辣鸡树莓派和他连着交换机的百兆带宽。总感觉用树莓派有点浪费了速度OAQ

## Aria2

不能下载东西的硬盘和咸鱼有什么区别！
 Aria2是一个命令行下载工具，其他平台能下啥他都能下，甚至有一些小工具还能让他支持百度网盘和迅雷离线。

安装aria2 

```
sudo apt-get install aria2  
```

aria2运行的时候需要两个文件，并且需要我们手动配置，一个是配置文件aria2.conf，保存配置，另一个是aria2.session，要不每次aria2关闭的时候，之前下载的进度都没了。为了显得我的目录很整齐，我把他放到/home/pi/aria2/目录下面

```
cd /home/pi
mkdir aria2
cd aria2 
#摸一个空白的aria2s session文件出来  
touch aria2.session  
#编辑一下配置文件
nano aria2.conf  
```

内容如下

```
#文件保存目录 
dir=/mnt/usbdisk/download
#因为垃圾运营商还没有ipv6，获取了也没有，关掉
disable-ipv6=true  
#打开rpc给等会的web管理界面用
enable-rpc=true  
rpc-allow-origin-all=true  
rpc-listen-all=true  
#rpc-listen-port=6800  
#允许断点续传
continue=true  
#进度保存文件
input-file=/home/pi/aria2/aria2.session  
save-session=/home/pi/aria2/aria2.session  
#最大同时下载任务数
max-concurrent-downloads=3  
```

保存退出

用配置文件启动一下测试

```
sudo aria2c --conf-path=/etc/aria2/aria2.conf  
```

服务很愉快的跑起来了没有报错，用Ctrl+C停下来，继续配置

顺便给aria2c做成服务吧，用起来会方便的多

新建一个aria2c的服务

```
sudo nano /etc/init.d/aria2c
```

存入一下内容
 （反正也看不懂，大概就是系统服务的一种配置格式嗯……）
 记得把用户名和位置改成自己的

```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          aria2
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Aria2 Downloader
### END INIT INFO

case "$1" in
start)

echo -n "Starting aria2c"
sudo -u pi aria2c --conf-path=/home/pi/.aria2/aria2.conf -D
#把上面的两个pi换成你的用户名
;;
stop)

echo -n "Shutting down aria2c "
killall aria2c
;;
restart)

killall aria2c
sudo -u pi aria2c --conf-path=/home/pi/.aria2/aria2.conf -D
#把上面的两个pi换成你的用户名
;;
esac
exit
```

保存退出，再给个权限

```
sudo chmod 755 /etc/init.d/aria2c
```

来测试一下Are you ok？

```
sudo service aria2c start
```

显示Starting aria2c，ok，ok

让他开机自己启动

```
sudo update-rc.d aria2c defaults
```

aria2c还没有管理页面，我们可以找个好看的web页面进行管理

![webui-aria2](https://raw.githubusercontent.com/ziahamza/webui-aria2/master/screenshots/overview.png)

Github上有不少这样的界面，我用的是webui-aria2，看起来比较高档
 https://github.com/ziahamza/webui-aria2

想要能访问web界面的话还需要安个Apache，这个就轻车熟路多了。

```
sudo apt-get apache2
```

装好之后设置好apache2的权限：

```
sudo visudo
```

底部添加这行

```
www-data ALL=(ALL) NOPASSWD: ALL
```

Ctrl + O保存，Ctrl + X退出

然后从git下载web界面

```
cd /var/www
git clone https://github.com/ziahamza/webui-aria2
```

现在，打开树莓派的地址就能访问到，点击设置，服务器设置，填上地址和端口号，提示Successfully就成功了！
 http://192.168.1.10/aria2/
 添加任务就能开始下载了。

## 迅雷

然后我就开始下东西，老司机开的车就很好嘛。*9FE23BE78F054024F05B59522B75AA6423720E5A*

结果发现根本没速度啊卧槽，于是等了一个下午，才下了几Kb，我当时就不乐意了。这什么辣鸡！
 我用迅雷试着下了一下，这车没问题啊。
 于是求助，老司机凌妹看了看说：“传统bt不行的”……
 哦，原来如此，（英文怎么说来着），so_da_na！

不行，还是搞套迅雷离线下载吧。

http://luyou.xunlei.com/forum-51-1.html
 先去迅雷那里下下来编译好的迅雷下载程序。
 因为不是开源的，迅雷对各种平台都提供了编译好的版本，树莓派用到的是armel_v5te_glibc。

参考迅雷官方的配置说明
 http://g.xunlei.com/forum.php?mod=viewthread&tid=30&extra=page%3D1%26filter%3Dtypeid%26typeid%3D3

下载好之后通过sftp把文件推倒树莓派上，为了文件的整齐，我放倒了/home/pi/xunlei下面。

启动一下试试看？

```
/home/pi/xunlei/portal
```

成功的话，会有一个六位的激活码生成，打开http://yuancheng.xunlei.com/，添加设备就ok了！

但是这样管理不方便，而且想让他开机启动，还要把他做成一个服务，加到启动项里。

创建服务

```
sudo nano /etc/init.d/xunlei
```

和刚才的aria一样配置一个服务

```
#!/bin/sh
#
# Xunlei initscript
#
### BEGIN INIT INFO
# Provides:          xunlei
# Required-Start:    $network $local_fs $remote_fs
# Required-Stop::    $network $local_fs $remote_fs
# Should-Start:      $all
# Should-Stop:       $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start xunlei at boot time
# Description:       A downloader
### END INIT INFO

do_start()
{
        ./home/pi/xunlei/portal
}

do_stop()
{
        ./home/pi/xunlei/portal -s
}

case "$1" in
  start)
    do_start
    ;;
  stop)
    do_stop
    ;;
esac
```

最后设置一下开机启动

```
sudo update-rc.d xunlei defaults
```

完成，现在可以再迅雷里面看到树莓派并且方便的下载了。在外网也可以控制。

## hdparm

最后，还有一个问题没解决。
 那块硬盘，有没有事干的时候都在不停的工作。因为是旧硬盘，在磁盘参数里已经看到两个报警了，连续工作时间也达到了500days+
 这么高寿的硬盘24小时工作总感觉有点对不起它。
 这就需要一个硬盘自动休眠的东西。

安装haparm

```
sudo apt-get hdparm
```

设置硬盘自动休眠

```
#立刻让硬盘休眠，先试试看好不好用
sudo hdparm -Y /dev/sda1
#设置硬盘自动休眠，数值/12 = 分钟，设置为120就是无操作10分钟后休眠
sudo hdparm -S 120 /dev/sda1
```

这样硬盘没事的时候就能睡觉了。
 当然，还要加到启动项里好让硬盘自动休眠

编辑rc.local

```
sudo nano /etc/rc.local
```

在exit 0之前插入休眠硬盘的命令就ok

## 总结

![NAS](http://r6.loli.io/QFnuIn.jpg)

于是，这样就折腾好了，在路由器设置中让树莓派暴露给外网，在其他地方也能控制他下载东西了。
 因为树莓派和交换机都是百兆网口，能满速已经不错了，还是浪费了好多性能，还不如买个小米路由器还送个1t硬盘，但是过程还是挺有趣的，特别是看到桌子上的一堆设备开始闪着led运行的时候简直炒鸡帅(๑•ั็ω•็ั๑)
 最近一直在填坑，偷偷摸鱼玩了这些不知道会不会被打。
 说起来咱干活真是不专注，本来想着写完赶紧继续填坑，结果想引用几句[比特客栈](http://bitinn.net/)一篇文章的话，就跑去那里一边翻一边读以前的文章，结果一看就是两个小时，最后还是没找到……又浪费了一个下午OAQ

还有多少时间可以发呆呢？



------

相关链接：
 1 - 给树莓派挂载移动硬盘或U盘
 http://shumeipai.nxez.com/2013/09/08/raspberry-pi-to-mount-the-removable-hard-disk.html
 2 - 用树莓派打造一个NAS
 http://www.leiphone.com/news/201406/respberry-nas.html
 3 - 树莓派做下载机，Aria2！
 http://shumeipai.nxez.com/2014/07/01/raspberry-pi-do-download-machine-aria2.html
 4 - Linux 指令篇：磁盘维护--hdparm
 http://www.jb51.net/linux/hdparm.htm
 5 - 树莓派上使用迅雷远程下载
 http://shumeipai.nxez.com/2014/06/25/raspberries-come-remotely-download-thunder.html?variant=zh-cn