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
<!--more-->
![Marker Pen](/images/2018/12/markerpen1.png)

每个点转成矩形后，会形成这样的效果

![Marker Pen](/images/2018/12/markerpen2.png)

所以还需要依次将各个矩形连起来

![Marker Pen](/images/2018/12/markerpen3.png)

连起来需要6个点，这6个点还需要根据2个坐标点的相对位置来判断
如果是上下或左右的移动，则只需4个点

将每2个坐标点连起来后是这样

![Marker Pen](/images/2018/12/markerpen4.png)

然后 `fill()` 进行填充

![Marker Pen](/images/2018/12/markerpen5.png)

然后我就懵逼

![Marker Pen](/images/2018/12/markerpen6.png)

放大看，会发现有些重叠的区域并没有被填充

没有填充是因为canvas把那些区域判断为线外区域，这涉及到nonzero和evenodd填充规则，可以看下 张鑫旭的这篇 [《搞懂SVG/Canvas中nonzero和evenodd填充规则》](https://www.zhangxinxu.com/wordpress/2018/10/nonzero-evenodd-fill-mode-rule/)

简单说就是连点方式顺时针和逆时针的差异，导致了填充时把内部区域判断为外部区域

测试发现只有在点往右下方画的时候会出现这个问题，于是单独把往右下角的连线方式改为逆时针，问题就解决了
比如已上图为例，默认连线是 A1 -> B1 -> B2 -> C2 -> D2 -> D1 -> A1
则逆时针反过来连，这跟交叉线夹角有关