---
layout: post
title: ubuntu 12.04 配置 VPN pptpd
categories: [ubuntu, vpn , pptpd]
tags: [ubuntu, vpn, pptpd]
---

在 Ubuntu 中建立 pptp server 需要的软件包为 pptpd，用 apt-get 即可安装：
    
    sudo apt-get <abbr title="Thanks zz!">installabbr> pptpd

系统会自动解决依赖关系，安装好后，需要进行一番设置。首先编辑 `/etc/pptpd.conf`

    sudo nano /etc/pptpd.conf

去掉文件最末端的 localip 和 remoteip 两个参数的注释，并进行相应修改。这里，localip 是 VPN 连通后服务器的 ip 地址，而 remoteip 则是客户端的可分配 ip 地址。下面是我的配置：

    localip 10.100.0.1
    remoteip 10.100.0.2-10

<!--more-->

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
