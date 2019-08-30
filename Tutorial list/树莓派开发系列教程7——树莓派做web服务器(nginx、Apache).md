# 树莓派开发系列教程7——树莓派做web服务器(nginx、Apache)

一想到Linux Web服务器，我们首先想到的是：

Apache + MySql + Php．

    Apache:是世界使用排名第一的Web服务器软件。
    
    可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件.
    
    MySQL:是一个关系型数据库管理系统，由瑞典MySQL AB公司开发。是最流行的关系型数据库管理系统，在WEB应用方面MySQL是最好的RDBMS(关系数据库管理系统)应用软件之一。
    
    PHP:（外文名: Hypertext Preprocessor，中文名：“超文本预处理器”）是一种通用开源脚本语言。语法吸收了C语言、Java和Perl的特点，易于学习，使用广泛，主要适用于Web开发领域。


树莓派可以安装这个LAMP系列，但Apache 和 MySql对于树莓派这个小小的机器，太重了，主要是消耗内存多＼速度慢＼占用磁盘大(约200M吧)，所可以选择安装一个轻量级的Web服务器：

nginx + php + sqlite

    nginx:是个轻量级的Web服务器，是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器,上nginx的并发能力确实在同类型的网页服务器中表现较好。
    
    SQLite:是一款轻型的数据库，是遵守ACID的关系型数据库管理系统，它的设计目标是嵌入式的，而且目前已经在很多嵌入式产品中使用了它，它占用资源非常的低，在嵌入式设备中，可能只需要几百K的内存就够了。

Apache + MySql + Php．

1、安装Apache

Apache可以用下面的命令来安装
sudo apt-get install apache2

Apache默认路径是/var/www/

其配置文件路径为： /etc/apache2/

可以通过：sudo vi /etc/apache2/ports.conf修改监听端口号

重启服务生效：sudo service apache2 restart


2、安装mysql
sudo apt-get install mysql-server
安装过程中，会出现一个提示符让你输入一个密码。
这个密码是mysql root用户的密码。


3、安装PHP
输入下面的命令，就可以安装PHP 5,以及PHP访问mysql数据库所需要的库。

sudo apt-get install php5

sudo apt-get install php5-mysql


4、测试

安装完成后，可以在浏览器中输入你路由器的IP或域名，就可以访问你的网站了。

你应该能看到一个页面显示“It works”，但是没有其它内容。

创建一个/var/www/index.php

    <?php
      print <<< EOT
    <!doctype html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Test successful</title>
    </head>
    <body>
    <h1>Test successful</h1>
    <p>Congratulations.</p>
    <p>Your webserver and PHP are working.</p>
    </body>
    </html>
    EOT;
     
    ?>




二、nginx + php + sqlite

1、安装nginx  web服务器 (约6MB)
sudo apt-get install nginx


2、启动nginx

sudo /etc/init.d/nginx start

nginx的www根目录默认在 /usr/share/nginx/www中


3、修改nginx的配置文件
sudo vi /etc/nginx/sites-available/default


1)以下几个选项注意一下：

    listen   8080;                                             ## listen for ipv4; this line is default and implied
    
    //监听的端口号，如果与其它软件冲突，可以在这里更改
    
    root /usr/share/nginx/www;
    
    //nginx 默认路径html所在路径
    
    index index.html index.htm index.php;
    
    //nginx默认寻找的网页类型，我们可以增加一个index.php


2)PHP脚本支持（不设这几项PHP脚本无法识别）

找到php的定义段，将这些行的注释去掉 ，修改后内容如下

    location ~ .php$ {
    　fastcgi_pass unix:/var/run/php5-fpm.sock;
    　fastcgi_index index.php;
    　include fastcgi_params;
    }
    
    php段中有一些其它定义，不要去动它，比如：
    #      fastcgi_split_path_info ...
    #      fastcgi_pass 127.0.0.1:9000

4)安装php和sqlite(约3MB)
sudo apt-get install php5-fpm php5-sqlite


5)重新加载nginx的配置
sudo /etc/init.d/nginx reload


6)测试html

通过主机的IE访问树莓派，可以看到主页(表示Web服务器已正常启动)


7)测试php

在树莓派中生成一php文件

sudo vi /usr/share/nginx/www/index.php

在文件中输入以下内容

    <?php
      print <<< EOT
    <!doctype html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <title>Test successful</title>
    </head>
    <body>
    <h1>Test successful</h1>
    <p>Congratulations.</p>
    <p>Your webserver and PHP are working.</p>
    </body>
    </html>
    EOT;
     
    ?>


存盘退出

IE访问一下这一页，说明php也是OK的
 ———————————————— 
版权声明：本文为CSDN博主「老徐拉灯」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xdw1985829/article/details/38919495