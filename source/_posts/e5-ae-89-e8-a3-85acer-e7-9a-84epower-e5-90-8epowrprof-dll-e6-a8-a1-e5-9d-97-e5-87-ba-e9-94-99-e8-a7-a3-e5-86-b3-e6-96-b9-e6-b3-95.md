---
title: 安装Acer的ePower后powrprof.dll模块出错解决方法
tags:
  - Acer
  - epower
  - 宏碁
  - 模块
  - 电源
id: 909
categories:
  - IT
abbrlink: 56a5ea23
date: 2007-01-22 09:33:31
---

![无法定位序数24于动态链接库powrerof.dll](/images/2007/01/22_200701272035453083_12740.jpg)

Acer的蓝牙模块，需要装个ePower的软件进行开启，这还真是件麻烦事，且不说占了20多M空间和50M内存（ePower是建立在Acer Empowering Framework之上的，这家伙一启动就吃了32M内存），而且目前没有在Windows2003下使用的版本，XP下倒是没问题，2003下启动肯定会报错，虽然还能凑合用，但无法对电源管理进行设置，而且使用过程中偶尔也会报错而终止。

上周下了个新版的ePower，其实也不新，06年10月30日发布的，2.0.2028版For Win2000/XP，在Windows2003下安装时提示只能在XP下安装，于是把兼容性改为XP，顺利完成安装，启动也没报错，电源管理也能用了，以为事情解决，可是重启电脑后又报错了，还是那句&ldquo;<font face="Arial">无法定位序数24于动态链接库powrerof.dll</font>&rdquo;，真是悲哀，把ePower所有可执行文件兼容性都改为XP，还是不行，网上搜了一下也没找到解决办法。

然后想到能否用XP的电源模块来代替呢，在朋友机子拷了个powrprof.dll过来，然后用深山红叶启动进行替换，忐忑不安的重启后，居然可以了，经过一周试用，一切正常，哇哈哈~~~

<table width="475" height="106" cellspacing="1" cellpadding="1" border="1" summary="">    <caption>powrprof.dll的比较</caption>
    <tbody>        <tr>            <td>&nbsp;</td>            <td>WindowsXP</td>            <td>Windows2003</td>        </tr>        <tr>            <td>版本号</td>            <td><font face="Arial">6.00.2900.2180

            (xpsp_sp2_rtm.040803-2158)</font></td>            <td><font face="Arial">6.00.3790.1830

            (srv03_sp1_rtm.050324-1447)</font></td>        </tr>        <tr>            <td>文件大小</td>            <td><font face="Arial">17.0 KB (17,408 字节)</font></td>            <td><font face="Arial">16.5 KB (16,896 字节</font></td>        </tr>    </tbody></table>