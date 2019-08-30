# 树莓派通过邮件上报实时IP，随时随地远程登录树莓派

 树莓派接上键鼠和显示器就是一台普通的Linux的主机了，这样没什么好玩的。我的想法是只接网线和电源，将它作为一台永久运行的个人服务器，随时随地可以远程登录它，在上面跑耗时的爬虫代码等任务。

 除了一台装好Linux系统（Raspbian）的树莓派，一根路由器给的网线，合适的放置环境，我们还需要做软件方面的配置。主要是如何从外网ssh登录到树莓派，昨天晚上写了个脚本解决了这个问题，这里分享一下。

 电信分给用户的IP是动态的，我使用的一般是2天变化一次，重启路由器的话立即改变。ssh需要的就是ip和用户名，所以只要解决了ip的问题，就可以做到随时登录了。

##  1.路由器静态分配内网IP

 路由器在内网分配的ip也是动态的，以我的为例，是tplink的路由器，内网的ip两个小时换一次，电脑重启立即改变。

 解决方案比较简单。

1.  打开路由器的管理页面（通常是192.168.1.1，不同型号也可能不同）。
2.  在DHCP服务中将地址池设置为192.168.1.100-200，这样动态分配的ip会在这个范围。
3.  设置静态分配。添加一个静态分配的规则，添加树莓派的MAC地址和静态分配的IP（要在局域网的网段，通常是192.168.1.XXX，而且不要在Step2设置的地址池范围内），我使用的是192.168.1.20.

 [![JINGTAIFENPEI](http://www.kawabangga.com/wp-content/uploads/2015/10/JINGTAIFENPEI.png)](http://www.kawabangga.com/wp-content/uploads/2015/10/JINGTAIFENPEI.png)

 如果不知道自己的MAC地址，可以在打开DHCP服务的时候查看一下。或者在树莓派中用`ifconfig`命令查看`HWaddr`。

 到此为止，任何时候只要你连接了这个路由器，就可以使用192.169.1.22登陆到树莓派系统了（通常情况下，路由器的设置都需要重启生效）。

##  2.外网IP和端口映射到内网

 如果想要从外网登陆的话，还需要做一步映射。因为路由器分出了很多ip，如果外网访问某一端口的话，到底是访问了哪一个ip呢？

 在路由器的「转发规则」页面添加一个对22端口（这个就是ssh端口）的转发，到192.168.1.20（你在上一节设置的IP）上。

 [![zhuanfaguize](http://www.kawabangga.com/wp-content/uploads/2015/10/zhuanfaguize.png)](http://www.kawabangga.com/wp-content/uploads/2015/10/zhuanfaguize.png)

 设置成功，你可以在 http://www.ip138.com/ 查看一下自己当前的ip是多少，然后尝试使用这个ip ssh登陆，正常情况下，是可以登陆的。

##  3.解决外网IP变化的方法

 外网的IP一旦改变，原来的你记住的ip就失效了，解决这个问题，我用的方案是：树莓派每一个小时检查自己的公网ip，如果改变，就向我的邮箱发送一封邮件报告自己的最新ip。

 原理是使用Linux的crontab添加定时任务，任务就是一个脚本，这个脚本来做check ip和发送邮件的功能。

 操作步骤：

1.  在树莓派上登录Root账户（sudo -s），因为这个程序要放在/root/rootcrons下而且添加为root的例行程序。

2.  执行`git  clone https://github.com/laixintao/Report-IP-hourly.git /root/rootcrons/`下载脚本代码（如果放在别的目录下，要注意调整后面的步骤相应的路径）。

3. `vi  reportip.py`

   设置邮件信息，修改代码的

   `e-mail  config`

   部分： 

   -  smtpserver：你的SMTP服务器
   -  username：你登陆SMTP的用户名
   -  password：密码
   -  sender：发送人，注意要和SMTP登录的账户对应，通常都是一个一个邮箱账户。
   -  receiver：收信人列表
   -  subject：邮件主题

    具体的修改信息，在代码中有详细的注释。

4.  执行`crontab  /root/rootcrons/rootcron`，将rootcroon的任务添加到crontab列表中。

5.  重启crontab，使配置立即生效。`/etc/init.d/cron  restart`

 这时整个定时检查ip并发送邮件的功能已经实现了，下一节是解释代码和更高级的配置，如果没有兴趣可以跳过，去读一下「特别注意」（我惨痛的教训）。

>  如何得到树莓派的内网ip？
>
>  开一个socket，然后用python直接获得此socket的名字。代码像这样：
>
> 获得内网ip
>
>  ```
> def get_lan_ip():    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)    s.connect(("1.1.1.1",80))    ipaddr=s.getsockname()[0]    s.close()    return ipaddr
>  ```

3.5添加开机启动

通过使用了一段时候之后，我发现，如果ip变化了，而树莓派启动的时间不是在整点的话，你只能等到下一个整点才能收到邮件了。所以这里我们再加一步，将脚本添加至开机启动里面。

这一步很简单，只要 vim /etc/rc.local ，然后添加下面一句话就可以了。

开机自启动

```
# report ip to e-mail  /usr/bin/python /root/rootcrons/reportip.py  
```

 

##  4.代码原理

 最新版本的代码请看这里：https://github.com/laixintao/Report-IP-hourly

 脚本会先检查网络是否连通，连通才会有后续的步骤。方法是访问百度是否正常返回结果（百度的主要功能是检查自己在不在线，不是搜索引擎）。

 如果联通，则尝试获得公网ip，使用一个lastip.txt来保存ip地址，每次检查的结果和这个ip对比，如果相同，则不发送邮件。

 那么如何获得公网ip呢？我的方法是去访问一个测试ip的网站，得到的html中用正则表达式去找ip。脚本中提供了三个网址，如果失败或者访问超时（默认20s），则尝试下一个。如果不能满足需要大家可以添加更多。

 运行结果像下面这样：

 [![reportip](http://www.kawabangga.com/wp-content/uploads/2015/10/reportip.png)](http://www.kawabangga.com/wp-content/uploads/2015/10/reportip.png)

 详细的代码和注释请见github吧。

###  关于crontab

 `crontab  filename`是将任务添加到定时计划中，如果有需要可以更改rootcron的配置来控制脚本的执行频率。rootcron中的代码像下面这样：

```

0 */1 * * * /usr/bin/python /root/rootcrons/reportip.py

```

* 第1列分钟1～59

-  第2列小时1～23（0表示子夜）
-  第3列日1～31
-  第4列月1～12
-  第5列星期0～6（0表示星期天）
-  第6列要运行的命令

 例如，数字表示第x分钟或者小时来执行，*表示每分钟/小时等执行。详细介绍见文末参考。

##  5.特别注意

1.  树莓派放置环境，注意温度不能太高。
2.  如果1 2小节设置完毕之后测试失败，试一下重启路由器，路由器的设置一般都是在重启之后生效的。不同的路由器相应地菜单和设置方法可能是不一样的，有一些甚至没有提供转发功能。
3.  不要使用常用邮箱给树莓派做发送用，这样不安全，最好申请一个邮箱专门给树莓派发送报告用。
4.  添加到crontab之前最好手动执行一下（`python  reportip.py`）来看一下代码是否能够正常执行，可以的话再添加到crontab。
5.  如果代码有授权失败异常（503），先不要怀疑是自己的配置出错了，去邮件提供商，看一下设置里面的「客户端SMTP」是否允许了（别问我怎么知道的，申请了三个邮箱才反应过来啊！）。
6.  收信箱请设置一个规则，例如标题带[RPI]的放到[RPI]的放到一个RPI文件夹，不提示。这样自己就不会被打扰，邮件也不会被扔到垃圾箱了。
7.  特别重要！crontab执行脚本的时候运行环境是和我们直接在shell执行不一样的！！！所以crontab文件中的命令全部写绝对路径，例如`/usr/bin/python  /root/rootcrons/reportip.py`这样。脚本中所有用到的配置都要写绝对路径，例如file文件存放的目录（两个小时才找到这个bug啊衰！手动执行正常，crontab就是不正常！）。

##  6.推荐使用

-  TP-link路由器：提供了映射服务，我的路由器支持花生壳动态解析，挺好用的。

-  [Secure  Shell for Chrome](https://chrome.google.com/webstore/detail/pnhechapfaindjhompbnflcldabbghjo?utm_source=chrome-ntp-launcher):配色好看，而且使用这个，无论你用Windows，mac，linux还是什么系统，只要有Chrome，就可以随时登陆ssh。

 [![SCURESHELL-FOR-CHROME](http://www.kawabangga.com/wp-content/uploads/2015/10/SCURESHELL-FOR-CHROME.png)](http://www.kawabangga.com/wp-content/uploads/2015/10/SCURESHELL-FOR-CHROME.png)

-  [JuiceSSH for android](https://play.google.com/store/apps/details?id=com.sonelli.juicessh)：GooglePlay所有的ssh  app我都用过，这个是最好的，相信我。

 [![JUICESSH](http://www.kawabangga.com/wp-content/uploads/2015/10/JUICESSH-168x300.png)](http://www.kawabangga.com/wp-content/uploads/2015/10/JUICESSH.png)

-  what for ios？:我没有iPhone，iPhone用户自求多福吧 ：）。

##  7.其他获得解决动态变化的IP的方案

 这些方法我也考虑过，有兴趣的朋友可以实现以下：

1.  发送邮件的方式改为用网页公开。写一个html网页，然后用ftp上传到自己的服务器，这样每次想要看最新的ip的话只要打开这个网页看一下就行了。缺点：总觉得树莓派不安全，放这里ftp密码有点不放心。
2.  有一种叫做动态解析的东西。缺点：花钱。

##  8.树莓派做服务器的缺点

 树莓派做到7×24运行，不知道靠不靠谱，我是个稳重的人，所以给树莓派贴了两片散热铜片还装了个风扇。只是…………这破烂玩意太吵了，这么点个风扇比机箱还吵。

 安卓使用app无法ssh登陆，估计是22端口被屏蔽了，因为我用vpn访问是正常的。

##  9.参考资料

-  [crontab的详细介绍](http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/crontab.html)