# 折腾树莓派（二）使用Samba进行局域网共享

安装Samba



    sudo apt-get samba samba-common-bin

配置Samba

修改配置文件smb.conf



    sudo vim /etc/samba/smb.conf

在文件最下面加上以下内容，这里设置/home/pi/Public为共享文件夹



    [Public]
       comment = Public Storage  # 共享文件夹说明
       path = /home/pi/Public # 共享文件夹目录
       read only = no # 不只读
       create mask = 0777 # 创建文件的权限
       directory mask = 0777 # 创建文件夹的权限
       guest ok = yes # guest访问，无需密码
       browseable = yes # 可见

重启Samba服务



    sudo samba restart

设置文件夹权限

在Samba配置文件设置过权限后，还需要在系统中将共享文件夹的权限设置为同配置文件中相同的权限，这样才能确保其他用户正常访问及修改文件夹内容



    sudo chmod -R 777 /home/pi/Public/

配置完成后即可从局域网内其他电脑访问共享文件夹，Windows下访问目录为\\IP\Folder，例如：

\\192.168.1.55\Public
 ———————————————— 
版权声明：本文为CSDN博主「Meow323」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Meow323/article/details/52408948