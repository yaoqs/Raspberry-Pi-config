# 树莓派跑一个简单c++小程序教程

我用的是树莓派3代b型，所使用的是Debian系统的衍生系统raspbian(对系统不太了解不清楚)。树莓派开发c++程序需要的工具有编辑器vim,调试器gdb,编译器gcc或者g++.（大神飘过就行~~ 记录一下）用红笔涂得地方是我命令敲错的地方，大家忽略就行。

安装vim   执行命令 sudo apt-get install vim（因为我之前安装过了所以显示already the newest version）

![img](https://img-blog.csdn.net/20180209095951681?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


安装调试器gdb  执行命令sudo apt-get install vim（因为我之前安装过了所以显示already the newest version）

![img](https://img-blog.csdn.net/20180209100249814?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

使用的编译器是树莓派系统自带的gcc或g++编译器。

这样就可以开始调试c++程序了

新建一个文件夹 命令 mkdir raspberry  进入名称为raspberry文件夹  命令cd raspberry , 这样我们就可以来写一个c或c++程序在raspberry文件夹下树莓派上跑起来。

![img](https://img-blog.csdn.net/20180209101025140?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

新建一个test.c文件 ，命令 vim test.c 回车 进入如下界面。就可以在这里写代码了。（直接往里输入可能会有些问题），先按下键盘上的i键，就可以正常输入了。

![img](https://img-blog.csdn.net/20180209101758585?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![img](https://img-blog.csdn.net/20180209104848840?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

保存文件 按下esc  然后输入：wq! 

![img](https://img-blog.csdn.net/20180209104319852?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

回车 

![img](https://img-blog.csdn.net/20180209103732754?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


这事生成了一个test.c文件在raspberry文件夹下。

开始编译  输入命令 gcc test.c -o test1 

![img](https://img-blog.csdn.net/20180209104457133?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

执行我们生成的文件 执行命令./test1

![img](https://img-blog.csdn.net/20180209105015711?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VudGluZ3NoZW5nMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

至此 一个简单的小程序跑完了。
 ———————————————— 
版权声明：本文为CSDN博主「tingsheng123」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/suntingsheng123/article/details/79295334