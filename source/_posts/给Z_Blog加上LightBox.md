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

很常见的一个效果，像本站的 [上海之行](/blog/post/166.html) 和 [深圳欢乐谷](/blog/post/167.html) 都是使用这种效果

Z-Blog 1.7 Laputa Build 70216

[Lightbox 2.02 官方网](http://www.huddletogether.com/projects/lightbox2/)

1.  下载下面文件，其实跟官方一样，只是针对Z-Blog的JS和图片文件存放位置修改了几个路径，直接解压到Z-Blog目录即可

        ![](/image/common/download.gif)[200703232142548004.rar](/blog/upload/2007/3/200703232142548004.rar)
2.  在模板文件，例如single.html、default.html、catalog.html，加入以下代码

        <textarea class="xml" name="code" cols="100" rows="4">&lt;link rel=&quot;stylesheet&quot; href=&quot;&lt;#ZC_BLOG_HOST#&gt;css/lightbox.css&quot; type=&quot;text/css&quot; media=&quot;screen&quot; /&gt;&lt;script type="text/javascript" src="&lt;#ZC_BLOG_HOST#&gt;script/prototype.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="&lt;#ZC_BLOG_HOST#&gt;script/scriptaculous.js?load=effects"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="&lt;#ZC_BLOG_HOST#&gt;script/lightbox.js"&gt;&lt;/script&gt;</textarea>
3.  在要使用lightbox效果的图片加链接，格式化如下

        <font color="#ff0000">&lt;a href=&quot;http://www.21ido.com/blog/upload/2007/2/200702211425424414.JPG&quot; rel=&quot;lightbox&quot;&gt;点这里&lt;/a&gt;</font>
4.  如果有多张图片，要使用套图效果，类似 [上海之行](/blog/post/166.html) 的 Next / Prev，代码如下

        <textarea class="xml" name="code" cols="100" rows="4">&lt;a href="images/image-1.jpg" rel="lightbox[roadtrip]"&gt;image #1&lt;/a&gt;
&lt;a href="images/image-2.jpg" rel="lightbox[roadtrip]"&gt;image #2&lt;/a&gt;
&lt;a href="images/image-3.jpg" rel="lightbox[roadtrip]"&gt;image #3&lt;/a&gt;
</textarea>
5.  压缩包里面有2套图片，请自行选择