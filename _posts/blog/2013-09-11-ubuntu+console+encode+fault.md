---
layout: post
title: ubuntu终端显示乱码问题
---

已经几次遇到这个问题了。每次安装完ubuntu之后，各种编码问题，很头疼。今天又遇到了。把找到的方法写下来。下次查阅方便。

`sudo vi /var/lib/locales/supported.d/local`

将文件内容修改如下：
{% highlight python %}
en_US.UTF-8 UTF-8  
zh_CN.UTF-8 UTF-8  
zh_CN.GBK GBK  
zh_CN GB2312  
{% endhighlight %}

修改以下文件：

`sudo vi /etc/default/locale`

修改文件内容如下：

{% highlight c %}
LANG=”zh_CN.UTF-8″
LANGUAGE=”zh_CN:zh”
LC_NUMERIC=”zh_CN.UTF-8″
LC_TIME=”zh_CN.UTF-8″
LC_MONETARY=”zh_CN.UTF-8″
LC_PAPER=”zh_CN.UTF-8″
LC_NAME=”zh_CN.UTF-8″
LC_ADDRESS=”zh_CN.UTF-8″
LC_TELEPHONE=”zh_CN.UTF-8″
LC_MEASUREMENT=”zh_CN.UTF-8″
LC_IDENTIFICATION=”zh_CN.UTF-8″
{% endhighlight %}

执行命令`sudo locale-gen`登出然后登入即可。

参考：http://www.2cto.com/os/201211/165712.html
