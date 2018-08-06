---
title: XUL学习笔记(2)
tags:
  - XUL
id: 876
categories:
  - Web
abbrlink: 697236ca
date: 2006-03-23 22:20:59
---

#### 1.3 XUL可以做什么

先补充一下，XUL发音为：zool
XUL可以创建图形界面的绝大多数元素，像Firefox、Thunderbird界面都是XUL的应用。

#### 1.4 XUL相关资料

目前XUL应用并不多，相关资料(无论中文或英文)非常的少，大多仅有一些简单的介绍而已，下面是几个相关网站
[http://www.xulplanet.com](http://www.xulplanet.com/)
[http://developer.mozilla.com](http://developer.mozilla.com/)

### 二、开发平台

#### 2.1 开发工具

以下为扩展开发所需工具，可以在下面的两上网站下载和获取到帮助信息：
[https://addons.mozilla.org/extensions/?application=firefox](https://addons.mozilla.org/extensions/?application=firefox)
[http://www.extensionsmirror.nl/](http://www.extensionsmirror.nl/)

#### 2.2 配置环境变量
<!--more-->
Firefox 是一个允许创建多个 Profile 的浏览器，不同的 Profile 被分别进行管理。由于在开发扩展时，我们希望一个 Profile 用来做开发，一个 Profile 用来测试扩展的发布版本。所以，我们要配置不同的 Profile 来满足这种要求。
操作系统 Proile 对应的目录
Windows 9x/Me C:\Windows\Application Data\Mozilla\Firefox\Profiles\&lt;Profile name&gt;\
Windows 9x/Me, alternate C:\Windows\Profiles\&lt;Windows login/user name&gt;\Application Data\Mozilla\Firefox\Profiles\&lt;Profile name&gt;\
Windows NT 4.x C:\Winnt\Profiles\&lt;Windows login/user name&gt;\Application Data\Mozilla\Firefox\Profiles\&lt;Profile name&gt;\
Windows 2000/XP C:\Documents and Settings\&lt;Windows login/user name&gt;\Application Data\Mozilla\Firefox\Profiles\&lt;Profile name&gt;\ 或
%APPDATA%\Mozilla\Firefox\Profiles\&lt;Profile name&gt;\
Unix ~/.mozilla/firefox/&lt;Profile name&gt;/
Mac OS X ~/Library/Mozilla/Firefox/Profiles/&lt;Profile name&gt;/ 或
~/Library/Application Support/Firefox/Profiles/&lt;Profile name&gt;/

一般把默认的 Profile 做为开发的 Profile，所以，我们不用做什么特殊的设置。你可以把上面提到的那些扩展，全部安装到此 Profile 下。
那么，我们还要创建一个新的 Profile 来完成扩展的安装，或者说是发布测试工作。通过下面的两个步骤，你可以实现这种需求。

首先，我们需要在命令行方式下，定位到 Firefox 的安装目录，通过运行
firefox -P
启动 Firefox 的 Profile 管理器窗口，建立一个负责测试安装的 Profile。当然，也可以建立更多的 Profile。
假设你建立了一个名为“test”的 Profile，想启用这个 Profile，你需要通过
set MOZ_NO_REMOTE=1
firefox -P test
这两行命令来启用这个非默认启动状态的 Profile。为了方便测试，这两条命令可以写成一个 Shell 脚本或做成一个批处理文件。
下面列出Profile几个需要更改的设置项。

通过在地址栏中键入 about:config，你可以打开 Mozilla 内置的配置界面，这样你就可以更改那些设置项的值。在更改完这些设置项之后，我们会在以后的开发中更加得心应手。