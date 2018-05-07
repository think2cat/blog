---
title: 给Z-Blog加上代码高亮显示
tags:
  - blog
  - css
  - javascript
  - 修改
  - 技巧
id: 926
categories:
  - Web
date: 2007-03-19 11:21:03
---

用的是dp.SyntaxHighlighter ，JS+CSS实现的，很不错的东东

**版本信息**
[dp.SyntaxHighlighter 1.4.1](http://www.dreamprojections.com/SyntaxHighlighter/)
Z-Blog 1.7 Laputa Build 70216
&nbsp;

1.  下载[dp.SyntaxHighlighter.rar](/blog/upload/2007/3/dp.SyntaxHighlighter.rar)，解压到Z-Blog根目录，每种语言都有单独JS文件，把需要用到的传到WEB空间即可
2.  打开z-Blog模板文件\TEMPLATE\single.html，在&lt;head&gt;标签里加
    <font color="#ff0000">&lt;link type=&quot;text/css&quot; rel=&quot;stylesheet&quot; href=&quot;&lt;#ZC_BLOG_HOST#&gt;css/SyntaxHighlighter.css&quot;&gt;&lt;/link&gt;</font>
    如果首页等其它页面也要实现语法高亮显示，在相应模板重复2、3步骤
3.  在&lt;/body&gt;前面加下面代码，根据自己需要选择性的加
    <textarea name="code" cols="100" rows="15" class="xml"><script class="javascript" src="/SCRIPT/shCore.js"></script>
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
<script class="javascript">dp.SyntaxHighlighter.HighlightAll('code');</script></textarea>
4.  打开/function/c_system_event.asp，266行<textarea name="code" cols="100" rows="15" class="javascript">Case "fckeditor"
	objArticle.Content=Request.Form("txaContent")
	'objArticle.Content=Replace(objArticle.Content,vbCrLf,"")
	'objArticle.Content=Replace(objArticle.Content,vbLf,"")
	If objArticle.Intro="" Then
		s=objArticle.Content
		s=TransferHTML(s,"[nohtml]")
		s=Left(s,ZC_TB_EXCERPT_MAX) & "..."
		objArticle.Intro=s
	End If</textarea>
5.  打开/function/c_system_lib.asp，380行最后的**&quot;[html-japan][vbCrlf][upload]&quot;**，去掉<font color="#ff0000">[vbCrlf]</font>，改后代码如下
    <textarea name="code" cols="100" rows="15" class="vb:firstline[379]">Public Property Get HtmlContent
	HtmlContent=TransferHTML(UBBCode(Content,"[face][link][autolink][font][code][image][typeset][media][flash=500, 400, true][key]"),"[html-japan][upload]")
End Property</textarea>
6.  使用加亮实例
    <textarea name="code" cols="100" rows="15" class="xml">&lt;textarea name=&quot;code&quot; class=&quot;java&quot; rows=&quot;15&quot; cols=&quot;100&quot;&gt;Java代码&lt;/textarea&gt;
&lt;textarea name=&quot;code&quot; class=&quot;vb:firstline[26]&quot; rows=&quot;15&quot; cols=&quot;100&quot;&gt;VB代码&lt;/textarea&gt;</textarea>