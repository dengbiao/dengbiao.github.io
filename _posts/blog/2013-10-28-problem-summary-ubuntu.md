---
layout: post
title: ubuntu问题汇总贴
tags: [ubuntu, problems, summary]
---
在使用ubuntu的过程中经常遇到各种各样的小问题，有时候觉得问题比较小，很容易通过google就解决了。但是，这些小问题其实挺消耗时间的。所以建立这个问题汇总贴。在使用ubuntu中遇到的各种小问题进行汇总。希望能够帮助遇到同样问题的人找到解决办法。同时，也是给自己的学习历程留下痕迹。  

---------------

###ubuntu用户添加
以这段代码进行分析：`sudo useradd db -g tcteam -m -s /bin/bash`  

{% highlight console %}
db: 用户名 
tcteam: 用户组名  
-m: 同时创建用户目录  
-s /bin/bash : 指定shell为bash  
{% endhighlight %}

<!--more-->

---------------

###ubuntu vi配置文件
直接提供链接下载：[ubuntu-vimrc](/assets/uploads/vimrc.txt)

---------------

###ubuntu 终端忽略大小写
在终端输入以下命令:``echo "set completion-ignore-case on">>~/.inputrc``  

---------------

###ubuntu 12.10 安装dropbox失败 解决办法  

万恶的gfw。  

ubuntu软件中心下载的dropbox安装完成后，用命令sudo dropbox start -i 安装失败。提示：  

{% highlight console %}
csumt@vivi-pc:~/Desktop$ sudo dropbox start -i
Starting Dropbox
Dropbox is the easiest way to share and store your files online. Want to learn more? Head to http://www.dropbox.com/
Error: Trouble connecting to Dropbox servers. Maybe your internet connection is down, or you need to set your http_proxy environment variable.
URL that failed to download: http://www.dropbox.com/download?plat=lnx.x86_64
Error: [Errno 104] Connection reset by peer
The installation of Dropbox failed.
{% endhighlight %}

顿时头痛不已。这么好用的东西难道就不能用？  

万恶的GFW  

google之。方法如下：

{% highlight console %}
1.去官网下载drobox安装包。我的是dropbox_1.4.0_amd64.deb  
2.然后下载dropbox-lnx.x86-1.6.17.tar。  
3.解压缩 dropbox-linux.tar.gz ，并将解压出来的 .dropbox-dist 文件夹放入你的 根目录，也就是 /home/你的名字 目录下（新版本的放到/var/lib/dropbox中，不然安装不了）  
4.安装dropbox_1.4.0_amd64.deb。  
{% endhighlight %}


ok到此安装完毕。


-------------------------

###ubuntu 下配置 uamp

#### 1.准备工作  

如果之前安装过LAMP，不完整或者有错误，可以依照如下方式卸除：
    
    #apt-get remove --purge apache2 apache2-mpm-prefork apache2-utils apache2.2-common libapache2-mod-php5 #apt-get remove --purge libapr1 libaprutil1 libdbd-mysql-perl libdbi-perl libmysqlclient15off #apt-get remove --purge libnet-daemon-perl libplrpc-perl libpq5 mysql-client-5.0 mysql-common #apt-get remove --purge mysql-server mysql-server-5.0 php5-common php5-mysql  

这将卸除一切LAMP有关的软件安装以及配置文件。然后执行：

<!--more-->
    # rm -R /etc/php5

将可能存在的php5的目录删除。  

执行自动移除和清理：
    
    # apt-get autoremove   
    # apt-get autoclean

#### 2.安装LAMP  

在新立的里勾选LAMP SERVER即可。如果分布安装，则执行下面的命令：
    
    # apt-get install apache2  
    # apt-get install php5   
    # apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql

这将自动安装并默认配置apache2、php5以及mysql。安装期间mysql会需求管理员账户密码。  

apache2的默认的目录为/var/www，安装完后为root拥有。可以修改其权限：  

    # chmod -R 777 /var/www


重新设置目录和apache2的更多配置信息，参阅Linux中配置apache2或参阅apache2官方http://www.apache.org/ 的文档。  

设置php解析：
    
    # apt-get install libapache2-mod-php5  
    # a2enmod php5

重启apache2：
    
    # /etc/init.d/apache2 restart

安装完成。

#### 3.安装phpmyadmin  

