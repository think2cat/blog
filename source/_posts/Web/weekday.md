---
title: Weekday()
tags:
  - asp
  - blog
id: 821
categories:
  - Web
abbrlink: 786b2ee7
date: 2004-09-03 01:57:00
---
今晚在重新写月历代码，没有了表格的&lt;TR&gt;&lt;TD&gt;似乎变简单了，反正调整好宽度，然后一直`response.write`就行了，会自动换行，嘿嘿

之后在写`Weekday()`发现week老是对不上，用的函数是`weekday(date())`，日期格式明明是mm/dd/yyyy，可是却不行，然后用1-7替换day，发现weekday()返回的数是没有规律的，便用了yyyy-mm-dd格式，还是不行，最后试了mm/dd/yy，居然行了，太麻烦了，正确的函数
```vbs
weeks = Weekday(Month() & "/" & Day() & "/" & Right(Year(),2))
```

还有个问题，用`Monthname()`返回的是月份的中文名称，比如"2004年九月"，这样看起来怪怪的，或许是因为我系统用的是中文版吧，如果服务器也是中文版那要输出"September 2004"就要再多建个数组了，真是麻烦
明天是周五了，要是工作还没有消息，我会疯掉的

ps:[《网站重构》勘误表](http://www.w3cn.org/dwws/2004/71.html)