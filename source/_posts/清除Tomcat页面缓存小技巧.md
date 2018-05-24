---
title: 清除Tomcat页面缓存小技巧
tags:
  - java
  - tomcat
id: 936
categories:
  - Web
abbrlink: ebcd1bf6
date: 2007-04-05 22:14:52
---

相信很多用Tomcat的人都会重复这样的事

1.  关闭Tomcat
2.  打开tomcat\work目录
3.  删除Catalina目录
4.  运行Tomcat
其实可以用RD命令删除整个目录，只需打开 tomcat\bin\startup.bat 文件，在开头加入

```sh
rd/s/q "D:\Program Files\tomcat-5.5.9\work\Catalina"
```

路径根据自己实际情况做修改

这样每次启动Tomcat的时候就会先删除页面缓存了，不用担心页面修改后没被重新编译