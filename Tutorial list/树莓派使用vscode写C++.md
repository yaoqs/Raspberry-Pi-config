# 树莓派使用vscode写C++

vscode用起来挺不错，于是最近决定树莓派也装一个，用起来还挺好，毕竟vscode挺轻量

首先下载安装，直接下载安装网上的好心人编译好（vscode是开源的）的版本：（原文请见下方注释）1
下载地址：https://pan.baidu.com/s/19oD7pewFQA93EebyF20w5g

安装好后安装C/C++ 插件和Code Runner插件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125172634212.png)


打开后搜索C++和Code Runner
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125172916622.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjQ2OTcy,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173001495.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjQ2OTcy,size_16,color_FFFFFF,t_70)
树莓派官方系统自带GCC，所以不必担心
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173140591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjQ2OTcy,size_16,color_FFFFFF,t_70)
安装好这两插件后为提升体验，个人觉得再进行以下设置可能更好一点点

①、设置Run In Terminal
翻到这里：Run Code configuration
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173342945.png)
设置为true
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173505213.png)
②、运行前保存
同样设为true
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173703971.png)
其它设置可以自己研究研究，写得很直白了
设置好后右边应该如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190125173902177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjQ2OTcy,size_16,color_FFFFFF,t_70)
然后我们来试一下【点右上角的小三角（也就是我们的 Code Runner 插件）】
：）

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019012517425596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjQ2OTcy,size_16,color_FFFFFF,t_70)
不过代码提示不够犀利，应该要设置一下，我没弄，麻烦，哈哈

另外要说明的是：vscode本身只是editor，因而最原本的标准做法是用vscode写完后，在命令行中编译运行，Code Runner只是帮助我们完成这个工作而已，因为我们可以在Terminal中看到：



    pi@raspberrypi:~/Desktop $ cd "/home/pi/Desktop/" && g++ Untitled-1
    .cpp -o Untitled-1 && "/home/pi/Desktop/"Untitled-1
    q w e r t y 

# 

    # cd "/home/pi/Desktop/"
    
    # 进入文件所在目录
    
    # g++ Untitled-1.cpp -o Untitled-1 && "/home/pi/Desktop/"Untitled-1
    
    # 编译运行

python、java等同理，安装相应插件，还是挺方便的


PS.Ctrl+K，Ctrl+T，换主题 ^ - ^
在这里插入图片描述

    该下载地址源自https://www.cnblogs.com/GeGeBoom/p/8627438.html
————————————————
版权声明：本文为CSDN博主「systempause」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_41246972/article/details/86650011

##   [raspberry pi（树莓派）上安装Visual Studio Code IDE](https://www.cnblogs.com/GeGeBoom/p/8627438.html)  		

　　Visual Studio Code微软公司推出的一款轻量级的Visual  Studio风格的跨平台的IDE。当然，除了Windows，OSX，还能在树莓派上使用。目前树莓派上可用的IDE真不多，VScode是比较好用的了，其它像Eclipse，SublimeTXT，ATOM之类的都没法在树莓派上使用。

　　VScode应该是目前为止最适合开发angular的编辑器，安装上相关的插件后会有代码提示，代码检查，代码自动完成，自动模块导入，自动更正等。可以大大提高开发效率。

　　安装包链接：

```
https://pan.baidu.com/s/19oD7pewFQA93EebyF20w5g
```

　　下载后直接安装，免编译

![img](https://images2018.cnblogs.com/blog/1358380/201803/1358380-20180322223208211-1862021705.png)

 

最后分享几个相关话题的链接：

1、源包下载地址：

```
https://packagecloud.io/headmelted/codebuilds
```

2、国外大佬手动编译教程：

```
http://www.hanselman.com/blog/BuildingVisualStudioCodeOnARaspberryPi3.aspx
```

 

- [使用Visual Studio代码（通过文件共享）与Raspberry Pi（Raspbian）](http://thisdavej.com/using-visual-studio-code-with-a-raspberry-pi-raspbian/)
- [在Raspberry Pi上安装Node.js的新手指南](http://thisdavej.com/beginners-guide-to-installing-node-js-on-a-raspberry-pi/)
- [使用Visual Studio代码（通过文件共享）与Raspberry Pi（Raspbian）](http://thisdavej.com/using-visual-studio-code-with-a-raspberry-pi-raspbian/)