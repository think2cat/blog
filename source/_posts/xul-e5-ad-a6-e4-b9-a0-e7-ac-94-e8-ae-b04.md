---
title: XUL学习笔记(4)
tags:
  - XUL
id: 879
categories:
  - Web
date: 2006-03-30 21:26:22
---

笔记就先写到这，今天起开始翻译XUL Planet的《XUL指南》，哈

### 四、XUL常用元素

#### 4.1 创建窗体

当准备好开发工具之后，就可以开始写XUL了。先看一段XUL文件
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;?xml version="1.0" encoding="GB2312"?&gt;
&lt;?xml-stylesheet href=""chrome://chrome/skin" type="text/css"?&gt;
&lt;?xml-stylesheet href=""chrome://chrome/skin/contact.css" type="text/css"?&gt;
&lt;window
id="QQ_setup"
class="sb_faceplate"
title="QQ2006设置"
xmlns:html="http://www.w3.org/1999/xhtml"
xmlns="http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul"
&gt;</DIV>

解释一下

#### &nbsp;

#### 4.2 按钮 &lt;button&gt;
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;button id="button" label="Find" default="true"/&gt;</DIV>

按钮比较简单，label是显示的文字，如果有多种语言版本，就要在文件头载入DTD文件，然后用引用形式显示文字，后面将做介绍，default是布尔值，表示窗口打开时按回车所触发的按钮

#### &nbsp;

#### 4.3 文本标签&lt;label&gt;和&lt;description&gt;
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;label value=" DisplayName " class="label"/&gt;</DIV>

Value是显示文本，同时可以用class来为标签定义样式
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;description value=" DisplayName " class="label"/&gt;</DIV>

Description基本上跟label是相同的，比如上面2行代码显示效果是一样的，但description标签可以显示多行文字，例如
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;description&gt;
This is a multi-line description. 
It should wrap if there isn't enough room to put it in one line.
Let's put in another sentence for good measure.
&lt;/description&gt;</DIV>

跟html一样，多个空格将被当成一个空格，回车和’\n’也是不起作用的，只有当文本宽度超过上级元素围的宽度了，才会换行，但是也可以用width属性来定义宽度，例如
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;description style="max-width:20px;"&gt;
This is a multi-line description. 
It should wrap if there isn't enough room to put it in one line.
Let's put in another sentence for good measure.
&lt;/description&gt;</DIV>

例外，文本标签可以使用control属性来跟按钮、输入框之类的元素绑定，下面代码点击label后会触发button，accesskey是快捷键，用Alt+D即可使该label为当前焦点
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;label control="DisplayName" value=" DisplayName " accesskey="d" class="label"/&gt;
&lt;button id=" DisplayName " label="MyName"/&gt;</DIV>

#### &nbsp;

#### 4.4 图片&lt;image&gt;
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;image src=”snow.png” width=”20” height=”26”/&gt;</DIV>

Image标签跟html一样，不过这不是常用的作法。常用的作法是放在skin目录下，通过样式表来控制。例如：
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">&lt;image id="snow" /&gt;
同时在CSS里面指定属性
#snow{
height: 20px;
width: 26px;
background-image: url(images/snow.gif);
}</DIV>

当然，你也可以用这样显示图片，但我觉得这样并不灵活，有时会撑大图片，比较难控制
<DIV style="BORDER-RIGHT: #cccccc 1px solid; PADDING-RIGHT: 5px; BORDER-TOP: #cccccc 1px solid; PADDING-LEFT: 5px; BACKGROUND: #f3f3f3; PADDING-BOTTOM: 5px; MARGIN: 5px 20px; BORDER-LEFT: #cccccc 1px solid; PADDING-TOP: 5px; BORDER-BOTTOM: #cccccc 1px solid">#snow{
height: 20px;
width: 26px;
list-style-image: url(images/snow.gif);
}</DIV>