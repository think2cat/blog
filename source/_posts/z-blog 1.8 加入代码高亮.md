---
title: z-blog 1.8 加入代码高亮
tags:
  - ajax
  - highlight
  - html
  - javascript
  - zblog
  - 修改
id: 1033
categories:
  - Web
abbrlink: 87262eef
date: 2008-06-17 19:10:42
---

代码高亮，我所知道的方法有3种

1.  直接改模板，目前所使用的，好处就是无须修改源码，升级方便，缺点就是修改文章的时候，代码里的换行会被编辑器搞掉
2.  改FCKEditor，直接作为插件使用，用JS直接把高亮后的代码插入正文，优点是无需改模板，缺点是需要改FCKEditor，而且日后修改文章同样不方便
3.  新增UBB标签，例如[code=html]，需改源码和模板，优点是修改文章特方便，缺点是需要对程序大量修改，同样日后程序升级不方便

&nbsp;

这里说的是第一种方法

Z-blog 1.8

[syntaxhighlighter 1.5.1](http://code.google.com/p/syntaxhighlighter/) 下载后解压放好，我是分别把JS和CSS放到script和images目录

&nbsp;

以首页为例，打开default.html

在&lt;/head&gt;之前加入

<textarea name="code" class="xml" rows="15" cols="100"><link type="text/css" rel="stylesheet" href="<#ZC_BLOG_HOST#>image/styles/SyntaxHighlighter.css" />

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shCore.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushCpp.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushCss.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushJava.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushJScript.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushSql.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushVb.js"></script>

<script class="javascript" src="<#ZC_BLOG_HOST#>SCRIPT/Highlighter/shBrushXml.js"></script></textarea>

在文件最后一个 &lt;/script&gt;之前加入
<textarea name="code" class="js" rows="15" cols="100">dp.SyntaxHighlighter.ClipboardSwf = "/SCRIPT/Highlighter/clipboard.swf";

dp.SyntaxHighlighter.BloggerMode();

dp.SyntaxHighlighter.HighlightAll('code');</textarea>

&nbsp;

single.html 文件用上面方法同样处理，注意路径

使用方法还是一样
<textarea name="code" class="xml" rows="15" cols="100">&lt;textarea name=&quot;code&quot; class=&quot;java&quot; rows=&quot;15&quot; cols=&quot;100&quot;&gt;Java代码&lt;/textarea&gt;

&lt;textarea name=&quot;code&quot; class=&quot;vb:firstline[26]&quot; rows=&quot;15&quot; cols=&quot;100&quot;&gt;VB代码&lt;/textarea&gt;</textarea>