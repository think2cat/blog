---
title: Blue Blog开发历程
tags:
  - blog
id: 873
categories:
  - Web
abbrlink: 21ca53d9
date: 2006-03-20 23:41:03
---

![](/images/2006/03/20_2006-3-321494776_12719.gif)

应该没人记得这个Banner吧，这是最初的博客，程序写于03年9月。

当时只是几个简单的页面，自己可以发发日记，如此而已。

![](/images/2006/03/20_2006-3-321254079_12720.gif)

* 03年12月，把Blog跟论坛数据库整合一起，所有论坛注册用户均可发表日志。上面是当时的Banner。
<!--more-->


![](/images/2006/03/20_2006-3-321112864_12721.gif)

* 04年10月，开始学习网页标准，并应用到Blog中，CSS是通过W3C验证了，网页由于某些页面用了不符合XHTML的属性，没通过

* 05年12月，开始改版，开发真正支持多用户Blog

* 06年2月，终于放弃开发，采用Oblog系统架设Blue Blog

  目前已运行一个来月，问题多多，不知所谓的&ldquo;经过Oblog＆LeadBBS开发人员共同审核&rdquo;的说法是怎么来的，于是自己在改，凡事还是靠自己的好。
---

补充一下Oblog to LEADBBS Access版转SQL的修改方法

**oBlog to Leadbbs 3.1 Access changeTo MSSQL**

conn.asp 第10行
```
Const is_sqldata=0
```
改为
```
Const is_sqldata=1
```
在29行，自行配置数据库的连接参数

oBlogConn.asp 第4行
```
Const is_sqldata=0
```
改为
```
Const is_sqldata=1
```
同样在下面配置连接参数

inc/class_blog.asp 第673行
```
logdate = c_year & "-" & c_month
```
改为
```
logdate = c_year & "-" & c_month & "-" & c_day
```