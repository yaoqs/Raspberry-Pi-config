# 树莓派部署BT下载机

要求：做一个BT下载机，能下载BT和emule的资源，要有远程控制方式（web或专用端口）

1、安装文件

`apt-get install transmission-daemon`




2、配置

进入程序目录：

`cd /etc/transmission-daemon/`


结束相关进程



修改权限

`chmod -R 777 /etc/transmission-daemon/`




修改配置文件：

`nano setting.json`




加载配置重启

    停止服务：sudo service transmission-daemon stop 
    加载配置：sudo service  transmission-daemon reload 
    启动bt下载：sudo service transmission-daemon start



这样即可通过80端口访问 树莓派IP地址/transmission进程下载控制。
 ———————————————— 
版权声明：本文为CSDN博主「南极光」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/rk2900/article/details/8660812