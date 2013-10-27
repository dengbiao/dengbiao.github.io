---
layout: post
title: win7+virtualbox与ubuntu server虚拟机共享文件
---
老规矩，先描述应用场景。电脑windows 7 64位系统、virtualbox4.2.12 r84980、虚拟机ubuntu server 12.04 64位版本。现在想在host win7和ubuntu server之间共享文件。  

+++++++++++++++++++++++++++++我是分割线  场景描述完毕++++++++++++++++++++++++  

实现方案1：FTP 在虚拟机中搭建ftp服务器  

实现方案2：利用virtualbox的文件共享功能  ----本文采用方法  

+++++++++++++++++++++++++++++我是分割线  具体步骤+++++++++++++++++++++++++++  

考虑到FTP共享需要上传下载，放弃。下面具体讲述virtualbox共享实现方案。  

查看ubuntu的内核版本，命令：`uname -r`
获得输出结果如下：`3.2.0-23-generic`  根据自己系统版本不同而不同
下载同版本的内核头文件。根据我电脑的情况命令如下：
`sudo apt-get install build-essential linux-headers-3.2.0-23-generic`
在virtualbox界面上依次选择： 设备-->安装增强功能  

这一步系统会将`virtualbox`安装目录下的`VBoxGuestAdditions.iso`加载到光驱
挂载光驱文件到mnt目录并执行脚本，命令如下：  
{% highlight python %}
mount /dev/cdrom /mnt
cd /mnt
sudo ./VBoxLinuxAdditions.run
{%  endhighlight %}
安装完成后在`virtualbox`界面依次选择：设置-->共享文件夹-->添加共享文件夹-->共享文件夹路径选择你需要映射的主机共享路径-->输入共享文件夹名称（downloads这是我写的名称）-->勾选固定分配 （其他两个 自动挂载、只读分配 看情况自己选择） 
依次执行以下几个命令：  

`sudo umount /mnt` (取消之前的挂载）
`sudo mount -t vboxsf /downloads /mnt`  

挂载完成，可以共享了。
