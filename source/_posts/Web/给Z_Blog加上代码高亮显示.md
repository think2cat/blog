---
title: 给Z-Blog加上代码高亮显示
tags:
  - blog
  - css
  - javascript
id: 926
categories:
  - Web
abbrlink: d4672af4
date: 2007-03-19 11:21:03
---

用的是dp.SyntaxHighlighter ，JS+CSS实现的，很不错的东东

**版本信息**
[dp.SyntaxHighlighter 1.4.1](http://www.dreamprojections.com/SyntaxHighlighter/)
Z-Blog 1.7 Laputa Build 70216


1.  下载[dp.SyntaxHighlighter.rar](/blog/upload/2007/3/dp.SyntaxHighlighter.rar)，解压到Z-Blog根目录，每种语言都有单独JS文件，把需要用到的传到WEB空间即可
2.  打开z-Blog模板文件\TEMPLATE\single.html，在 `<head>` 标签里加
  `<link type="text/css" rel="stylesheet" href="<#ZC_BLOG_HOST#>css/SyntaxHighlighter.css"></link>``
  如果首页等其它页面也要实现语法高亮显示，在相应模板重复2、3步骤
3. 在 `</body>` 前面加下面代码，根据自己需要选择性的加
  ```html
  <script class="javascript" src="/SCRIPT/shCore.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushCSharp.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushPhp.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushJScript.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushJava.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushVb.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushSql.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushXml.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushDelphi.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushPython.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushRuby.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushCss.js"></script>
  <script class="javascript" src="/SCRIPT/shBrushCpp.js"></script>
  <script class="javascript">dp.SyntaxHighlighter.HighlightAll('code');</script>
  ```<!--more-->
4.  打开/function/c_system_event.asp，266行
  ```js
  Case "fckeditor"
	objArticle.Content=Request.Form("txaContent")
	'objArticle.Content=Replace(objArticle.Content,vbCrLf,"")
	'objArticle.Content=Replace(objArticle.Content,vbLf,"")
	If objArticle.Intro="" Then
		s=objArticle.Content
		s=TransferHTML(s,"[nohtml]")
		s=Left(s,ZC_TB_EXCERPT_MAX) & "..."
		objArticle.Intro=s
	End If
  ```
5.  打开/function/c_system_lib.asp，380行最后的 `[html-japan][vbCrlf][upload]` ，去掉 `[vbCrlf]` ，改后代码如下
  ```vbs
  Public Property Get HtmlContent
    HtmlContent=TransferHTML(UBBCode(Content,"[face][link][autolink][font][code][image][typeset][media][flash=500, 400, true][key]"),"[html-japan][upload]")
  End Property
  ```
6. 使用加亮实例
  ```html
  <textarea name="code" class="java" rows="15" cols="100">Java代码</textarea>
  <textarea name="code" class="vb:firstline[26]" rows="15" cols="100">VB代</textarea>