---
title: 让DIV不换行
tags:
  - css
  - html
id: 824
categories:
  - Web
date: 2004-09-11 02:47:00
---

一，单个DIV：
1.用nobr元素

<pre lang="html4strict"><html>
<head>
</head>
<body>
<div><nobr>不换行不换行<nobr></div>
</body>
</html></pre>
2.用nowrap元素

<pre lang="html4strict"><html>
<head>
</head>
<body>
<div nowrap>div不换行div不换行div不换行</div>
</body>
</html></pre>

二，两个DIV
1\. 用CSS里的float属性

<pre lang="html4strict"><html>
<head>
<style type="text/css">
div#row1 { float: left; color: blue; }
div#row2 { color: red }
</style>
</head>
<body>
<div id="row1">第一个div</div>
<div id="row2">第二个div不换行</div>
</body>
</html></pre>

2\. 用CSS里的diplay属性

<pre lang="html4strict"><html>
<head>
<title>CSS中的不换行</title>
<style type="text/css">
div#row1 { color: blue;display: inline }
div#row2 { color: red;display: inline }
</style>
</head>
<body>
<div id="row1">第一个div</div>
<div id="row2">第二个div不要换行</div>
</body>
</html></pre>