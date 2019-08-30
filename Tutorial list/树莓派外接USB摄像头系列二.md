# 树莓派外接USB摄像头系列二

  实现网络监控功能。（转自[麦知](http://mazclub.com/zh-cn/weblog/2015/10/28/Shu-Mei-Pai-Wai-Jie-2/#jtss-qzone)）

 

 树莓派的USB摄像头，我们可以用了，那么如何实现实时的网络监控功能呢？

 

 这里面需要认识一个软件：motion

 打开终端输入：

```
sudo apt-get updatesudo apt-get install motion
```



 然后：

 配置 motion.conf   这个文件。

 打开终端输入：

```
cd /etc/motion/
```

 会找到motion.conf 这个文件，然后用 vi 或者 nano 打开文件，

 这样修改：

 ”control_localhost on “    -------》   control_localhost off

 ”webcam_localhost on“  --------》  webcam_localhost off

 找到：



 并改变它。

 注意在改文件之前，我建议先备份文件，这样个好习惯，我想对我们是有帮助的。

 然后。在终端里面输入:(这样是开启设备)

```
sudo motion -n
```

 在终端里面会看到：



 然后，在同一路由器下的电脑中打开树莓派的地址：

 由于我的树莓派的地址是：192.168.1.103.所以我会在IE或是其它的浏览器中输入：192.168.1.103:8081