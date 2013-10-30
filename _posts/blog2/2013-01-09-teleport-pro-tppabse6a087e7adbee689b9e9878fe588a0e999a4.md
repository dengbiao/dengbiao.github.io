---
author: dengbiao
comments: true
date: 2013-01-09 07:21:13+00:00
layout: post
title: teleport pro tppabs标签批量删除
categories:
- Accumulation
---
使用Teleport Pro下载的网页代码中包含了很多垃圾代码，比如下载的html网页代码中会出现tppabs标签，而且还会将所有的href标签中加入了很多垃圾代码，在css会加入了tpa标签，这些都是冗余代码，可以将其全部删除，但是由于代码太多，我们不可能一个个删除，因此可以使用Dreamweaver的查找/替换工具中的正则表达式来进行替换。  

    1. 替换tppabs标签，使用Dreamweaver查找\btppabs="h[^"]*"，将其替换为空即可。  
    2. 替换href中的多余代码，使用Dreamweaver查找href="javascript:if\(confirm\('htt[^"]*"替换为href=""即可。  
    3. 替换css文件中的tpa标签，使用Dreamweaver查找/\*tpa=.*\*/替换为空即可。



