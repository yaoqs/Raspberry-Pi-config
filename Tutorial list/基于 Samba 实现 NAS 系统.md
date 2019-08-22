# [基于 Samba 实现 NAS 系统](http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html)

摆弄了几天Raspberry Pi，在搞定了无线网络、FTP服务之后，打算更进一步，通过Samba实现NAS系统与PC共享文件。
 需要安装的软件：
 sudo apt-get install samba samba-common-bin
 sudo apt-get install netatalk （可选，用于支持AFP）
 sudo apt-get install avahi-daemon（可选，用于支持网内的计算机自动发现）
 接下来就是配置了：
 [Samba 配置] /etc/samba/smb.conf 文件尾部增加

```
`[public]``comment = Public Storage``path = ``/home/pi``read` `only = no``#任何人都具有了访问修改的权限``#因为是公共文件夹，所以给了所有用户全部权限，可以自定义``create mask = 0777``#新创建文件的默认属性``directory mask = 0777``#新创建文件夹的默认属性``guest ok = ``yes``#默认的访问用户名为guest``browseable = ``yes`
```

有关 Samba 的详细配置可以参考[资料1](http://linux.vbird.org/linux_server/0370samba.php)、[资料2](http://blog.csdn.net/liuben/article/details/5077935)。然后就可以 smbd restart 了。这时候已经可以通过网上邻居看到共享文件目录，只是进不去。提示需要输入口令，尝试用本地帐户也无法进入。查了资料原来因为 Samba 使用了自己一套用户帐号资料库。要登录的话还需要向该库添加帐号信息，方法有两种：
 1.直接用 mksmbpasswd.sh 将系统用户转换成 Samba 用户：

```
`cat` `/etc/passwd` `| mksmbpasswd.sh >``/etc/samba/smbpasswd`
```

2.用 smbpasswd 命令直接设置，需要首先要添加系统用户然后用 smbpasswd -a 用户名 添加 Samba 用户；smbpasswd -e 用户名 激活用户。
 [Netatalk 配置] /etc/netatalk/AppleVolumes.default 方法参考[资料3](http://archboy.org/2011/08/18/netatalk-afp-linux-share-file-mac-osx-timemachine-backup-server/)。
 最后就是挂载USB移动硬盘了：

```
`mount` `/dev/sda1` `/home/shares/public/`
```

有时候卸载USB移动硬盘的时候会提示设备忙(Device is busy)，只需要加上 –l 参数就行了：

```
`umount` `-l ``/home/shares/public/`
```

[via](http://wangxianyuan.com/post/install-nas-on-raspberrypi)

**文章标题：**[基于 Samba 实现 NAS 系统](http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html) - [树莓派实验室](http://shumeipai.nxez.com)

**固定链接：**http://shumeipai.nxez.com/2013/08/24/install-nas-on-raspberrypi.html