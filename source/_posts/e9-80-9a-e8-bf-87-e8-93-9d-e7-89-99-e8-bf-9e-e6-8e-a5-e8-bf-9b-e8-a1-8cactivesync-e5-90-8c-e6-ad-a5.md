---
title: 通过蓝牙连接进行ActiveSync同步
tags:
  - 同步
  - 多普达
  - mobile
  - 蓝牙
id: 925
categories:
  - Digital
abbrlink: a541da7e
date: 2007-03-15 15:35:30
---

蓝牙驱动&nbsp;&nbsp; BlueSoleil 1.6.1.4

同步软件&nbsp;&nbsp; Microsoft ActiveSync 4.5 中文版

手机&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 多普达575

操作系统&nbsp;&nbsp; Windows 2003 中文企业版 sp1

![](/images/2007/03/15_12773.gif)

1.打开 BlueSoleil ，打开菜单 &gt;&gt; 我的服务 &gt;&gt; 属性

![](/blog/upload/2007/3/200703151540390755.gif)

2.确认串口A服务A是打勾的，记下串口号，这里是 COM7，如图

![](/blog/upload/2007/3/200703151541334577.gif)

3.打开ActiveSync的连接属性，在【允许连接到以下其中一个端口】打勾，并选中刚刚记下的串口号，【允许在设备上建立无线连接】也打勾

![](/blog/upload/2007/3/200703151547001834.gif)

4.在手机上打开蓝牙设备，然后搜索，搜到之后建立连接，记住是从手机连接电脑，不是从电脑连接手机

![](/blog/upload/2007/3/200703151548058134.gif)

5.建立好连接之后BlueSoleil窗口如图所示

![](/blog/upload/2007/3/200703151548381320.gif)

6.在手机上打开ActiveSync（在附件里面），然后点菜单【通过蓝牙连接】，连接成功后如图

此时就可以进行同步了，但要在手机上网，还需如下设置

![](/blog/upload/2007/3/200703151552295814.gif)

7.打开设置里面的【数据连接】，其中 Internet连接改为自动

![](/blog/upload/2007/3/200703151553388337.gif)

8.然后在电脑上，所使用的网络连接的属性页里，【Internet连接共享】打勾，完成了

![](/images/2007/03/15_200703151556057718_12749.gif)

手机里IE上雪猫博客，和MSN的截图，哇哈哈