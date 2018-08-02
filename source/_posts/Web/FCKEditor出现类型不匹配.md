---
title: 'FCKEditor出现类型不匹配: ''Server.HTMLEncode'' 解决方法'
tags:
  - asp
id: 940
categories:
  - Web
abbrlink: f1a2a654
date: 2007-05-25 15:18:29
---

程序如下：
```vbs
Dim oFCKeditorIntro
Set oFCKeditorIntro = New FCKeditor
oFCKeditorIntro.BasePath = "../FCKeditor/"

oFCKeditorIntro.ToolbarSet = "Default"
oFCKeditorIntro.Width = "100%"
oFCKeditorIntro.Height = "350"
oFCKeditorIntro.Value = rs("Info")

oFCKeditorIntro.Create "Info"
```
无情滴出错了

> Microsoft VBScript 运行时错误 错误 '800a000d'
> 类型不匹配: 'Server.HTMLEncode'
> /fckeditor.asp，行97
<!--more-->
网上搜了一下，大都说是 rs("Info")内容有问题，然后提供了类似防SQL注入的解决方法，把xxxx非法字符做过滤处理

自己试了下，解决方法很简单

```vbs
Dim oFCKeditorIntro,InfoText	'多定义个变量
InfoText = rs("Info")		'look
Set oFCKeditorIntro = New FCKeditor
oFCKeditorIntro.BasePath = "../FCKeditor/"

oFCKeditorIntro.ToolbarSet = "Default"
oFCKeditorIntro.Width = "100%"
oFCKeditorIntro.Height = "350"
oFCKeditorIntro.Value = InfoText		'look

oFCKeditorIntro.Create "Info"
```