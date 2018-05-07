---
title: 'FCKEditor出现类型不匹配: ''Server.HTMLEncode'' 解决方法'
tags:
  - asp
  - FCKEditor
  - 解决
id: 940
categories:
  - IT
date: 2007-05-25 15:18:29
---

程序如下：
<textarea name="code" cols="60" rows="6" class="vb">Dim oFCKeditorIntro
Set oFCKeditorIntro = New FCKeditor
oFCKeditorIntro.BasePath = "../FCKeditor/"

oFCKeditorIntro.ToolbarSet = "Default"
oFCKeditorIntro.Width = "100%"
oFCKeditorIntro.Height = "350"
oFCKeditorIntro.Value = rs("Info")

oFCKeditorIntro.Create "Info"</textarea>
无情滴出错了

<div style="border: 1px solid #000; margin: 0px; padding: 5px;width:500px;">Microsoft VBScript 运行时错误 错误 '800a000d' 

类型不匹配: 'Server.HTMLEncode' 

/fckeditor.asp，行97 </div>

网上搜了一下，大都说是 rs("Info")内容有问题，然后提供了类似防SQL注入的解决方法，把xxxx非法字符做过滤处理

自己试了下，解决方法很简单

<textarea name="code" cols="60" rows="6" class="vb">Dim oFCKeditorIntro,InfoText	'多定义个变量
InfoText = rs("Info")		'look
Set oFCKeditorIntro = New FCKeditor
oFCKeditorIntro.BasePath = "../FCKeditor/"

oFCKeditorIntro.ToolbarSet = "Default"
oFCKeditorIntro.Width = "100%"
oFCKeditorIntro.Height = "350"
oFCKeditorIntro.Value = InfoText		'look

oFCKeditorIntro.Create "Info"</textarea>