# [树莓派 Raspberry Pi 使用简介](http://www.waveshare.net/txt/RPi-B-Plus-UM.htm)

  你需要以下这些东西：
   一张Micro SD记忆卡
   一个SD读卡器，用于将系统映像写入到Micro SD卡中供电来源。我们用的是一个旧的安卓手机充电器，你需要一个5V的micro USB接口充电器为它供电。
   如果你是用的普通显示器而不是高清电视，你需要一条HDMI连接线与HDMI-DVI转换器。如果你的显示器支持HDMI或者你打算使用电视机进行连接，那么你就可以省去转换器了。
   USB接口的键盘和鼠标
   一条以太网线

​      可选设备：
   用于保护Raspberry Pi的盒子
   除了键盘和鼠标之外，如果你打算要连接更多的USB设备，那么你需要一个USB Hub集线器。
   我们在本文中使用的东西：
   一个Model B Raspberry Pi
   一张16GB Class 10 SanDisk Ultra SDHC记忆卡（传输率标为300MB/s）
   一个原本用于HTC Inspire的旧充电器
   罗技K260无线键鼠套装（两件只用一个接收器，因此只占用一个USB口）
   一条RJ-45网线

​      设置
   当你把上面的一切都准备好以后，我们就可以开始设置Raspberry Pi了

​      将Raspbian映像安装到Micro SD卡中
​      Raspberry Pi出来已经有一段时间了，也正因如此，无论你是在用Windows、Mac OS  X还是Linux，都有好几种方法将Raspbian的映像写到你的Micro  SD卡中。然而为了让这篇文章不变成介绍各种映像写入软件的文章，我们将会把最常用的方法展示给大家看：在OS  X与Linux中我们将会用系统自带的dd工具写入，而在Windows中我们将会使用一个叫Win32DiskImager的工具进行。

为了让过程便得简单，我们建议你在开始操作前，先把下载文件夹清空。然后，从这里下载Raspbian.zip。在里面，选择最新的Raspbian来下载。当然你也可以选择使用BT来下载，为了能让其他人继续用BT下载，我们建议你在下载完成后继续上传。

把压缩文件解压以后，你将得到Raspbian的映像文件，这个时候打开终端窗口。如果你使用的是OS X 10.7或更高版本，你可以从应用程序中的工具(10.7)或者其他(10.8)中找到终端窗口。打开后，使用”cd ~/Downloads”来进入下载文件夹。

如果你在开始前清空了你的下载文件夹，那么现在里面应该有两个文件，输入”ls”来确认是否分别有一个img后缀与一个zip后缀的文件，如下图：

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-11.jpg)

接下来就要开始将解压出来的映像文件写到Micro SD卡中了，很可惜这个操作并不是直接把文件拷到卡中那么简单。首先，我们在把Micro SD卡插到你的Mac之前，要把你电脑分区状态先搞清楚。如果你已经迫不及待把卡插了进去，那么先把它安全移除吧。

在终端窗口中输入”df –h”，你应该会看到类似这样的结果

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-12.jpg)

现在，插入你的Micro SD卡，再运行一次刚才的命令，你会发现多了一个”/dev/disk1s1”的设备。把它先记下来，等一会儿把它转换成原始设备名称时会用到的。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-13.jpg)

接下来，你需要先从系统中把卡卸载掉，以便”dd”这个工具可以将映像写入到Micro SD卡中。在终端窗口输入”sudo diskutil unmount /dev/disk1s1”，然后输入你的系统密码。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-14.jpg)

如果操作成功的话，你会看到最后那一行字：”Volume XXX unmounted”中的XXX是你Micro SD卡的名称。现在我们就可以往卡上写入映像了。

记得刚才让你记下来的设备名么？现在就是用到它的时候了。将你的设备名（例如/dev/disk1s1）最后的s1去掉，然后在disk前面加上r，变成”/dev/rdisk1”，这样你就得到Micro SD卡的原始设备名称了。

也就是说，”/dev/disk1s1” = “/dev/rdisk1”。懂了吗？

然后在终端中输入以下命令：”sudo ddb s=1m if=2012-06-15-wheezy-raspbian.img of=/dev/YOURDISKNAME”。将YOURDISKNAME改成你的原始设备名称，通常这个都是/dev/rdisk1。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-15.jpg)

写入的过程需要一点时间。当”dd”完成了它的工作以后，将桌面的Micro SD卡图标拖进回收站中以将Micro SD卡安全移除。

准备工作就完成了！直接到“初次启动你的Pi”部分吧。

Our prep work is finished! Skip ahead to "Booting your Pi for the first time."

**如果你使用的是Linux**

