---
layout:post
title:ubuntu server vsftpd 配置
categories: [ubuntu, vsftpd, ftp]
tags: [ubuntu, vsftpd, ftp]
---

####匿名用户权限控制 

    anonymous_enable=YES　　 #是否启用匿名用户  
    no_anon_password=YES 　　#匿名用户login时不询问口令  


下面这四个主要语句控制这文件和文件夹的上传、下载、创建、删除和重命名。  

    anon_upload_enable=（yes/no)； #控制匿名用户对文件（非目录）上传权限。  
    anon_world_readable_only=（yes/no）； #控制匿名用户对文件的下载权限  
    anon_mkdir_write_enable=（yes/no）； #控制匿名用户对文件夹的创建权限  
    anon_other_write_enable=（yes/no）； #控制匿名用户对文件和文件夹的删除和重命名  

<!--more-->

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

