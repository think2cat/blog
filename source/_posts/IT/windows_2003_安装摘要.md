---
title: Windows 2003 安装摘要
tags:
  - win2003
  - 安装
  - 笔记
id: 870
categories:
  - IT
abbrlink: fc4b2fc
date: 2006-03-14 23:31:46
---

![](/images/2006/03/14_2006-3-315853782_12716.gif)

一直用Windows2000的，这几天不晓得搞了什么东东，全部FTP都连不上，无奈之下只好重装，正好前几天刻了张Windows2003，记录下大致安装步骤。

1.  准备好驱动程序，防火墙、防毒软件，以及系统补丁，例如SP1
2.  备份文件，例如【我的文档】、【收藏夹】、【Outlook】、输入法词库&hellip;&hellip;
3.  网络中断，安装Windows2003
4.  安装补丁（安装盘已经集成SP1，这步跳过）、驱动程序、防火墙和防毒软件（如果是服务器，请屏蔽掉端口，留几个Web、FTP、SQL、远程控制端口即可）
5.  连接网络，Update系统，同时更新防火墙和防毒软件
6.  安装应用程序
  Office、MSSQL、Macroeedia、Adobe之类，安装完后记得Update
7.  我习惯把我的文档、收藏夹、[Quick Launch]之类目录放到D盘，这样一来C盘报废也不怕
  修改注册表 AppData/Favorites/Personal，或者用超级兔子也可以修改这些系统文件夹
  大致在下面几个地方，最好F3查一下
  HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
  HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
  HKEY_USERS\S-1-5-21-3825453747-3422435210-596121274-500\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
  HKEY_USERS\S-1-5-21-3825453747-3422435210-596121274-500\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders

  修改AppData的话还要修改下面几处
  HKEY_CURRENT_USER\Volatile Environment
  HKEY_USERS\S-1-5-21-3825453747-3422435210-596121274-500\Volatile Environment
8.  配置IIS和MSSQL
  Windows2003要将IIS【Web服务扩展】里面[Active Server Pages]设为允许，同时站点属性 &gt;&gt; 主目录 &gt;&gt; 配置 &gt;&gt; 选项 &gt;&gt; 启用父路径
9.  配置各软件，根据使用习惯配置系统
10.  清除临时文件，整理C盘，克隆一份

PS：安装Flash8时遇到&ldquo;ISSCRIPT.MSI&rdquo;错误，正好论坛有篇帖子可以解决。~~[装Macromedia 8 套件，出现&ldquo;ISSCRIPT.MSI&rdquo;错误的完美解决办法](/bbs/a/a.asp?BoardID=350&amp;ID=11033&amp;q=1&amp;r=453)~~

之前用Mcafee的防火墙，到了Windows2003就装不了，郁闷ing...