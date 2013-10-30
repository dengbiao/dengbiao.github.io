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


###ubuntu进程管理方法

转自：[http://www.cnblogs.com/xinzaibing/archive/2012/03/24/2415434.html](http://www.cnblogs.com/xinzaibing/archive/2012/03/24/2415434.html)  

    ps 显示当前进程  
    ps -l 显示详细信息   
    ps -u 以用户的格式显示

    
####相关字段说明  

    VF 进程状态标志   
    S 进程状态代码   
    UID 进程执行者ID   
    PPID 父进程标识(parent process ID)   
    PRI 进程执行的优先级(priority)   
    NI 进程执行优先级的nice值,负值表示其优先级较高   
    SZ 进程占用的内存大小   
    WCHAN 进程或系统调用等待时的地址  
    %CPU cpu使用百分比   
    %MEM 内存使用百分比   
    VSZ 占用虚拟内存大小   
    RSS 占用物理内存大小   
    START 进程开始时间

####kill 删除进程  
    
    kill pid 删除指定pid的进程   
    kill -l 查看所有可供传送的信号   
    kill -9 pid 强制删除进程,传送的是SIGKILL信号   
    kill -15 pid 强制删除进程,传送的是SIGTERM信号   
    kill -HUP pid 重启Deamon进程  

    
####free 查看内存使用状态  

    free -s 10 每10秒检查内存使用情况   
    nice 设置执行优先级,-20~19,19最低  
    sudo nice -2 vi 将vi的优先级调为-2   
    renice 修改执行优先级,-20~19,19最低  


####top 动态显示进程  

    按"P"键 按CPU使用时间排序   
    按"M"键 按内存使用多少排序   
    按"T"键 按执行时间多少排序   
    按"u"键 监视特定用户   
    按"K"键 删除进程   

    top -d 10 指定更新时间   
    lsof -p 查看进程打开的文件   
    jobs 命令查看后台作业


####ubuntu结束进程方法  


    1、打开终端  
    2、敲 ps -ef 查出进程的编号（就是PID那列）  
    3、敲 kill PID （如果PID是123456，则kill 123456）  
    4、OK了


在本地Ubuntu Linux系统运行大软件的时候，或者服务器长时间运行后，由于有些设计有缺陷的软件，容易出现假死的情况！

那程序假死了以后，我们该怎么办呢？其实这个 问题其实说简单也简单，直接结束进程不就OK了嘛！就像我们在Windows下面做的一样！下面来介绍几种Ubuntu Linux下面结束进程的几种方法！


####最安全杀死进程的方法  

杀死进程最安全的方法是单纯使用kill命令，不加修饰符，不带标志。  

首先使用ps -ef命令确定要杀死进程的PID，然后输入以下命令：

    kill -pid  

注释：标准的kill命令通常都能达到目的。终止有问题的进程，并把进程的资源释放给系统。然而，如果进程启动了子进程，只杀死父进程，子进程仍在运行，因此仍消耗资源。为了防止这些所谓的"僵尸进程"，应确保在杀死父进程之前，先杀死其所有的子进程。  

还可以使用如下命令来确定要杀死进程的PID或PPID  

    ps -ef | grep httpd  

以最优雅的方式来结束进程  

    kill -l PID  

-l选项告诉kill命令用好像启动进程的用户已注销的方式结束进程。当使用该选项时，kill命令也试图杀死所留下的子进程。但这个命令也不是总能成功--或许仍然需要先手工杀死子进程，然后再杀死父进程。


####TERM信号  

给父进程发送一个TERM信号，试图杀死它和它的子进程。

    kill -TERM PPID  
    killall命令    
    killall命令杀死同一进程组内的所有进程。其允许指定要终止的进程的名称，而非PID。  
    killall httpd   

####停止和重启进程  

有时候只想简单的停止和重启进程。如下：

    kill -HUP PID  

该命令让Linux和缓的执行进程关闭，然后立即重启。在配置应用程序的时候，这个命令很方便，在对配置文件修改后需要重启进程时就可以执行此命令。

    绝杀 kill -9 PID  
    同意的 kill -s SIGKILL  

这个强大和危险的命令迫使进程在运行时突然终止，进程在结束后不能自我清理。危害是导致系统资源无法正常释放，一般不推荐使用，除非其他办法都无效。

当使用此命令时，一定要通过`ps -ef`确认没有剩下任何僵尸进程。只能通过终止父进程来消除僵尸进程。如果僵尸进程被init收养，问题就比较严重了。杀死init进程意味着关闭系统。

如果系统中有僵尸进程，并且其父进程是init，而且僵尸进程占用了大量的系统资源，那么就需要在某个时候重启机器以清除进程表了

------------------------------

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

--------------------------------

###ubuntu 12.04 配置 VPN pptpd


在 Ubuntu 中建立 pptp server 需要的软件包为 pptpd，用 apt-get 即可安装：
    
    sudo apt-get <abbr title="Thanks zz!">install</abbr> pptpd

系统会自动解决依赖关系，安装好后，需要进行一番设置。首先编辑 `/etc/pptpd.conf`

    sudo nano /etc/pptpd.conf

去掉文件最末端的 localip 和 remoteip 两个参数的注释，并进行相应修改。这里，localip 是 VPN 连通后服务器的 ip 地址，而 remoteip 则是客户端的可分配 ip 地址。下面是我的配置：

    localip 10.100.0.1
    remoteip 10.100.0.2-10

编辑好这个文件后，我们需要编辑 `/etc/ppp/pptpd-options` 文件，还是用 nano 编辑，命令这里就不写了。这里绝大多数参数只需维持原来的默认值即可，我们只需要改变其中的 ms-dns 选项，为 VPN 客户端指派 DNS 服务器地址：

    ms-dns 202.113.16.10
    ms-dns 208.67.222.222

修改 `/etc/ppp/chap-secrets` 文件，这里面存放着 VPN 的用户名和密码，根据你的实际情况填写即可。如文件中注释所示，第一列是用户名，第二列是服务器名（默认写 pptpd 即可，如果在 `pptpd-options` 文件中更改过的话，注意这里保持一致），第三列是密码，第四列是 IP 限制（不做限制写 \* 即可）。

全部搞定后，我们需要重启 pptpd 服务使新配置生效：
	
    sudo /etc/init.d/pptpd restart

找一台 Windows 电脑，新建个 VPN 链接，地址填服务器的 IP（或域名），用户名密码填刚才设置好的，域那项空着（如果你在 `pptpd-options` 中设置了，这里就保持一致），点连接就可以了。正常情况下您应该能够建立与服务器的 VPN 链接了。


建立连接之后，您会发现除了可以访问服务器的资源，其余内外和互联网的内容均无法访问。如果需要访问这些内容的话，我们还需要进一步设置：

首先，开启 ipv4 forward。方法是，修改 `/etc/sysctl.conf`，找到类似下面的行并取消它们的注释：
	
    net.ipv4.ip_forward=1

然后使新配置生效：

    sudo sysctl -p

有些时候，经过这样设置，客户端机器就可以上网了（我在虚拟机上这样操作后就可以了）。但我在实验室的服务器上这样操作后仍然无法访问网络，这样我们就需要建立一个 NAT。这里我们使用强大的 iptables 来建立 NAT。首先，先安装 iptables：
     
    sudo apt-get install iptables

装好后，我们向 nat 表中加入一条规则：
	
    sudo iptables -t nat -A POSTROUTING -s 10.100.0.0/24 -o eth0 -j MASQUERADE
 

这样操作后，客户端机器应该就可以上网了。

但是，只是这样，iptables 的规则会在下次重启时被清除，所以还需要把它保存下来，方法是使用 `iptables-save` 命令：

    sudo iptables-save > /etc/iptables-rules

然后修改 `/etc/network/interfaces` 文件，找到 eth0 那一节，在对 eth0 的设置最末尾加上下面这句：

    pre-up iptables-restore < /etc/iptables-rules

这样当网卡 eth0 被加载的时候就会自动载入我们预先用 `iptables-save` 保存下的配置。

到此，一个 VPN Server/Gateway 基本就算架设完毕。当然，也许你按照我的方法做了，还是无法成功，那么下面总结一些我碰到的问题和解决方案：

#### 无法建立 VPN 连接

安装好 pptpd 并设置后，客户端还是无法建立到服务器的连接。造成的原因可能有以下几种：
		
    1. 服务器端的防火墙设置：PPTP 服务需要使用 1723(tcp) 端口和 gre 协议，因此请确保您的防火墙设置允许这两者通行。
    2. 如果服务器在路由器后面，请确保路由器上做好相应的设置和端口转发。
    3. 如果服务器在路由器后面，那么请确保你的服务器支持 VPN Passthrough。
    4. 如果客户端在路由器后面，那么客户端所使用的路由器也必须支持 VPN Passthrough。其实市面上稍微好点的路由器都是支持 VPN Passthrough 的，当然也不排除那些最最最便宜的便宜货确实不支持。当然，如果你的路由器可以刷 DD-Wrt 的话就刷上吧，DD-Wrt 是支持的。


#### 能建立链接，但"几乎"无法访问互联网

这里使用`"[几乎]"`这个词，是因为并不是完全不能访问互联网。症状为，打开 Google 搜索没问题，但其它网站均无法打开；SSH 可用，但 scp 不行；ftp 能握手，但传不了文件。我就遇到了这种情况，仔细 Google 后发现原来是 MTU 的问题，用 ping 探测了一下果然是包过大了。知道问题就好办了，我们可以通过 iptables 来修正这一问题。具体原理就不讲了，需要的自己 Google。这里只说解决方案，在 filter 表中添加下面的规则：

    sudo iptables -A FORWARD -s 10.100.0.0/24 -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1200
 
上面规则中的 1200 可以根据你的实际情况修改，为了保证最好的网络性能，这个值应该不断修改，直至能保证网络正常使用情况下的最大值。

好了，至此，一台单网卡 pptp-server 就算完成了。


------------------------------


###ubuntu server vsftpd 配置


####匿名用户权限控制 

    anonymous_enable=YES　　 #是否启用匿名用户  
    no_anon_password=YES 　　#匿名用户login时不询问口令  


下面这四个主要语句控制这文件和文件夹的上传、下载、创建、删除和重命名。  

	anon_upload_enable=（yes/no)； #控制匿名用户对文件（非目录）上传权限。  
	anon_world_readable_only=（yes/no）； #控制匿名用户对文件的下载权限  
	anon_mkdir_write_enable=（yes/no）； #控制匿名用户对文件夹的创建权限  
	anon_other_write_enable=（yes/no）； #控制匿名用户对文件和文件夹的删除和重命名  

