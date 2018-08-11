---
title: 给Z-Blog加上LightBox
tags:
  - blog
  - javascript
  - ligntbox
id: 930
categories:
  - Web
abbrlink: e6f81019
date: 2007-03-23 21:39:49
---

很常见的一个效果，像本站的 {% post_link '上海之行的PP' %} 和 ~~[深圳欢乐谷](/blog/post/167.html)~~ 都是使用这种效果

Z-Blog 1.7 Laputa Build 70216

[Lightbox 2.02 官方网](http://www.huddletogether.com/projects/lightbox2/)

1. 下载下面文件，其实跟官方一样，只是针对Z-Blog的JS和图片文件存放位置修改了几个路径，直接解压到Z-Blog目录即可 ~~[200703232142548004.rar](/blog/upload/2007/3/200703232142548004.rar)~~
2. 在模板文件，例如single.html、default.html、catalog.html，加入以下代码
  ```html
  <link rel="stylesheet" href="<#ZC_BLOG_HOST#>css/lightbox.css" type="text/css" media="screen" /><script type="text/javascript" src="<#ZC_BLOG_HOST#>script/prototype.js"></script>
  <script type="text/javascript" src="<#ZC_BLOG_HOST#>script/scriptaculous.js?load=effects"></script>
  <script type="text/javascript" src="<#ZC_BLOG_HOST#>script/lightbox.js"></script>
  ```
3. 在要使用lightbox效果的图片加链接，格式化如下 `<a href="/blog/upload/2007/2/200702211425424414.JPG" rel="lightbox">点这里</a>`
4.  如果有多张图片，要使用套图效果，类似 {% post_link '上海之行的PP' %} 的 Next / Prev，代码如下
  ```html
  <a href="images/image-1.jpg" rel="lightbox[roadtrip]">image #1</a>
  <a href="images/image-2.jpg" rel="lightbox[roadtrip]">image #2</a>
  <a href="images/image-3.jpg" rel="lightbox[roadtrip]">image #3</a>
  ```
5. 压缩包里面有2套图片，请自行选择