---
title: Canvas踩坑(2)马克笔效果
categories:
  - Web
tags:
  - javascript
  - canvas
abbrlink: 27f27d32
date: 2018-12-24 19:52:27
---
![Marker Pen](/images/2018/12/markerpen0.png)

画笔里面，马克笔算是比较简单，比较难的是铅笔和毛笔

一般的线是由点连起来的，设置好 `lineCap` 和 `lineJoin` 即可，但马克笔是由**块**连起来的

当我们拿到一个XY坐标点后，需进行扩大，使其变成一个矩形

![Marker Pen](/images/2018/12/markerpen1.png)

待更新