# [树莓派安装DLNA实现流媒体服务器](http://shumeipai.nxez.com/2015/07/12/raspberry-pi-install-dlna-streaming-media-server.html?variant=zh-cn)

 

![20150712113429145-0](http://shumeipai.nxez.com/wp-content/uploads/2015/07/20150712113429145-0.jpg)

平板电视大都支持DLNA(Digital Living Network Alliance)，一些智能电视可能无法支持直接播放Samba上的媒体，这时在树莓派安装一个MiniDLNA就可以让平板电视直接播放树莓派上的影音资源了。另外树莓派实验室之前有介绍过[在树莓派上安装Samba的方法](http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html)，将DLNA和Samba结合起来用更是方便又强大。下面介绍DLNA安装的具体步骤。

**安装 minidlna**
 更新一下安装源

```
`sudo` `apt-get update`
```

安装 minidlna

```
`sudo` `apt-get ``install` `minidlna`
```

**设置配置文件**
 设置/etc/minidlna.conf文件，在文件尾部添加如下内容：

```
`#A表示这个目录是存放音乐的，当minidlna读到配置文件时，它会自动加载这个目录下的音乐文件``media_dir=A,/samba/DLNA/Music``media_dir=P,/samba/DLNA/Picture``media_dir=V,/samba/DLNA/Video``#配置minidlna的数库数据的存放目录``db_dir=/samba/DLNA/db``#配置日志目录``log_dir=/samba/DLNA/log`
```

以上配置中，/samba/DLNA/* 这个目录可以自定义，一定要确保目录存在且设置权限为可读写。例子中的是已经配置好的 Samba 所在的目录，这样可以把 DLNA 目录共享在局域网之中，更方便的管理媒体文件。树莓派实验室之前有介绍过[在树莓派上安装Samba的方法](http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html)。

**重启 minidlna**

```
`/etc/init``.d``/minidlna` `restart`
```

测试

```
`/etc/init``.d``/minidlna` `status`
```

返回 [ok] minidlna is running 为正常。

这个时候就可以通过平板电视、电脑、手机来发现“媒体设备”播放DLNA目录下的媒体资源了。

**DLNA 支持的媒体格式：**
 Image JPEG PNG, GIF, TIFF
 Audio LPCM AAC, AC-3, ATRAC 3plus, MP3, WMA9
 AV MPEG2 MPEG-1,MPEG-4*, AVC, WMV9

**minidlna 使用技巧**
 查看资源个数
 http://树莓派的IP地址:8200/
 让 minidlna 随机启动

```
`sudo` `update-rc.d minidlna defaults`
```

启动 minidlna 服务

```
`sudo` `service minidlna start`
```

当你修改配置文件及媒体资源更新时，需要强制刷新，以便minidlna将最新的媒体文件进行索引

```
`sudo` `service minidlna force-reload`
```

取消 minidlna 的开机自动启动

```
`sudo` `update-rc.d -f minidlna remove`
```

停止 minidlna 服务

```
`sudo` `service minidlna stop`
```

停止 minidlna 所有进程

```
`sudo` `killall minidlna`
```

卸载 minidlna

```
`sudo` `apt-get remove --purge minidlna`
```

**文章标题：**[树莓派安装DLNA实现流媒体服务器](http://shumeipai.nxez.com/2015/07/12/raspberry-pi-install-dlna-streaming-media-server.html?variant=zh-cn) - [树莓派实验室](http://shumeipai.nxez.com/?variant=zh-cn)

**固定链接：**http://shumeipai.nxez.com/2015/07/12/raspberry-pi-install-dlna-streaming-media-server.html?variant=zh-cn