注：匿名用户下载是使用的是nobody这个用户，所以相应的O这个位置要有R权限才能被下载。若想让匿名用户能上传和删除权限，必需设置  

	write_enable=YES #全局设置，是否容许写入（无论是匿名用户还是本地用户，若要启用上传权限的话，就要开启他）  
	anon_root=(none) #匿名用户主目录  
	anon_max_rate＝（0） #匿名用户速度限制  
	anon_umask＝（077） #匿名用户上传文件时有掩码(若想让匿名用户上传的文件能直接被匿名下载，就这设置这里为073)  
	chown_uploads=YES #所有匿名上传的文件的所属用户将会被更改成chown_username  
	chown_username=whoever #匿名上传文件所属用户名  

	  

####本地用户权限控制  

	write_enable=YES #可以上传(全局控制) 删除，重命名  
	local_umask=022 #本地用户上传文件的umask  
	userlist_enable=YES #限制了这里的用户不能访问  
	local_root #设置一个本地用户登录后进入到的目录  
	user_config_dir #设置用户的单独配置文件，用哪个帐户登陆就用哪个帐户命名，实现不同用户不同权限  
	download_enable #限制用户的下载权限  
	chown_uploads=YES #所有匿名上传的文件的所属用户将会被更改成chown_username  
	chown_username=whoever #匿名上传文件所属用户名  
	chroot_list_enable=YES　#如果启动这项功能，则所有列在chroot_list_file之中的使用者不能更改根目录，默认YES  
	chroot_list_file=/etc/vsftpd/chroot_list #指出被锁定在自家目录中的用户的列表文件。  
	chroot_list_enable=YES通过与chroot_local_user=YES/NO搭配能实现以下几种效果：


    1. 当chroot_list_enable=YES，chroot_local_user=YES时，在/etc/vsftpd/chroot_list文件中列出的用户，可以切换到其他目录；未在文件中列出的用户，不能切换到其他目录。
    2. 当chroot_list_enable=YES，chroot_local_user=NO时，在/etc/vsftpd/chroot_list文件中列出的用户，不能切换到其他目录；未在文件中列出的用户，可以切换到其他目录。
    3. 当chroot_list_enable=NO，chroot_local_user=YES时，所有的用户均不能切换到其他目录。
    4. 当chroot_list_enable=NO，chroot_local_user=NO时，所有的用户均可以切换到其他目录。


