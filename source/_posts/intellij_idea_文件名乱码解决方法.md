---
title: IntelliJ IDEA 文件名乱码解决方法
tags:
  - java
id: 915
categories:
  - Web
abbrlink: d684af54
date: 2007-02-03 23:55:16
---

![](/images/2007/02/03_200702040012440565_12745.jpg)

今天打开IDEA6，不知怎么文件名都变成方块□□，原以为字体设置问题，选了宋体还是一样，后来问了同事，原来是样式造成的，打开Setting，在Appearance类找Look and feel，改为Windows就可以了，其它某个也可以正常显示中文，例如Metal，自己试试吧。

另外在1280分辨率下（例如我本就是宽屏的），编辑器右边会有条线，类似打印线，同事伟哥说那是提醒你代码别太长，哈，也有点道理，不过我看着碍眼，去掉好了，在Editor类，Right Margin前面勾去掉就好了