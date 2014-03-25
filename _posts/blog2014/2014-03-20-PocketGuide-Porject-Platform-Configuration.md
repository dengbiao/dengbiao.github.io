---
layout: post
title: PocketGuide掌上导游项目开发第2篇-后台环境搭建
author: dengbiao
categories:
- develop
- python
keywords: [掌上导游,安卓,景点导航,掌上导游后台环境搭建,android,python,flask,flask-sqlalchemy,sqlalchemy]
description: 掌上导游项目后台开发环境搭建

---

千里之行，始于足下。本文将讲解掌上导游项目（PocketGuide)的后台环境搭建。但是本文的内容并不会包含整个后台需要使用到的平台或者插件。因为很多东西是在使用过程中需要用到才会去安装的。暂时不会用到的东西咱们先不讲。不然你也看不懂不是。

### Python安装
Python在Mac下是自带的，并不需要安装。所以使用mac的同学和朋友们并不需要这一步。直接跳过即可。  
ubuntu的使用用户安装python命令如下：`sudo apt-get install python`  。然后等待安装完成即可。  
如果你想自己安装python，<http://legacy.python.org/ftp//python/>这个链接里面有各个版本的安装包，下载下来然后自己配置安装即可。具体操作请自行google。参考方法<http://blog.csdn.net/huhui_cs/article/details/8748419>。注意：本文不负责所有教学，也不提倡做一个伸手党。能力的提升靠的是一点点的积累，从不知道到知道，而不是简单的全部从别处索取。这样只会让人越来越愚钝。***个人观点，不喜勿喷***。  

<!--more-->

###Flask安装
这个很简单，我也是参照官网来的，上链接<http://flask.pocoo.org/docs/installation/#installation>,中文版链接<http://docs.jinkan.org/docs/flask/installation.html>，推荐看英文的，因为很多地方中文翻译的有问题，不是很好理解。当然，你也可以直接看中文的，这个个人喜好，不强求。   
因为安装这个的时候，我自己有些地方没理解好，遇到了些小问题。这里我自己粗略总结下流程：  

####第一步： 安装virtualenv  

其实这一步对于只用一个版本的python的人来说是可有可无的，甚至有的时候你会很奇怪为什么环境配置好了之后，下次来运行又说缺少包，其实是因为自己没有进入到这个virtualenv的环境中。但是还是强烈建议安装virtualenv，因为你不知道你下一个应用安装的版本和插件会不会和这个应用的冲突。简单的说，牛人的做法，当你还不是牛人的时候，先学着吧，等你牛了，别人就来学你的了。  

mac用户命令（推荐）：  

    sudo pip install virtualenv  
 
或者（不推荐）：  

    sudo easy_install virtualenv

至于pip和easy_install的区别，stackoverflow上有人有具体分析，网址:<http://stackoverflow.com/questions/3220404/why-use-pip-over-easy-install>,内容如下：

    1.All packages are downloaded before installation. Partially-completed installation doesn’t occur as a result.
    2.Care is taken to present useful output on the console.
    3.The reasons for actions are kept track of. For instance, if a package is being installed, pip keeps track of why that package was required.
    4.Error messages should be useful.
    5.The code is relatively concise and cohesive, making it easier to use programmatically.
    6.Packages don’t have to be installed as egg archives, they can be installed flat (while keeping the egg metadata).
    7.Native support for other version control systems (Git, Mercurial and Bazaar)
    8.Uninstallation of packages.
    9.Simple to define fixed sets of requirements and reliably reproduce a set of packages.

####第二步：使用virtualenv

    $ mkdir myproject
    $ cd myproject
    $ virtualenv venv
    New python executable in venv/bin/python
    Installing distribute............done.

其中要说明的是参数问题：上面的三个命令中 ***myproject*** 和 ***venv***为用户自定义参数，可以按照自己需求修改。特别是***venv***跟第三步的步骤相关。请带着思考的头脑跟进，不然你会在这个上面浪费很多时间。原谅我自己在这上面犯傻了。。。  

####第三步：激活virtualenv环境

    $ . venv/bin/activate

其中的***venv*** 为第二步中使用的参数。

####第四步：安装Flask 

    pip install Flask

完毕。

###安装mysqldb模块
后面的程序会用到该模块，如果没有安装会遇到错误：***No module named MySQLdb***

mac系统用户：

    pip install mysql-python

ubuntu系统用户：
    
    sudo apt-get install  python-mysqldb

###安装mysql
装数据库插件之前，保证数据库已经装上，并且启动服务。 

mac命令安装：

    brew install mysql 

ubuntu命令安装：

    sudo apt-get install mysql

配置教程自行google.这里仅仅说一下mac下mysql配置。   
用`brew install mysql`安装成功之后，会出现以下提示：

    To connect:
    mysql -uroot

    To have launchd start mysql at login:
    ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
    Then to load mysql now:
    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    Or, if you do not want/need launchctl, you can just run:
    mysql.server start  

翻译成中文就是可以使用`mysql -uroot`进行连接。默认密码为空。 然后就是如果你想设置开机自动启动的操作步骤。如果你只希望在使用的时候启动mysql，则使用命令`mysql.server start`开启即可。

如果想设置mysql密码，可是使用命令`mysql_secure_installation  mysql`，提示输入root密码，直接回车即可，然后输入新密码，剩下的全部选择***Y***即可。


###安装Flask-SQLAlchemy
Flask-SQLAlchemy是机遇SQLAlchemy的一个flask插件。还挺好用的。官网地址：<http://flask.pocoo.org/docs/patterns/sqlalchemy/> 

mac安装命令,如果没有安装sqlalchemy，pip会自动帮你下载好：

    sudo pip install flask-sqlalchemy

ubuntu下apt-get没有找到对应的包，所以从官网上下载了Flask-SQLAlchemy的安装包，解压，用命令`sudo python setup.py install`安装即可(mac用户也适用，如果您觉得pip下载速度慢，可以试试自己下载与安装)。中间遇到了问题`No module named setuptools`,很明显缺少了setuptoolls包， 安装命令跟上`sudo pip install setuptools`. 

###结束语

到此，基本环境搭建完毕，后面将继续介绍后台项目结构和数据库设计等内容。