下面是个实例，希望对大家有用：  

1、只能上传。不能下载、删除、重命名。  

	cmds_allowed＝FEAT,REST,CWD,LIST,MDTM,MKD,NLST,PASS,PASV,PORT,PWD,QUIT,RMD,SIZE,STOR,TYPE,USER,ACCT,APPE,CDUP,HELP,MODE,NOOP,REIN,STAT,STOU,STRU,SYST  

2、对于参数的详细的解释  

	cmds_allowed=ABOR,ACCT,APPE,CWD,CDUP,DELE,HELP,LIST,MODE,MDTM,MKD,NOOP,NLST,PASS,PASV,PORT,PWD,QUIT,REIN,RETR,RMD,RNFR,RNTO,SITE,SIZE,STOR,STAT,STOU,STRU,SYST,TYPE,USER

	
    # ABOR - abort a file transfer 取消文件传输
    # CWD - change working directory 更改目录
    # DELE - delete a remote file 删除文件
    # LIST - list remote files 列目录
    # MDTM - return the modification time of a file 返回文件的更新时间
    # MKD - make a remote directory 新建文件夹
    # NLST - name list of remote directory
    # PASS - send password
    # PASV - enter passive mode
    # PORT - open a data port 打开一个传输端口
    # PWD - print working directory 显示当前工作目录
    # QUIT - terminate the connection 退出
    # RETR - retrieve a remote file 下载文件
    # RMD - remove a remote directory
    # RNFR - rename from
    # RNTO - rename to
    # SITE - site-specific commands
    # SIZE - return the size of a file 返回文件大小
    # STOR - store a file>只能下载。不能上传、删除、重命名。write_enable=NO  

