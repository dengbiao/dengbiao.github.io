---
layout: post
title: PocketGuide掌上导游项目开发第1篇-平台介绍与搭建
author: dengbiao
categories:
- develop
- android
keywords: 掌上导游,安卓,景点导航,android,python,flask,flask-sqlalchemy,sqlalchemy,pocket guide,XListview,Slidingmenu,umeng
description: PocketGuide（掌上导游）项目是一个基于android开发的移动景点导航app。本文对该项目的组织架构进行了完整的分析。包括项目前后台环境，应用层级结构，API接口说明等。

---

前面的博客提到了最近要做一个手机景点导航的应用。到昨天已经完工，今天补加了一个分享功能。数了一下时间，正好20天，比预期的时间要久一些，不过整个过程中还是学到了很多的东西。本来的打算是边学习边写博客，来记录整个学习过程，但是由于某些原因，需要尽快完成应用的开发，所以没有一点点记录，只保留了学习过程中的一些笔记，现在回过头来根据这些笔记来整理博文。

<!--more-->

### 项目需求
PocketGuide项目最终的需求是完成一个手机平台的景点导航软件。包括景点推荐、周边景点查询(定位，周边查询与距离排序)、景点地区归类、排序（按不同属性，包括人气、收藏、价格、评论等）、景点导航（定位与路径计算）、景点和应用分享等功能。
对项目需求进行分析与整理，PocketGuide项目主要分为三个部分，数据后台管理平台，数据访问接口以及手持设备端APP。

1. 数据后台管理平台   包含景点数据添加与管理、用户数据添加与管理等功能
2. 数据访问接口      提供景点、地区、用户数据等统一访问接口
3. 手持设备端APP    提供友好的用户界面，对接数据访问接口。

### 平台选择
由于对python产生了兴趣，这次开发没有选择自己最擅长的java来开发。其实很重要的原因是喜欢上了mac和vim，不喜欢用eclipse（虽然写android还是没有逃脱用eclipse的悲剧），感觉用vim很酷，慢慢习惯了之后也感觉到了效率的提升，终于理解以前认识的那些用vim的程序员们所说的用vim不是为了装逼，而是真的能提升工作效率。Nice。

最终，数据后台管理平台以及数据访问接口选择了在**mac**下基于**python**开发。其中用到的python框架有**flask**，没有理由，就是以前在一个工作室实习的时候，别人介绍过TA，知道了TA。仅此而已。数据库选择的是**mysql**，数据库持久化层框架用的是**flask-sqlalchemy**。手持设备端APP选择了**android**平台，为什么不选iphone，因为穷，没有测试机。人艰不拆。开发工具选择的是**eclipse**。**android studio**还没有用熟，这次由于时间问题，没有选择TA了。  

另外要说明的是定位与周边查询用到了**geohash**地理位置编码。具体原理可以查看[geohash维基百科](http://en.wikipedia.org/wiki/Geohash)

### 平台搭建
接下来要讲的是项目开发平台搭建。由于开发选择的系统平台为**MAC**，所以以下内容为MAC平台下的搭建流程。如果你选择的是windows或者linux，以下内容仅供参考。

#### python+flask
python在mac下已经自己集成了。不需要安装，但是各种版本可能会有不同。我使用的是**Python 2.7.5**,虽然已经python3了，但是似乎还不够完善（不知正确与否，道听途说而已，没有验证）。可以通过在终端用命令`python -V`查看版本号。


flask安装过程主要参考的是[官网](http://flask.pocoo.org/docs/)。中文版地址：[flask中文版教程](http://docs.jinkan.org/docs/flask/index.html).有些地方翻译的不是很好，我英语不怎么好，都是两边照着看过来的。。。惭愧。。。具体的Flask、Flask-sqlalchemy环境搭建将在下一篇中进行说明。

#### android
android的开发环境搭建，网上一抓一大把，这里就不详细介绍了。主要跟大家说一下用的sdk版本号。***eclipse+SDK Tool22.3+Android4.0.3 API15***. 







