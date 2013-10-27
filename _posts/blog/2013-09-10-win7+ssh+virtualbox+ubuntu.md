---
layout: post
title: windows 7下用ssh连接virtualbox中的ubuntu
tags: [win7 ssh virtualbox ubuntu]
---
首先讲一下自己的环境和应用场景。

之前是用的vagrant配置好的ubuntu环境。速度是很快，但是后来发现在学习过程中不太合适，因为经常要安装很多软件。vagrant更加适合于大家协同工作的时候有一个相同的测试环境。相当于一台装了还原卡的电脑。所以，想了想，用virtualbox装了一个ubuntu12.04server
64bit版。然后用ssh连接，不就达到了可修改的vagrant效果么。其实，你会发现直接用virtualbox提供的界面操作也是一样的，根本不需要用ssh连接。对，是这样的。不过个人表示不太喜欢那个界面。所以应用场景就出现了。

<!--more-->  

电脑：windows 7 64位系统，软件：装有ubuntu 64位虚拟机的virtuabox，装有ssh的cygwin（个人比较喜欢它的界面）。

环境描述完毕。方法很简单。在本机上做一个端口映射即可。注意，不需要修改虚拟机的联网方式，仍然是NAT上网。（有些教程是通过桥接来实现ssh链接的，这里的方法是端口映射，方法不同）

1. 第一步：将virtualbox安装路径加入到PATH中，或者直接打开virtualbox的安装路径（我电脑上的为：C:\Program Files\Oracle\VirtualBox），然后按住shift键并点击右键，选择在此处打开命令窗口。（理由：virtualbox安装路径下有个工具叫VBoxManage.exe，这里需要用到它)
2. 按以下命令格式输入命令：`VBoxManage  --natpf<1-N> [<name>],tcp|udp,[<hostip>],<hostport>,[<guestip>], <guestport>`
举例：``VBoxManage modifyvm "VM name" --natpf1 "guestssh,tcp,,3333,22"``
注：如果需要修改端口映射，需要先删除之前建立的映射，命令格式如下：`VBoxManage --natpf<1-N> delete <name>` 举例：`VBoxManage --natpf1 delete guestssh`
更多参数可参考：[http://www.virtualbox.org/manual/ch08.html](http://www.virtualbox.org/manual/ch08.html)
3. 打开cygwin，输入以下命令格式进行连接：`ssh -p<port> <userName>@localhost` 例如：`ssh -p3333 biao@localhost.`其中port代表你上面设置的映射端口，userName表示你虚拟机中的用户名，然后在弹出的提示框中输入密码即可。第一次连接会出现确认提示，输入yes即可。
######我是分割线之界面配置方法  
由于测试需求，需要添加主机到虚拟机的web映射关系。应用场景如下：  

virtualbox安装的ubuntu server版虚拟机中安装了apache服务器。希望在主机浏览器上可以访问虚拟机中的web程序。

界面配置方案:  

![](/assets/uploads/4878242821472382364.jpg)  

![](/assets/uploads/156500087151292641.png)  

![](/assets/uploads/3167437913025476657.png)  