3、只能上传、删除、重命名。不能下载。download_enable＝NO  

4、只能下载、删除、重命名。不能上传。  

    cmds_allowed=FEAT,REST,CWD,LIST,MDTM,MKD,NLST,PASS,PASV,PORT,PWD,QUIT,RMD,RNFR,RNTO,RETR,DELE,SIZE,TYPE,USER,ACCT,APPE,CDUP,HELP,MODE,NOOP,REIN,STAT,STOU,STRU,SYST  

		  

####虚拟用户设置

虚拟用户使用PAM认证方式。  

    pam_service_name=vsftpd #设置PAM使用的名称，默认值为/etc/pam.d/vsftpd。  
    check_shell=YES #（注意：仅在没有pam验证版本时有用,是否检查用户有一个有效的shell来登录 )  
    guest_enable= YES/NO #启用虚拟用户。默认值为NO。  
    guest_username=ftp #这里用来映射虚拟用户。默认值为ftp。  
    virtual_use_local_privs=YES/NO #当该参数激活（YES）时，虚拟用户使用与本地用户相同的权限。 #当此参数关闭（NO）时，虚拟用户使用与匿名用户相同的权限。默认情况下此参数是关闭的（NO）。  

		  

####访问控制设置

两种控制方式：一种控制主机访问，另一种控制用户访问。  

