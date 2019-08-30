## 极客DIY：使用树莓派制作一套“NAS+私有云盘+下载机”

原创作者：HackLiu



**0×00 前言**

‍‍如果你家里有多台设备需要联网需要娱乐，你一定会或多或少遇到设备碎片化带来的烦恼。当然，已经有很多厂商包括新晋的小米、360在内的互联网公司做了这个事情，给你搞个家庭存储中心，基本能解决你的大部分需求。但是，本着不折腾就会死的机翼安劲儿，咱自个来造个低成本低功耗的解决方案，成本680元，耗时2个小时左右。

**0×01 NAS基础设施搭**

材料：先要有Raspberry Pi/路由器/破电脑等能装Linux的设备，适当容量的移动硬盘或SATA硬盘；
原理：在运行Linux的设备上，挂载大容量硬盘，并利用Samba搭建NAS；

![raspnas-web.jpg](https://www.freebuf.com/buf/themes/freebuf/images/grey.gif)

![raspnas-web.jpg](https://image.3001.net/images/20151228/14512646484836.jpg!small)

至此，如果你是严格按照教程部署，你的NAS应该已经开始正常运转，iOS、Android、Windows、Mac等不同系统都有连接NAS服务器的应用，可自行度之。

变身双向同步云盘

NAS主要服务于同一局域网内的设备，如果你想将你的NAS打造成可以外网访问并进行数据同步的网盘，我们只需借助FTP+动态域名解析即可实现。

**1.1 配置FTP服务**

接下来，我们使用vsftpd来搭建FTP服务（VSFTPD是一个开源的轻量级FTP服务器程序），按照以下步骤进行配置：

```bash
sudo apt-get install vsftpd //安装vsftpd服务器 (约400KB)
sudo nano /etc/vsftpd.conf //编辑vsftdp的配置文件
```

在配置文件中找到以下行，并设定为对应值：

```xml
anonymous_enable=NO //表示不允许匿名访问
local_enable=YES //设定本地用户可以访问。
write_enable=YES //设定可以进行写操作
local_umask=022 //支持断点续传
sudo service vsftpd restart //重启vsftpd服务
```

OK，此时搭载有NAS的服务器也已经具备FTP服务，现在可以使用浏览器访问试试。此时，你的FTP依然只能在内网访问，我们需要借助动态域名解析服务实现外网访问：

**1.2 外网访问设置（如果不需要外网访问，此步非必须）**

如果你还没自己的域名，可以在网上申请各类免费得二级域名或直接在花生壳、3322上进行动态域名申请及配置。如果你有自己的域名，参照以下教程使用DNSPOD服务进行设置。先在DNSPOD注册并添加域名，然后将你的域名DNS配置为DNSPOD的。之后，在linux上进行脚本设定。确保你已经安装了Python环境，然后新建Py脚本，粘贴如下代码（Via e-fly）：

```python
#!/usr/bin/env python  
 #<strong style="color:black; background-color:#99ff99">-</strong>*<strong style="color:black; background-color:#99ff99">-</strong> coding:utf<strong style="color:black; background-color:#99ff99">-</strong>8 <strong style="color:black; background-color:#99ff99">-</strong>*<strong style="color:black; background-color:#99ff99">-</strong>  
   
 import urllib2,urllib,json  
   
 class Dns:  
     #Dnspod账户  
     _dnspod_user = ’你的账户’  
     #Dnspod密码  
     _dnspod_pwd = ’你的密码’  
     #Dnspod主域名，注意：是你注册的域名  
     _domain = ’cb.e<strong style="color:black; background-color:#99ff99">-</strong>fly.org’  
     #子域名，如www，如果要使用根域名，用@  
     _sub_domain = ’@’  
   
     def getMyIp(self):  
         try:  
             u = urllib2.urlopen(‘http://members.3322.org/dyndns/getip’)  
             return u.read()  
         except HTTPError as e:  
             print e.read()  
             return None;  
   
     def api_call(self,api,data):  
         try:  
             api = ’https://dnsapi.cn/’ + api  
             data[‘login_email’] = self._dnspod_user  
             data[‘login_password’] = self._dnspod_pwd  
             data[‘format’] =‘json’  
             data[‘lang’] =  ’cn’  
             data[‘error_on_empty’] = ’no’  
   
             data = urllib.urlencode(data)  
             req = urllib2.Request(api,data,  
                 headers = {  
                     ’UserAgent’ : ’LocalDomains/1.0.0(roy@leadnt.com)’,  
                     ’Content<strong style="color:black; background-color:#99ff99">-</strong>Type’:’application/x<strong style="color:black; background-color:#99ff99">-</strong>www<strong style="color:black; background-color:#99ff99">-</strong>form<strong style="color:black; background-color:#99ff99">-</strong>urlencoded;text/html; charset=utf8’,  
                     })  
             res = urllib2.urlopen(req)  
             html = res.read()  
             results = json.loads(html)  
             return results  
         except Exception as e:  
             print e  
   
     def main(self):  
         ip = self.getMyIp()  
         dinfo = self.api_call(‘domain.info’,{‘domain’ : self._domain})  
         domainId = dinfo[‘domain’][‘id’]  
         rs = self.api_call(‘record.list’,  
             {  
                 ’domain_id’: domainId,  
                 ’offset’ :’0’,  
                 ’length’ : ’1’,  
                 ’sub_domain’ : self._sub_domain  
             })  
   
         if rs[‘info’][‘record_total’] == 0:  
             self.api_call(‘record.create’,  
                 {  
                     ’domain_id’ : domainId,  
                     ’sub_domain’ : self._sub_domain,  
                     ’record_type’ : ’A’,  
                     ’record_line’ : ’默认’,  
                     ’value’ : ip,  
                     ’ttl’ : ’3600’  
                 })  
             print ’Success.’  
         else:  
             if rs[‘records’][0][‘value’].strip() != ip.strip():  
                 self.api_call(‘record.modify’,  
                 {  
                     ’domain_id’ : domainId,  
                     ’record_id’ : rs[‘records’][0][‘id’],  
                     ’sub_domain’ : self._sub_domain,  
                     ’record_type’ : ’A’,  
                     ’record_line’ : ’默认’,  
                     ’value’ : ip  
                     })  
             else:  
                 print ’Success.’  
   
 if __name__ == ’__main__’:  
     d = Dns();  
     d.main()
```

‍‍‍‍‍将以上代码保存后，设定755权限，运行即可。如需要定时检测更新域名IP地址，可以将该脚本文件加入 /etc/crontab 进行定时执行任务。

至此，你的整个服务器已经同时具备了内网的NAS，以及内网和外网的FTP服务。接下来，开始在你的各类终端上进行双向网盘同步设置吧，本文以Android系统为例，其他系统的FTP同步教程请自行度之。

**1.3 安卓手机数据同步**

原理很简单，找一个FTP的App，指定手机某个文件夹（一般都是相册所在文件夹）自动同步到上文搭建好的NAS路径中。相比使用百度云等网盘同步，我们这样做的优势除了隐私性更强外，还能在本地WiFi网络内以极高的上行速率进行数据备份。

一处备份（从手机至NAS），多处共享（从NAS到Mac、PC、iPhone、iPad）。

本文以FtpSyncX安卓版为例进行设置：

![ftpsyncx.jpg](https://www.freebuf.com/buf/themes/freebuf/images/grey.gif)

![ftpsyncx.jpg](https://image.3001.net/images/20151228/14512648412252.jpg!small)

第一步：添加服务器，点击Add server，选择FTP格式，设置好服务器地址（建议填入以上文中的额动态域名，如不需要填入内网IP即可），填写登录帐号和密码；
第二步：设置要同步的两端文件夹目录，选择Local  Diectory设置手机上的文件夹，Remote  directory设置NAS上对应的同步文件夹，建议新建一个。接下来设置同步类型，remote<=>local双向同步、remote=>local  NAS到手机单向同步、 local=>remote 手机到NAS单向同步，根据需要进行选择。勾选上WiFi  Only，只在WiFi下同步；

设定完成后开始同步看看效果吧。此软件还可以设定同步时间间隔等高级参数。

**0×02 搭建完美下载机**

看了小米路由宣称的1T硬盘和迅雷离线下载很心动？别急，咱也能。

Linux下可以实现BT下载的应用有很多，目前支持下载格式最多和最知名的是Aria2。相比以往的开源BT程序，Aria2不仅支持BT下载，还支持Http、Ftp、磁力链接等格式下载，当然，文章最后完美还附上了迅雷离线下载+Aria2的完美方案。

**2.1 安装并配置Aria2**

在Linux终端内执行以下命令安装Aria2程序：

```bash
sudo apt-get install aria2 //安装Aria2
```

接下来配置Aria2的配置文件，每行为一个单独命令：

```bash
mkdir ~/.aria2 //新建文件夹
touch ~/.aria2/aria2.session //新建session文件
nano ~/.aria2/aria2.conf //新建配置文件
```

在aria2.conf配置文件里粘贴以下代码，别忘记将里边的路径替换为自己的：

```xml
dir=/home/cubie/Download //下载的默认目录
disable-ipv6=true
enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true
#rpc-listen-port=6800
continue=true
input-file=/home/cubie/.aria2/aria2.session //路径要绝对路径
save-session=/home/cubie/.aria2/aria2.session //保存你的下载列表
max-concurrent-downloads=3
```

保存并执行以下命令：

```bash
aria2c –conf-path=/home/cubie/.aria2/aria2.conf
```

如果没有错误，Ctrl+C终止并继续下一步，把aria2做成系统服务：

```bash
sudo nano /etc/init.d/aria2c
```

粘贴内容如下：

```bash
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
    sudo -u cubie aria2c --conf-path=/home/cubie/.aria2/aria2.conf -D 
    #sudo -u后面的是你正在使用的用户名，因为我用的cubie，别忘记改成自己的
;;
stop)
    echo -n "Shutting down aria2c "
    killall aria2c
;;
restart)
    killall aria2c
    sudo -u cubie aria2c --conf-path=/home/cubie/.aria2/aria2.conf -D
    #同上面的一样，根据自己的用户名改cubie;;
esac
exit
```

保存后退出，然后设置这个文件的权限为755：

```bash
sudo chmod 755 /etc/init.d/aria2c
```

测试Aria2服务是否可以启动：

```bash
sudo service aria2c start
```

如果只显示Starting aria2c，没有其他错误提示的话就说明成功了。然后添加aria2c服务到开机启动：

```bash
sudo update-rc.d aria2c defaults
```

由于Aria2不具备Web管理，所以需要使用第三方开发的开源程序Yaaw进行Web管理。先要确保你的Linux已经配置了Nginx或Apache等Web服务器环境，关于Nginx的配置可参考此文：http://wemaker.cc/59，此处不再累述。

进入Web服务的文件目录，以Nginx为例：

```bash
cd /srv/www
git clone http://github.com/wzhy90/yaaw
```

程序执行完成后，在浏览器中打开http://IP地址或域名地址//yaaw，即看到Aria2的Web管理界面，此时一个支持BT、FTP、HTTP、磁力等格式的下载机已在完美运转。点击右上角的设置项，在弹出的界面中找到Aria2  JSON-RPC Path，留好在下一步备用。

**2.2 配置迅雷离线下载**

这一步需要额外的程序支持，国内有高手开发出了Aria2结合迅雷离线的浏览器插件，本文以Chrome为例设置。

![xunlei-lixian.jpg](https://www.freebuf.com/buf/themes/freebuf/images/grey.gif)

![xunlei-lixian.jpg](https://image.3001.net/images/20151228/14512648721846.jpg!small)

打开此链接，安装Chrome浏览器扩展程序，然后登录迅雷离线的Web站（需要会员），点击页面右上角的设置，在弹出的窗口中，找到Aria2  JSON-RPC  Path项，将Yaaw中复制出来的URL粘贴进去并保存。接下来，试试在迅雷离线下一部片子，基本是秒下，然后点击“取回本地”旁的下拉按钮，在选项中选择Yaaw。回到你的Aria2  Web管理页，是不是已经有一项任务躺在里边了？

至此，大功告成。咱也过上了想什么时候下就什么时候下、想在哪下就再哪下、想下啥就下啥、想在哪看就再哪看、想怎么同步就怎么同步的性福生活。

推荐方案：树莓派B英国产+4G TF（255元）+ 1T硬盘 （400元） + 有源的USB Hub（25元）进行搭建，总成本680元左右。

**0×03 预告**

接下来会把树莓派替换为一个802.11ac双频可刷OpenWRT的路由器，将整体成本和能耗降到最低。敬请期待我们带来的路由器的解决方案。有了这神器，还等啥小米路由、360路由……

\***作者：HackLiu，本文属FreeBuf原创奖励计划文章，未经许可禁止转载**