无论你使用的是哪一个发行版的Linux，”dd”这个工具通常都是默认安装的。对于接下来的操作，我们假设：

1) 你的sudo程序已经设置妥当，而且你知道如何使用它

2) 你的Linux中已安装fdisk工具（大多数发行版都默认安装）

在这里我们使用的是最为流行的Linux发行版Ubuntu，其中”dd”, “fdisk”已默认安装，并且”sudo”也已预先为用户配置好了。

首先，从这里下载Raspbian的最新版本压缩包。同样，你也可以选择使用BT下载。在你下载完成以后，第一个操作自然就是解压。

打开一个终端窗口，然后使用”cd ~”命令将工作目录变更到你的主目录中。输入”unzip imagename.zip”，将imagename替换为刚才你下载文件的名字。

如果你找不到刚才下载的压缩包，你可以尝试使用”cd ~/Downloads/”进入下载目录寻找。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-16.jpg)

接下来，我们要确定你的Micro SD卡在系统中的设备名称。插入你的Micro SD卡，然后在终端中输入”sudo fdisk –l”。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-17.jpg)

**注意看列出来的设备：你运行命令后得出的结果跟上面显示的应该很相似**

这里看到，/dev/sda是一个大小250.1GB的设备，而/dev/sdb是一个15.9GB的设备。很明显，/dev/sdb就是我们的Micro  SD卡。留意一下/dev/sdb下面显示的分区：/dev/sdb1与/dev/sdb2，我们要在写入映像之前将这两个分区先从系统中卸载掉：

sudo umount /dev/sdb1

sudo umount /dev/sdb2

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-18.jpg)

接下来，我们使用”dd”将映像文件写入到Micro SD卡中：”sudo dd bs=1M if=2012-08-16-wheezy-raspbian.img of=/dev/sdb”。跟在Mac中操作相同，这个过程需要一些时间来完成。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-19.jpg)

如果你看到终端显示跟上面类似的文字，那么写入已经完成了。现在，你可以跳到“初次启动你的Pi”部分继续阅读。


    **如果你正在使用Windows**

如果你正在使用的是Windows系统，有一个叫做Win32DiskImager的小工具可以帮你完成这项工作。把它下载下来以后，再把Raspbian的映像从这里下载下来。记得选择最新版本的Raspbian。

当你把Win32DiskImager和Raspbian映像文件都下载下来以后，把他们都解压出来。你可以把所有文件都解压到桌面的一个文件夹中，方便接下来的操作。

将你的Micro SD卡插入到你的读卡器中，留意一下读卡器在电脑中的盘符。将读卡器接到电脑以后，打开刚解压出来的Win32DiskImager程序：

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-21.jpg)

在启动时，Win32DiskImager可能会提示以下错误：（至少在Windows 7中是这样的）

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-22.jpg)

这个错误可以忽略掉，点OK继续

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-23.jpg)

点击右边的蓝色文件夹图标，然后选中刚才解压出来的Raspbian映像文件。确保最右边Device中的盘符为你读卡器的盘符。

打开映像文件以后，点Write，然后点一下Yes确定操作。整个写入过程需要一些时间来完成。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-24.jpg)

写入完毕以后，你会看到下面的信息，这时你就可以把你的Micro SD卡安全移除了。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-25.jpg)

 

**初次启动你的Pi**

初次启动Raspberry Pi时，你会看到一个叫做raspi-config的配置工具。如果在日后使用过程中你需要更改这些设置，你可以通过在Pi的命令行中运行raspi-config来使用这个工具。在这里，你需要进行一些最基本的设置来继续使用你的Pi。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-31.gif)

首先，我们要选择expand_rootfs。它的作用是将刚才写入到Micro SD卡中的映像文件大小扩展到整张Micro  SD卡中。如果你使用的是一张较大的Micro  SD卡（例如16GB），那么你肯定像充分利用上面的空间。因为原本的映像只有大约2GB的大小，进行该操作就能将它扩展到与你的Micro  SD卡同样的大小。

选中expand_rootfs选项，然后按下回车。你会看到如下提示，只需要再按一下回车就可以回到raspi-config的主菜单中。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-32.gif)

接下来就是overscan选项。你会发现，屏幕显示的图像并没有完全占用你的显示器空间；所以，最后就是将overscan禁用掉，来让系统充分利用整个屏幕。但如果你的屏幕显示没有问题，那么你就可以跳过这个步骤。假设你需要禁用overscan，那么将overscan选项选中后按下回车。

接下来的画面能够让你选择禁用(Disable)或者是启用(Enable) overscan。如果你在往后使用的过程中更换显示器或者电视机，你或许需要重新启用overscan。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-33.gif)

