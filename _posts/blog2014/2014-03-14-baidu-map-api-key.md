---
layout: post
title: 百度地图API key申请申请详细步骤
categories:
- develop
- android
tags: [android,baidumap,api,key]
image: /assets/uploads/baidumapapi.png
---

开发移动app应用，经常会遇到定位与导航需求，开发者自己去实现不太可能，一个是工作量大，另一个是程序员世界最常用的话`不要重复制造轮子`。百度在地图界混迹如此之久，积累的东西还是可以的。

最近开发一个景点相关的项目，借用百度地图提供的API来实现项目中的定位与导航需求。根据百度地图官方网站的引导，申请了开发者API。但是在请求路径规划的时候，总是遇到认证失败，error:102的错误。
因此，将百度地图API申请的相关流程记录下来。希望能够给需要用百度地图开发的程序员们一些引导。

<!--more-->

###申请API

百度地图Android平台API申请链接当前为<http://lbsyun.baidu.com/apiconsole/key>（`注：随时可能变动，已百度地图API首页最新链接为准`)

![API Key申请页面](http://dengbiao.github.io/assets/uploads/QQ20140314-1.png "APIkey申请页面")

####创建应用

进入API链接申请页面后，点击创建应用：

![创建应用](http://dengbiao.github.io/assets/uploads/QQ20140314-2.png "创建应用")

应用名称随便填写，应用类型选`for mobile`, 禁用服务不用选择，关键问题就是安全码。如图所示，安全码为`Android签名证书的sha1值+“;”+packagename，即数字签名+分号+报名`组成。我在这一步根据官网提供的两种获得安全码的办法，生成了自己的安全码，可是最后key认证失败了。
其实我感觉官网写的不是很清楚，当然也是因为自己对keystore机制不是很了解。这里具体说明一下。

官网提供的两种方法都是针对系统默认的keystore来生成安全码的，那么当联机调试的时候，eclipse也是用默认的keystore来生成的应用，所以，key认证没有遇到问题，当用eclipse导出APK时，eclipse会询问使用现有的keystore还是重新生成一个keystore。我选择的是生成了一个新的keystore。结果这个keystore和前面获取sha1值使用的keysotre不是同一个，直接导致了百度地图key认证失败。

所以，当发现问题之后，我的尝试了在eclipse导入我自己的keystore，但是没有成功，最后选用的办法是申请两个APIkey，分别与eclipse默认的keystore和自己生成的keystore对应，开发的时候使用默认keystore，当需要导出的时候，将代码中的key替换成与自己生成的keystore对应的key。

####key的获取

讲了很多废话，回头才发现没有讲具体怎么获取key。这里只讲一种简单的用eclipse获取sha1值与packageName值生成key的办法。至于那些使用keytool的程序员们，`keytool -list -v -keysotre your_keystore_file -storepass your_keystore_passwordl`,你们应该懂得，就不多说了。

#####1.获取keysotre的sha1值

我使用的版本mac下的eclipse，查看方法是依次进入如下路径： `ADT->Prefrence->Android->Build`,windows版本eclipse进入路径：`winows -> preferance -> android -> build`,然后在设置面板的右边可以看到sha1值的具体信息，如下图。

![eclipse查看keystore sha1](http://dengbiao.github.io/assets/uploads/QQ20140314-3.png "eclipse查看keystore sha1值")

#####2.获取PackageName

eclipse中，打开项目的AndroidMenifest.xml文件，头部的Manifest标签中的package对应的内容就是我们需要的，如下图

![eclipse查看packageName](http://dengbiao.github.io/assets/uploads/QQ20140314-4.png "eclipse查看packageName")

#####3.生成百度地图API开发者key

将上面获得的sha1值与packageName一起按格式`sha1;packageName`的形式填入到本文前面`创建应用`一节中图片中的安全码位置。然后点击确定，系统会提示创建成功，然后系统会为你的应用生成一个专门的字符串，即key，如下图所示。将其配置到自己的应用中使用即可。

![系统生成key](http://dengbiao.github.io/assets/uploads/QQ20140314-5.png "系统生成key")

###总结

到此，本文结束了，生命在于折腾。。。




