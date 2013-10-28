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



