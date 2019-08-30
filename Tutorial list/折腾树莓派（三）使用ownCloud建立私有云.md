# 折腾树莓派（三）使用ownCloud建立私有云

ownCloud prerequisites

开始安装owncloud之前，首先需要Apache, MySQL/MariaDB, PHP的支持，以下使用MySQL作为数据库进行安装



    sudo apt-get install apache2 mysql-server libapache2-mod-php5
    sudo apt-get install php5-gd php5-json php5-mysql php5-curl
    sudo apt-get install php5-intl php5-mcrypt php5-imagick

第一行安装运行完之后，MySQL需要配置root用户的密码
安装ownCloud

下载ownCloud

    https://owncloud.org/install/

进入download页面后复制.tar.bz2文件的链接，本文下载的版本为9.1.0（将文件下载至当前登录用户的Public文件夹下）



    cd Public/
    wget https://download.owncloud.org/community/owncloud-9.1.0.tar.bz2

下载完后可考虑进行MD5或SHA256校验，这里省略此步骤，直接进行解压安装操作



    tar -xjf owncloud-9.1.0.tar.bz2

解压步骤有些慢，稍等一会儿，接下来复制owncloud文件夹至webserver的文件根目录下，若没有更改过Apache设置，则直接执行以下命令



    sudo cp -r owncloud /var/www

至此，ownCloud安装完毕
Apache服务器配置

以下步骤需要使用root权限，首先使用su - root切换至root用户



    cd /etc/apache2/sites-available/
    vim owncloud.conf

将一下文字复制进owncloud.conf配置文件，注意复制时首字母‘A’可能没复制进去，导致下面的重启步骤出问题





    Alias /owncloud "/var/www/owncloud/"
    
    <Directory /var/www/owncloud/>   Options +FollowSymlinks   AllowOverride All
    
     <IfModule mod_dav.c>   Dav off  </IfModule>
    
     SetEnv HOME /var/www/owncloud  SetEnv HTTP_HOME /var/www/owncloud
     </Directory>

将配置文件symlink到/etc/apache2/sites-enabled下

ln -s /etc/apache2/sites-available/owncloud.conf 

    /etc/apache2/sites-enabled/owncloud.conf

创建文件链接后，可以看到/etc/apache2/sites-enabled/文件夹下多了一个owncloud.conf文件，此文件的更改和/etc/apache2/sites-available/下owncloud.conf文件的更改同步

接下来为可选设置，但建议还是设置一下



    a2enmod rewrite
    a2enmod headers
    a2enmod env
    a2enmod dir
    a2enmod mime

禁用服务器默认的认证方式：在上面提到的owncloud.conf文件中<Directory部分加入以下内容

    Satisfy Any

重启Apache



    service apache2 restart

开启SSL



    a2enmod ssl
    a2ensite default-ssl
    service apache2 reload

ownCloud 配置

首先，把owncloud目录的给你的HTTP user，树莓派原生系统下默认为www-data



    chown -R www-data:www-data /var/www/owncloud/

接下来，使用浏览器访问以下地址创建管理员账号

    http://localhost/owncloud

其中localhost使用树莓派内网IP代替，设置完管理员账号后即可登录
将ownCloud开放到外网

开放外网之前先要进行安全设置
设置可信任域名

要使用固定域名或IP访问你的ownCloud，首先必须将其添加进/var/www/owncloud/config下的config.php文件下，更改内容如下



    'trusted_domains' =>
      array (
       0 => 'localhost',
       1 => 'server1.example.com',
       2 => '192.168.1.50',
    ),

第一行为树莓派本地，第二行为将要使用的固定域名，第三行为树莓派内网IP。如外网有固定IP，也可将固定IP加至此列表。
为owncloud文件夹设置权限

之前配置时已经设置过owncloud文件夹由www-data用户访问，即以下命令



    chown -R www-data:www-data /var/www/owncloud/

为安全起见，建议将/var/www/owncloud/.htaccess和/var/www/owncloud/data/.htaccess设置为root用户可读写，www-data用户仅只读



    chown root:www-data /var/www/owncloud/.htaccess
    chown root:www-data /var/www/owncloud/data/.htaccess

注：ownCloud官网有提供一个权限设置脚本，建议不要使用，因上次树莓派被搞崩溃就是因为在执行此脚本时不知道什么原因将树莓派所有文件设置为www-data用户所有，连root用户都失去了权限。。。
设置域名默认访问页面

设置前域名默认指向Apache2默认页面，将/etc/apache2/sites-enabled/下的000-default.conf和default-ssl.conf文件中的

DocumentRoot /var/www/html

改为

DocumentRoot /var/www/owncloud
设置http自动转https访问

更改/etc/apache2/site-enabled/000-default.conf

在<VitualHost *:80>下面添加如下三行



    RewriteEngine on
    RewriteCond %{SERVER_PORT} !^443$
    RewriteRule ^/(.*) https://%{HTTP_HOST}/$1 [NC,R,L]

端口转发

该设置的安全设置设置完毕后，即可对内网的树莓派进行端口转发，将其开放至外网。端口转发时建议将默认的80、443等端口转发为其他端口，因为80、443等端口可能被网络服务提供商禁用，导致无法顺利开放至外网。

对于动态IP，可考虑去no-ip类似网站申请一个免费域名，并将其定时更新脚本安装在树莓派上，设置为开机自动启动即可。

    https://www.noip.com/
 ———————————————— 
版权声明：本文为CSDN博主「Meow323」的原创文章，遵循CC 4.0 by-sa版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Meow323/article/details/52409142