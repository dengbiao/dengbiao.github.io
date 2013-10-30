---
author: dengbiao
comments: true
date: 2013-03-14 12:05:29+00:00
layout: post
title: 'android开发环境配置问题汇总贴'
categories:
- Accumulation
---

###1.adb rejected install command with: device not found  

华为c8813的手机。在ubuntu 12.10下联机调试。eclipse不识别设备：运行程序的时候报错adb rejected install command with: device not found.
google之后，发现是华为手机的问题。解决办法也很牛逼：  

拨号：`*#*#2846579#*#*`  

`ProjectMenu->后台设置->USB端口配置->Google 模式`  
    
设置完毕重启即可。

ps：国产手机伤不起

<!--more-->

-----------------------

###2. adb]Failed to parse the output of 'adb version'

ubuntu 12.10 安装android SDK + eclipse不能运行,报错  

``adb]Failed to parse the output of 'adb version'``

`adb: error while loading shared libraries: libncurses.so.5`  

解决办法：  

`sudo apt-get install ia32-libs`

---------------------






