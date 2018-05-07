---
title: Weekday()
tags:
  - asp
  - blog
id: 821
categories:
  - Web
date: 2004-09-03 01:57:00
---

今晚在重新写月历代码，没有了表格的&lt;TR&gt;&lt;TD&gt;似乎变简单了，反正调整好宽度，然后一直response.write就行了，会自动换行，嘿嘿![](/images/2004/09/03_12739.gif)
之后在写Weekday()发现week老是对不上，用的函数是weekday(date())，日期格式明明是mm/dd/yyyy，可是却不行，然后用1-7替换day，发现weekday()返回的数是没有规律的，便用了yyyy-mm-dd格式，还是不行，最后试了mm/dd/yy，居然行了，太麻烦了，正确的函数
<span style="COLOR: red">weeks = Weekday(Month() &amp; &quot;/&quot; &amp; Day() &amp; &quot;/&quot; &amp; Right(Year(),2))</span>
还有个问题，用Monthname()返回的是月份的中文名称，比如&ldquo;2004年九月&rdquo;，这样看起来怪怪的，或许是因为我系统用的是中文版吧，如果服务器也是中文版那要输出&ldquo;September 2004&rdquo;就要再多建个数组了，真是麻烦![](/images/2003/10/em002.gif)
明天是周五了，要是工作还没有消息，我会疯掉的![](/images/2003/12/13_12730.gif)

ps:[《网站重构》勘误表](http://www.w3cn.org/dwws/2004/71.html)