可以到官网下载phpmyadmin：[http://www.phpmyadmin.net/home_page/index.php](http://www.phpmyadmin.net/home_page/index.php) 或者直接获取：  

    # wget http://downloads.sourceforge.net/sourceforge/phpmyadmin/phpMyAdmin-3.2.0.1-all-languages.tar.gz

将其解压到/var/www/下，复制其配置范例到配置文件：


-----------------------------

###ubuntu安装dropbox

在网上或者源里面下载dropbox之后，安装不成功。貌似是被墙了。google告诉我们在terminal中直接下载以下内容 并解压之后即可  
    
    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf   

新版本的0.7.1-12`https://dl-web.dropbox.com/u/17/dropbox-lnx.x86-1.6.17.tar.gz`在这个地址下载后要解压到`/var/lib/dropbox`下面

下载好之后可以用`~/.dropbox-dist/dropboxd` 便可运行


--------------------------------

###ubuntu触摸板失灵 解决办法

在终端输入以下代码  

    sudo modprobe -r psmouse  
    sudo modprobe psmouse proto=imps    
    
然后打开options文件：

    sudo gedit /etc/modprobe.d/options    
    
添加 `options psmouse proto=imps`  

保存改动重启。

---------------------------------

###ubuntu server 12.04 忘记mysql密码


忘了mysql密码，从网上找到的解决方案记录在这里。
编辑mysql的配置文件`/etc/mysql/my.cnf`，在\[mysqld\]段下加入一行`"skip-grant-tables"`。

重启mysql服务

    abbuggy@abbuggy-ubuntu:~$ sudo service mysql restart  
    mysql stop/waiting  
    mysql start/running, process 18669  


用空密码进入mysql管理命令行，切换到mysql库。

    abbuggy@abbuggy-ubuntu:~$ mysql  
    Welcome to the MySQL monitor.  Commands end with ; or \g. 
    mysql> use mysql 
    Database changed 


执行`update user set password=PASSWORD("new_pass") where user='root'`; 把密码重置为`new_pass`。退出数据库管理。

    mysql> update user set password=PASSWORD("new_pass") where user='root';   
    Query OK, 0 rows affected (0.00 sec)   
    Rows matched: 4  Changed: 0  Warnings: 0   
    mysql>quit 


回到`vim /etc/mysql/my.cnf`，把刚才加入的那一行`"skip-grant-tables"`注释或删除掉。

再次重启mysql服务sudo service mysql restart，使用新的密码登陆，修改成功。

    abbuggy@abbuggy-ubuntu:~$ mysql -uroot -pnew_pass 
    Welcome to the MySQL monitor.  Commands end with ; or \g. 
    mysql>

---------------------------------

###ubuntu 支持GD 图形 水印

ubuntu 支持GD 图形 水印
为支持更高级的图片显示功能需要安装GD图形支持

在ubuntu Server 下，装gd，命令为：  

    sudo apt-get install php5-gd  

重启apache：  

    /etc/init.d/apache2 restart


----------------------------------


###ubuntu unzip 安装以及命令参数说明


安装命令：
    
    sudo apt-get install unzip


解压.zip文件命令：`unzip`

unzip命令能够将被winzip压缩的文件解压。

unzip命令的执行方式为：

    unzip [-选项]  压缩文件名.zip

例如想将file1.zip文件在当前目录下解压，则执行命令为：
    
    unzip  file1.zip

如果只想查看压缩文件里的文件目录，但是并不想解压，则执行命令为：

    # unzip -v file1.zip

将file1.zip文件在/home/zip目录中进行解压，但是如有相同的文件则并不覆盖原文件，执行命令为：
    
    unzip -n file1.zip -d /home/zip 

unzip命令的选项参数说明:

    -v 查看文件目录列表，但不解压  
    -d 将文件解压到指定目录中  
    -n 不覆盖原来已经存在的文件  
    -o 覆盖已存在的文件并且不需要用户确认  

------------------------------------------------


###github 提交记住用户名密码

    git config --global credential.helper 'cache --timeout 3600'
	
------------------------------------------------

###ubuntu重启后resolv.conf被清空

其实这个问题很简单.resolv.conf文件是一个软链接,指向的是`/run/resolvconf/resolv.conf`文件.而这个文件的内容只有两行

    #Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
    #DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN

注意到这里的话,其实它很明确的告诉了我们不要手动去修改它,因为他会被覆盖的.

所以留给我们的办法就只有两个:

    1.修改`/etc/network/interfaces`文件的时候加入指定`nameserver`的记录
    2.修改会用来覆盖resolv.conf文件的base文件(`/etc/resolvconf/resolv.conf.d/base`)

第一种方法修改`etc/network/interface`文件内容,示例如下:

    iface eth0 inet static 
    address 192.168.1.100 
    netmask 255.255.255.0 
    gateway 192.168.1.1 
    dns-nameservers 202.197.64.6 8.8.8.8 

第二种方法修改`/etc/resolvconf/resolv.conf.d/base`文件内容,示例如下:

    nameserver 202.197.64.6
    nameserver 8.8.8.8




