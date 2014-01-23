---
layout: post
title: 项目的一些文档说明
categories: [tc, documents]
---

nfs目录: `/home/csumt/nfsroot/android/` 


光盘上的系统nfs目录: `/home/csumt/nfsroot/android2/`


android源码目录: `/home/csumt/soft/pandaboard/sourcecode/android/` 


模块编译方法: 进入到android源码目录(`cd /home/csumt/soft/pandaboard/sourcecode/android/`),执行 `source ./build/envsetup.sh`命令和`source ./build/myBuild.sh`)命令,
    然后进入到具体的模块目录(`cd /home/csumt/soft/pandaboard/sourcecode/android/frameworks/base/services/java/com/android/server/pm/`) 直接运行 `mm`命令即可.