1、控制主机访问：  

    tcp_wrappers=YES/NO，设置vsftpd是否与tcp wrapper相结合来进行主机的访问控制。默认值为YES。如果启用，则vsftpd服务器会检查/etc/hosts.allow 和/etc/hosts.deny 中的设置，来决定请求连接的主机，是否允许访问该FTP服务器。这两个文件可以起到简易的[_防火墙_](http://www.07net01.com/security/)功能。  


2、控制用户访问：  

    /etc/vsftp/ftpusers #用于保存不允许进行FTP登录的本地用户帐号。就是vsftp用户的黑名单  
    /etc/vsftp/user_list# userlist_enable=yes（默认）时，启用该列表。注意：默认情况下，不管userlist设置成什么样子，vsftp的pam设置都会检查/etc/vsftpd/ftpusers中禁止的用户。  
    （1）当userlist_deny=yes（默认）时，所有在/etc/vsftp/user_list中的用户都被禁止登录，都不会提示让用户输入密码。  
    （2）当userlist_deny=no（默认）时，只有在/etc/vsftp/user_list中的用户才被允许登录。  

		  

####超时设置  

    idle_session_timeout=600 #空闲连接超时  
    data_connection_timeout=120 #数据传输超时  
    ACCEPT_TIMEOUT=60 #PASV请求超时  
    connect_timeout=60 #PROT模式连接超时  

		  

####服务器功能选项

    xferlog_enable=YES　　 #开启日记功能  
    xferlog_std_format=YES　　 #使用标准格式  
    log_ftp_protocol=NO　　 #当xferlog_std_format关闭且本选项开启时,记录所有ftp请求和回复,当调试比较有用.  
    pasv_enable=YES　　 #允许使用pasv模式  
    pasv_promiscuous+NO　　 #关闭[_安全_](http://www.07net01.com/security/)检查,小心呀.  
    port_enable=YES　　 #允许使用port模式  
    prot_promiscuous　　 #关闭安全检查  
    tcp_wrappers=YES　　 #开启tcp_wrappers支持  
    pam_service_name=vsftpd　　 #定义PAM 所使用的名称，预设为vsftpd。  
    nopriv_user=nobody　　 #当服务器运行于最底层时使用的用户名  
    pasv_address=(none)　　 #使vsftpd在pasv命令回复时跳转到指定的IP地址.  
		  

####服务器性能选项  

    ls_recurse_enable=YES #是否能使用ls -R命令以防止浪费大量的服务器资源  
    one_process_model #是否使用单进程模式  
    listen=YES 绑定到listen_port指定的端口,既然都绑定了也就是每时都开着的,就是那个什么standalone模式  
    text_userdb_names=NO　　 #当使用者登入后使用ls -al 之类的指令查询该档案的管理权时，预设会出现拥有者的UID，而不是该档案拥有者的名称。若是希望出现拥有者的名称，则将此功能开启。  
    use_localtime=NO　　 #显示目录清单时是用本地时间还是GMT时间,可以通过mdtm命令来达到一样的效果  
    use_sendfile=YES　　 #测试平台优化  

		  

####信息类设置

    ftpd_banner=welcome to FTP .　　#login时显示欢迎信息.如果设置了banner_file则此设置无效  
    dirmessage_enable=YES　　 #允许为目录配置显示信息,显示每个目录下面的message_file文件的内容  
    setproctitle_enable=YES　　 #显示会话状态信息,关!  

		  

####文件定义

    chroot_list_file=/etc/vsftpd/vsftpd.chroot_list #定义不能更改用户主目录的文件  
    userlist_file=/etc/vsftpd/vsftpd.user_list #定义限制/允许用户登录的文件  
    banner_file=/etc/vsftpd/banner #定义登录信息文件的位置  
    banned_email_file=/etc/vsftpd.banned_emails #禁止使用的匿名用户登陆时作为密码的电子邮件地址  
    xferlog_file=/var/log/vsftpd.log #日志文件位置  
    message_file=.message #目录信息文件  

		  

####目录定义

    user_config_dir=/etc/vsftpd/userconf　　#定义用户配置文件的目录  
    local_root=webdisk #此项设置每个用户登陆后其根目录为/home/username/webdisk。定义本地用户登陆的根目录,注意定义根目录可以是相对路径也可以是绝对路径。相对路径是针对用户家目录来说的.  
    anon_root=/var/ftp　　 #匿名用户登陆后的根目录  

		  

####用户连接选项

    max_clients=100　　 #可接受的最大client数目  
    max_per_ip=5　　 #每个ip的最大client数目  
    connect_from_port_20=YES　　 #使用标准的20端口来连接ftp  
    listen_address=192.168.0.2　　 #绑定到某个IP,其它IP不能访问，多网卡多IP机器时有用  
    listen_port=2121　　 #绑定到某个端口  
    ftp_data_port=2020　　 #数据传输端口  
    pasv_max_port=0　　 #pasv连接模式时可以使用port 范围的上界，0 表示任意。默认值为0。  
    pasv_min_port=0　　 #pasv连接模式时可以使用port 范围的下界，0 表示任意。默认值为0。  

		  

####数据传输选项、vsftp限速 

    anon_max_rate=51200 #匿名用户的传输比率(b/s)  
    local_max_rate=5120000 #本地用户的传输比率(b/s)  
		  

####安全选项

    Idle_session_timeout=600 #（用户会话空闲后10分钟）  
    Data_connection_timeout=120 #（将数据连接空闲2分钟断）  
    Accept_timeout=60 #（将客户端空闲1分钟后断）  
    Connect_timeout=60 #（中断1分钟后又重新连接）  
    Local_max_rate=50000 #（本地用户传输率50K）  
    Anon_max_rate=30000 # （匿名用户传输率30K）  
    Max_clients=200 #（FTP的最大连接数）  
    Max_per_ip=4 #（每IP的最大连接数）  
    Listen_port=5555 #（从5555端口进行数据连接）  


转自：Vsftpd匿名用户无法上传、删除、创建文件、修改文件的解决

编译：[（转）vsftp详细配置、/etc/vsftpd/vsftpd.conf](http://www.07net01.com/linux/_zhuan_vsftpxiangxipeizhi__etc_vsftpd_vsftpd_conf_17718_1348644341.html)


-------------------------------------------------




	




