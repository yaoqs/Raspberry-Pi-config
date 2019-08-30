# 树莓派开发系列教程9——树莓派GPIO控制

一、常用开源工程简介

    树莓派内核中已经编译自带了gpio的驱动，我们常通过一些第三方写好的库函数来完成具体的操作，比较常见的操作库函数有：

1、python GPIO

    【开发语言】——python
    
    【简单介绍】——树莓派官方资料中推荐且容易上手。python GPIO是一个小型的python库，可以帮助用户完成raspberry相关IO口操作，但是python GPIO库还没有支持SPI、I2C或者1-wire等总线接口。
    
    【官方网站】—— https://code.google.com/p/raspberry-gpio-python/


2、wiringPi

    【开发语言】——C语言
    
    【简单介绍】——wiringPi适合那些具有C语言基础，在接触树莓派之前已经接触过单片机或者嵌入式开发的人群。wiringPi的API函数和arduino非常相似，这也使得它广受欢迎。作者给出了大量的说明和示例代码，这些示例代码也包括UART设备，I2C设备和SPI设备等。
    
    【官方网站】—— http://wiringpi.com/


3、BCM2835 C Library

    【开发语言】——C语言
    
    【简单介绍】BCM2835 C Library可以理解为使用C语言实现的相关底层驱动，BCM2835 C Library的驱动库包括GPIO、SPI和UART等，可以通过学习BCM2835 C Library熟悉BCM2835相关的寄存器操作。如果有机会开发树莓派上的linux驱动，或自主开发python或PHP扩展驱动，可以从BCM2835 C Library找到不少的“灵感”。
    
    【官方网站】—— http://www.airspayce.com/mikem/bcm2835/


二、树莓派GPIO编号方式

1、功能物理引脚：

从左到右，从上到下：左边基数，右边偶数：1-40

2、BCM:
编号侧重CPU寄存器，根据BCM2835的GPIO寄存器编号。

3、wpi：

  编号侧重实现逻辑，把扩展GPIO端口从0开始编号，这种编号方便编程。正如图3 WiringPi一栏。

![img](https://img-blog.csdn.net/20140926150759750?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGR3MTk4NTgyOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

三、python GPIO

1、先安装python-dev，输入以下指令。

  `sudo apt-get install python-dev`


2、安装RPi.GPIO，依次输入以下指令。

1)下载：$ wget http://raspberry-gpio-python.googlecode.com/files/RPi.GPIO-0.5.3a.tar.gz

2)解压缩：$ tar xvzf RPi.GPIO-0.5.3a.tar.gz
3)进入解压之后的目录 ：$ cd RPi.GPIO-0.5.3a
4)启动安装 ：$ sudo python setup.py install

3、例子：

       # -*- coding: utf-8 -*-  
        import RPi.GPIO as GPIO  
        import time  
        # BOARD编号方式，基于插座引脚编号  
        GPIO.setmode(GPIO.BOARD)  
        # 输出模式  
        GPIO.setup(11, GPIO.OUT)  
          
        while True:  
            GPIO.output(11, GPIO.HIGH)  
            time.sleep(1)  
            GPIO.output(11, GPIO.LOW)  
            time.sleep(1) 

4、执行：
`sudo python led.py`

5、说明：
1)GPIO.setmode(GPIO.BOARD)，采用插座引脚编号方式。
2)由于采用插座引脚编号方式，此处的11脚相当于BCM2835寄存器编号方式的引脚11。

四、python GPIO
1、说明：
    WiringPi是应用于树莓派平台的GPIO控制库函数，WiringPi遵守GUN Lv3。wiringPi使用C或者C++开发并且可以被其他语言包转，例如python、ruby或者PHP等。
    wiringPi包括一套gpio控制命令，使用gpio命令可以控制树莓派GPIO管脚。用户可以利用gpio命令通过shell脚本控制或查询GPIO管脚。wiringPi是可以扩展的，可以利用wiringPi的内部模块扩展模拟量输入芯片，可以使用MCP23x17/MCP23x08（I2C 或者SPI）扩展GPIO接口。另外可通过树莓派上的串口和Atmega（例如arduino等）扩展更多的GPIO功能。另外，用户可以自己编写扩展模块并把自定义的扩展模块集成到wiringPi中。WiringPi支持模拟量的读取和设置功能，不过在树莓派上并没有模拟量设备。但是使用WiringPi中的软件模块却可以轻松地应用AD或DA芯片。

2.wiringPi安装

1)方案A——使用GIT工具
通过GIT获得wiringPi的源代码

```
git clone git://git.drogon.net/wiringPi
cd wiringPi
./build
```

build脚本会帮助你编译和安装wiringPi

build脚本会帮助你编译和安装wiringPi

2)方案B——直接下载
我们可以在https://git.drogon.net/?p=wiringPi;a=summary网站上直接下载最新版本编译使用

```
tar xfz wiringPi-xx.tar.gz
cd wiringPi-xx
./build
```

3、测试：
wiringPi包括一套gpio命令，使用gpio命令可以控制树莓派上的各种接口，通过以下指令可以测试wiringPi是否安装成功。

```
$gpio -v
$gpio readall
```

即可出现上面的io图

即可出现上面的io图

4、例子：

    #include <wiringPi.h>  
    int main(void)  
    {  
      wiringPiSetup() ;  
      pinMode (0, OUTPUT) ;  
      for(;;)   
      {  
        digitalWrite(0, HIGH) ; delay (500) ;  
        digitalWrite(0,  LOW) ; delay (500) ;  
      }  
    } 

5、编译运行：
在树莓派上:

```
gcc -Wall -o test test.c -lwiringPi
sudo ./test
```

在虚拟机中：

```
am-linux-gcc -Wall -o test test.c -lwiringPi
sudo ./test
```

6、注意事项：
1）IO的编号方式略有不同，采用wiring编码方式。
2）-lwiringPi表示动态加载wiringPi共享库。

五、BCM2835 C Library

1、下载:               $ wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.35.tar.gz
2、解压缩:             $tar xvzf bcm2835-1.35.tar.gz
3、进入压缩之后的目录:$cd bcm2835-1.35
4、 配置:              $./configure
5、从源代码生成安装包:$make
6、执行检查:           $sudo make check
7、安装 bcm2835库:    $sudo make install

8、例子

    #include <bcm2835.h>  
      
    // P1插座第11脚  
    #define PIN RPI_GPIO_P1_11  
      
    int main(int argc, char **argv)  
    {  
      if (!bcm2835_init())  
      return 1;  
      
      // 输出方式  
      bcm2835_gpio_fsel(PIN, BCM2835_GPIO_FSEL_OUTP);  
      
      while (1)  
      {  
        bcm2835_gpio_write(PIN, HIGH);  
        bcm2835_delay(100);  
          
        bcm2835_gpio_write(PIN, LOW);  
        bcm2835_delay(100);  
      }  
      bcm2835_close();  
      return 0;  
    } 


9、注意事项：
1）IO的编号方式略有不同，采用wiring编码方式。
2）-lwiringPi表示动态加载wiringPi共享库。

六、文章参考以下链接
http://elinux.org/RPi_Low-level_peripherals
http://blog.csdn.net/xukai871105/article/details/23115627
————————————————
版权声明：本文为CSDN博主「老徐拉灯」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xdw1985829/article/details/39580401