接下来，我们要确保我们的键盘正常工作。Raspbian默认的是英国键盘布局，而我们在中国使用的键盘布局与美国的相同，因此我们要对它进行更改才能正常使用。

选中configure_keyboard，然后按下回车。下面显示的画面中你会看到一个很长的列表，里面都是不同的键盘类型。你可以根据你的需要来选择，或者直接选择Generic 105-key (Intl) PC键盘。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-34.gif)

 在选择键盘类型以后，你需要为它选择一个键盘布局。刚开始显示的列表中，都只有英国的键盘布局，但是我们现在要选择美国的键盘布局，因此我们选中其他(Other)，然后再里面的列表选择English (US)。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-35.gif)

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-36.gif)

然后你会被问到关于辅助键的问题，选择默认的选项，并且在下一个画面中选择No compose key选项。同样，如果你需要更改这个设置，可以使用raspi-config来更改。

最后一个选项为是否打开CTRL+ALT+BACKSPACE的组合键。它可以在你的图形界面崩溃时，不需重启而将图形界面进程结束掉。

回到主菜单，下一步我们要设置一个用户密码。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-37.gif)

选中change_pass然后按下回车。一个确认窗口弹出以后，在屏幕的底下会提示你输入一个新的UNIX用户密码。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-38.gif)

到这里就差不多完成了。接下来设置你的“区域”，这个主要会决定系统使用的字符集和语言。同样，如果你不想使用英国英语，那么你可以在这里更改这些设置。在演示中我们选择的是美国英语，因此我们选择en_US.UTF-8，并下翻列表将en_GB选项剔除

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-39.gif)

再下一个对话框中会让你选择默认的区域，选择你刚才选的区域然后按回车继续。

回到raspi-config的主菜单，设置适当的时区。选中change_timezone选项然后按下回车。你会先看到一个地区列表。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-40.gif)

选择一个地区，然后下一个菜单中会显示该区域的具体位置。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-41.gif)

再次回到主菜单，剩下还没选过的选项我们可以忽略，直接点击完成(Finish)。系统会提示你，一些变更需要重启才能生效。重启以后，你会看到一个登录界面，如下：

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-42.gif)

在这里，用户名为”pi”，密码就是你刚才设置的UNIX用户密码。

使用你的Raspberry Pi

现在，你已经登录到了你之前设置的Raspberry Pi中了。你要做的第一件事，就是在命令行中输入”startx”来进入图形界面，以下我们会把这个界面称之为“窗口管理器”。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-43.gif)

屏幕快速闪烁几次后，你应该会看到如下画面：（除了那个终端窗口以外）

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-44.jpg)

欢迎使用LXDE窗口管理器。LXDE是一个非常轻量级，并且功能齐全的图形界面，它能够很好地运行在Raspberry Pi中。如果你从来未使用过LXDE，别担心，它用起来跟Windows非常相似。点击最左下角的图标，你会看到一个应用程序列表。

**现在能干嘛呢？**

当你把Pi配置完成，并且打开了LXDE窗口管理器以后，你就得到了一台运行完整 Debian的Linux机器了。在这个时候，你可以使用Midori来浏览网页，架设一个网页服务器，或者是进行一些平常的工作。

但首先，我们要谈的是Raspberry Pi本身最注重的方面：教育。特别是，软件开发教育

**在Raspberry Pi****上撰写你的第一个程序**

Raspbian中预装Python，它是Raspberry Pi的官方编程语言，还有IDLE 3，一个Python的集成开发环境(IDE)。我们将会教你如何使用IDLE在Raspberry Pi上写一个简单的程序。

在学习一门新的编程语言时，通常第一件事都是写一个”Hello World!”的小程序。接下来我们要教你的跟这个也差不多，不过要比它稍微花哨一些。这是一个会问你的名字，然后作自我介绍的Python程序。

首先，直接双击LXDE桌面上的图标来打开IDLE 3

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-51.jpg)

点击文件 > 新窗口，就会出现一个可以让你输入文字的空白窗口。

输入以下内容到你的新窗口中：

\#my first Python program

username = input("Hello, I'm Raspberry Pi! What is your name? ")

print ('Nice to meet you, ' + username + ' have a nice day!')

如下图：

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-52.jpg)

现在，点击文件 > 另存为，然后将你的文件保存为”hello”，点击保存。

现在就可以运行你刚才写的程序了！

点击运行 > 运行模块，或者直接按F5键。

当IDLE 3窗口中提示你”Hello, I’m Raspberry Pi! What is your name?”的时候，输入你的名字然后按下回车。你会看到Raspberry Pi对你做出回应。

![img](http://www.waveshare.net/photo/development-board/RPi-B/RB-53.jpg)