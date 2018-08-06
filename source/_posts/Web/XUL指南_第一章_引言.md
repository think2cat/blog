---
title: 《XUL指南》第一章：引言
tags:
  - xml
  - XUL
  - XUL指南
  - 笔记
id: 900
categories:
  - Web
abbrlink: c4d720b9
date: 2006-10-23 23:15:47
---

这是以前在wellhope维豪上班时翻译的文档，可惜没有翻译完就离开了

# XUL指南

原作者：[Neil Deakin](http://www.xulplanet.com/ndeakin/)

原文出处：[http://www.xulplanet.com/tutorials/](http://www.xulplanet.com/tutorials/)

原文最后更新日期：2006年2月19日(当地时间)

翻译：梦游的猫

译文最后更新日期：2006年4月10日（北京时间）

欢迎查看XUL指南，该指南讲述XUL&mdash;&mdash;XML的一种用户界面语言，该语言基于Mozilla框架，用于定义其用户界面。

该指南讲述的XUL基于Mozilla1.7和Firefox1.0

## 一、引言

## 1.1 引言

这份教程可以帮助你如何用 XUL (XML用户界面语言) 来创建跨平台的可描述界面的应用程序.

这份教程将设计出一个&quot;查找文件&quot;的应用程序界面, 类似于Macintosh的Sherlock或是Windows的文件查找的窗口. 只会提供基本的界面以及一些简单的功能函数. 真正的文件查找功能并没有实现,.

### 什么是XUL，及为什么要创建它？

XUL (发音是 zool,有点像cool) 是为开发更快更简单的Mozilla浏览器而开发的. 这是一个基于 XML的语言 ,所以XML的所有特性都适用于XUL.
<!--more-->
大多数应用程序在开发的时候都需要应用某个特定平台的特性，这就使得创建一个跨平台的软件变得费时费力。而这对于某些用户或许并非重要，但是对那些想在其它设备上，比如手持设备或机顶盒上，运行一个应用程序的用户来说却，跨平台确是至关重要的。

在过去诸多跨平台的解决方案都已被开发出来。比如Java的可移植性就是其主要卖点。XUL则是一个那样的语言，它被设计出用于创建可移植的用户界面。

创建一个应用程序是要花费好多时间的，哪怕它只工作在某个平台下。编译和排错需要的时间可能会很多。而使用XUL，一个界面可以被快速方便地制作和修改。

XUL具备所有其它XML语言的所有优点。举例来说，XHTML或者其它XML语言，比如MathML 或 SVG，可以被插入到XUL当中。同样，XUL中的文本也可以很容易地被本地化，那意味着它可以稍加努力就可以被翻译成其它国家的语言。样式表可以被提供用于修改用户界面的外观（就像WinAmp的主题和皮肤或者某些窗口管理器）。

### 用XUL可以做些什么类型的用户界面？

XUL有能力创建现代图形界面中的大多数元件。它的应用广泛到可以满足特定设备特殊要求，而它的强大足以使开发者创建出复杂的界面。

下列组件可以被创建：

*   输入组件，如文本框和复选框
*   带按钮和其它组件的工具条
*   带菜单和弹出菜单的菜单条
*   选项卡对话框
*   树形控件或栅格控件
*   快捷键
(本节以上部分出自MDC，网址[http://developer.mozilla.org/cn/docs/XUL_%E6%95%99%E7%A8%8B](http://developer.mozilla.org/cn/docs/XUL_%E6%95%99%E7%A8%8B))

显示的内容创建自XUL文件及数据源的数据，在Mozilla中，数据源包含用户邮件箱、书签、搜索结果等等，菜单、树状列表和其它元素都能从那添加，也可以从RDF文件添加自定义的数据。

创建XUL应用程序的方法有几种：

*   Firefox扩展：扩展可以增强浏览器的功能，一般出现在工具栏、菜单，或自定义的界面里面。这里使用一种在XUL里称为overlay的东西完成，可以被主界面所引用，在实际使用中，Firefox将从扩展里面引入相关界面。扩展也可以被其它基于Mozilla平台的产品所使用，例如Thunderbird。
*   基于XULRunner的程序：XULRunner是创建基于Mozilla平台XUL程序的一个独立程序，它有自己的可执行文件，因而不需要浏览器。
*   XUL包：有别与前面基于Firefox扩展及XULRunner，它更像是有独立运行窗口的独立程序，一般用于不想携带大体积的XULRunner程序，但仍然需要Mozilla浏览器来允许该包。
*   远程XUL程序：你可以把XUL代码放在Web服务器，然后在浏览器打开访问，但基于安全方面的原因，该方法有局限性。

前三种类型需要在用户电脑做安装操作，但是这样程序就没有安全限制了，可以访问本地文件和读写属性。例如扩展，所包含的XUL文件、脚本和图像将被打包成单一文件，用户下载后安装到本地。基于Mozilla的程序比如Firefox，提供了扩展管理器来对包进行安装管理，而不需要很多代码操作。

XUL也可以从远程WEB站点打开，然而这样很多类型操作会有限制，以及一些XUL外观效果会失效。如果你需要从远程站点载入XUL内容，WEB服务器必须设置XUL文件头格式为'application/vnd.mozilla.xul+xml'。通常XUL文件使用.xul为扩展名，你可以从Mozilla浏览器打开它，跟打开其它文件一样，点击菜单【打开文件】或者直接在地址栏输入URL地址。

### 阅读该指南之前我需要了解什么

你需要理解HTML，以及了解XML和CSS一些基础知识，下面几点建议需要注意：

*   XUL标签和属性全部用小写，XML是区分大小写的(不像HTML)
*   XUL属性值都要写在引号内，即使是数字
*   XUL文件一般分割为四部分，一是布局和元素，二是样式描述，三是脚本，四是语言包，另外通常还会包括一些图片和数据文件

基于Mozilla和Gecko平台的程序都支持XUL，例如Mozilla Firefox和Netscape6。随着时间推移XUL在语法有些不同，你可能需要使用适当的版本来进行工作。在大多例子里使用的是Mozilla 1.0或更高版本。XUL在Firefox和其它基于Mozilla浏览器中是相似的，虽然也存在一些细微的差别，例如工具栏。

该指南将最大限度介绍XUL方方面面，然而还是无法讲述所有的特征。一旦你熟悉XUL，可以使用 [XUL Element Reference](http://www.xulplanet.com/references/elemref/) 来查找其它标签的属性特征。

## 1.2 XUL结构

首先我们来看下XUL在Mozilla是如何工作的。

### XUL处理流程

在Mozilla平台，XUL处理过程跟HTML相似，当我们在浏览器地址栏输入一个HTML网址的时候，浏览器先查找网址和下载内容，解析引擎会把内容解析成一棵对象树，然后再分割成各个对象进行解析最后显示在屏幕上。CSS、图片和其它技术用来控制表现形式。XUL也是这样处理的。

实际上，在Mozilla平台，不管HTML还是XUL，甚至是SVG，都是采用相关底层代码进行解析的。这意味着你可以使用CSS定义HTML和XUL的样式，它们有很多共同的属性，可是也有一些特性是HTML特有的，例如form，也有一些特性是XUL特有例如overlay。由于HTML和XUL是采用相同处理流程，所以你可以从本地、网页或扩展载入文件，也可以用XULRunner程序来运行。

即使HTML、XML、XUL文件有很多相似处，Mozilla还是用不同DOM对象进行处理，虽然XUL也是XML，但它们在功能上仍有些不同。例如在HTML页面用 document.forms 属性控制 form 元素，但在XUL是无效的因为没有forms，在文件头部XUL可以指定overlays作为模板使用，这也是XUL特有的。

要分清不同文档的不同之处，不然可能会经常把XUL特有的属性用在HT
ML或XML文档，毕竟不同特征需要用在合适的文档。还有你也可以使用XUL布局方式来编写其它文档，即使它们并不是XUL文档类型。

总结一下本节：

*   Mozilla用同一个解析引擎渲染HTML和XUL，以及用CSS指定样式
*   XUL可以从远程网站、本地、或者已经安装的包访问，用chrome路径进行访问，也就是浏览器扩展
*   Chrome访问包的形式可以获取更改的权力
*   HTML、XML和XUL是不同文件类型，有些相同特征可以公用，也有各自的私有特征。

接下来几节将讲述安装在Mozilla的包的结构，如果你只是想创建独立的应用程序，可以直接跳到『Creating a Window』，以后再看这些基础。

**该节以下部分由limodou****翻译，网址[http://blog.donews.com/limodou/archive/2005/01/08/230804.aspx](http://blog.donews.com/limodou/archive/2005/01/08/230804.aspx)**

### 包组织

由Mozilla提供的包可以在Mozilla的安装目录下的chrome子目录中找到。但仅仅把文件拷贝到chrome目录下是不会赋给它任何的权限，也不可以通过chrome URL来访问的，还需要配置才可以。需要按照chrome包格式的要求来组织才可以。

一个包的文件通常被组织为一个JAR文件。注意，很象Java中的Jar包，都是Zip格式的。但里面可不是Java程序。这也造成了某些人认为开发扩展需要懂Java，其实不是的。只是后缀与Java一样。(不清楚为什么用这个后缀)同时，也可以不打成一个JAR包，就是展开的目录也可以(如果你看到另一个教程XUL App Tutorial，你会看到就没有打成一个JAR包)。

通常一个chrome包有三个目录，但它们都是可选的，分别为：content、skin和locale。

* content -- 窗口和脚本
  用户界面的定义都放在XUL文件中，这些文件都以.xul为后缀。一个content目录下可以有多个XUL文件，但主窗体文件名必须与包名相同。例如editor包必须有一个叫做editor.xul的文件。其它的脚本文件(一般是.js文件，Javascript)也放在这个目录下。*   skin -- 样式表、图片，和其它与theme相关的文件
  样式表用来描述窗口显示的外观。它们与XUL分离，可以灵活地修改程序的外观。
* locale -- 与语言相关的文件
  所有界面中用到的文本都放在这里。如果支持多国语言，它们将按语言种类进行组织。只要在这里进行翻译即可实现多语言。

同时在每个目录下还应该有一个名为：contents.rdf的文件，它用来描述相关目录的信息清单。Mozilla将读取它，并使用它的内容来注册包，并给这个包分配一个chrome URL，这样这些文件就可以能过这个chrome URL来访问了。如果有没contents.rdf文件，这个包就不能被分配一个chrome URL，因此也就不能通过chrome来访问了。同时要注意，只能你需要通过chrome URL来访问的目录才需要contents.rdf文件，不需要访问的目录可能不要。再有，如果一个目录下还有子目录，但子目录通过与父目录相同的chrome URL来访问，子目录也可以不要contents.rdf文件。

在locale目录中，语言化文件有两种形式：DTD和属性(properties)文件。DTD(Document Type Definition，文档类型定义)是XML中常用的文件，它一般是用来定义XML中的元素规则，还可以用来定义XML中出现的实体(entity)元素。XUL要使用DTD中的实体声明，用来显示文本信息。因此如果想实现多语言，就要按语种创建子目录，生成多个DTD文件。属性文件是给脚本使用的。

Mozilla中的包通常是放在chrome目录下，但是你也可以不放在这个目录下，而是放在磁盘任意的地方。在chrome目录下的chrome.rdf文件保存了安装了的包、skin、locale的列表及它们的位置。放在chrome下是最常见的。对于存在多种skin和locale的情况，chrome.rdf中保存了激活的配置。

## 1.3 The Chrome URL

XUL文件可以象HTML URL一样被引用。但安装过的包还可以通过chrome URL来引用。安装过的包除了没有安全性的限制外，还可以自动的处理多种themes和locales。当使用chrome URL来访问theme和locale中的文件时，安装包不需要知道哪一个正在被使用，由Mozilla来定位文件并返回正确的数据。chrome URL与文件的物理定位是分离的，这样可以使应用简化，不用关心文件的定位这种细节。

chrome URL的基本语法为：
`chrome://<packagename>/<part>/<file.xul>`

package name是包名。part是象content，skin和locale的目录。

当打开一个chrome URL时，Mozilla会在它的安装包目录中进行查找，试图定位与package name和part相匹配的JAR包。一旦找到，它将在JAR包中查找file.xul文件。同时Mozilla还会在JAR包中同一个目录下查找contents.rdf文件。如果你把某个包放到别的地方，同时更新了Mozilla的注册chrome包列表，那么这个包的功能仍然可以正常使用。因为这些功能并不依赖于包的安装位置。

在任何URL可以使用的地方你都可以输入chrome URL。你可以直接在浏览器的URL工具条上输入chrome URL。

你还会看到没有指明文件名的chrome URL，例如：
`chrome://navigator/content`

在这种情况下，仅有包名和part被指明。在时，Mozilla将会从正确的目录下自动选择一种引用类型。对于content来说，将会选择与包名相同的一个.xul文件。上面的例子中，就会选择navigator.xul文件。对于skin来说，将会选择&lt;package name&gt;.css文件。对于locale来说，将会选择&lt;package name&gt;.dtd文件。
请记住，chrome URL与它在磁盘上的位置无关。前两段是包名和part(content或skin或locale)。尽管平时我们会把content文件放在一个名为'content'的目录下，但这纯粹是一种习惯，这些文件可以放在一种完全不同的结构中。唯一的规则就是：chrome URL中的文件名据在的目录下要有一个 contents.rdf 文件。