---
title: 安装ZoneAlarm提示无法找到zpeng24.dll解决方法
tags:
  - try
  - zonealarm
  - 方法
id: 928
categories:
  - IT
date: 2007-03-21 17:27:03
---

一直用ZoneAlarm 6.5，今天程序提示升级，上[官网](http://www.zonelabs.com/)下了个7.0.337，经过各种尝试，蓝屏N次

1.  从ZoneAlarm 6.5升级安装7.0
2.  完全安装
3.  禁用卡巴KAV的主动防御后安装
4.  卸除KAV后安装系统是Windows2003+sp1，每次都是在进度条滚动那个启动画面不久后就蓝屏，然后自动重启，蓝屏一闪而过，没法看到错误信息，只能进入安全模式，卸除后才能正常启动

无奈只好装回6.5，没想到出现了以下错误提示，【vsmon.exe无法找到组件】：没有找到zpeng24.dll

![](/images/2007/03/21_200703211727103847_12750.gif)

解决方法是删除 \WINDOWS\system32下面的ZoneLabs目录，重启后就能再次安装了

顺便说下网上好多人说KAV和ZoneAlarm冲突，我不确定他们是什么系统，但是我用了多年这种搭配了，在2000/XP/2003都用过，不存在什么冲突