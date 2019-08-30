# 树莓派使用MJPG-Streamer实现网络监控

**一、所需设备**
 1、树莓派(B版)
 2、实现 UVC 协议的摄像头，支持列表可查看：http://linux-uvc.berlios.de/ （本文采用淘宝购买的32元天敏N82）

**二、mjpg-streamer下载**
 官网：http://sourceforge.net/projects/mjpg-streamer/
 http://sourceforge.net/apps/mediawiki/mjpg-streamer/index.php?title=Main_Page

推荐版本：
 http://mjpg-streamer.svn.sourceforge.net/viewvc/mjpg-streamer/
 下载SVN的mjpg-streamer的172版。

**三、可能出现的安装错误**
 1、mjpg-streamer-r63.tar.gz
 RasPi B版使用此版本包，编译时会报如下错误：
 error: linux/videodev.h: No such file or directory
 这是因为内核版本太高的原因，videodev.h这个接口不支持了。

解决办法：
 安装：libv4l-dev

```
sudo apt-get -f install libv4l-dev`
```

然后还需把这装包中，所有.c和.h文件中的头替换掉：

```
#include <linux/videodev.h>
```

代码，改为下面这行

```
#include <libv4l1-videodev.h>
```

但很不幸，笔者还是没能成功通过编译，虽然不报这个错误了，但是报出其它错误。

2、报jpg不支持,需安装

```
sudo apt-get install libjpeg62-dev
```

3、i: init_VideoIn failed错误
 错误原因：市面上大部分摄像头都是支持YUV的，而不是JPEG的。mjpg-stream支持JPEG和YUV两种格式。

解决方法：
 1、打开以下文件：

```
sudo nano mjpg-streamer /plugins/input_uvc/input_uvc.c
```

2、翻到大概第三页
 将一行中的： 

```
int width = 640, height = 480, fps = -1, format = V4L2_PIX_FMT_MJPEG, i;
```

改成：

```
V4L2_PIX_FMT_YUYV
```

需要重新编译才会有效。

**四、安装**
 推荐使用第二步下载的安装包（172版）,或才用SVN进行下载。

```
svn co https://mjpg-streamer.svn.sourceforge.net /svnroot/mjpg-streamer mjpg-streamer
```

以上操作，前提是需要安装SVN，如果你没有安装，也可以通过

```
sudo apt-get install subversion
```

安装：

```
tar -xvzf mjpg-streamer.tar.gz
cd mjpg-streamer
/mjpg-streamer#以下操原因，见错误3
sudo nano plugins
/input_uvc/input_uvc.c#将V4L2_PIX_FMT_MJPEG改成V4L2_PIX_FMT_YUYV后保存退出
make ./start
.sh
```

**五、web访问**
 http://你的IP:8080/stream.html（如：http://192.168.1.106:8080/stream.html）

**文章标题：**[树莓派使用MJPG-Streamer实现网络监控](http://shumeipai.nxez.com/2013/10/04/raspberry-pi-mjpg-streamer-network-monitoring.html) - [树莓派实验室](http://shumeipai.nxez.com)

**固定链接：**http://shumeipai.nxez.com/2013/10/04/raspberry-pi-mjpg-streamer-network-monitoring.html