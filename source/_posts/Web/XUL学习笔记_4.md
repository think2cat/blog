---
title: XUL学习笔记(4)
tags:
  - XUL
id: 879
categories:
  - Web
abbrlink: 3f28914c
date: 2006-03-30 21:26:22
---

笔记就先写到这，今天起开始翻译XUL Planet的《XUL指南》，哈

### 四、XUL常用元素

#### 4.1 创建窗体

当准备好开发工具之后，就可以开始写XUL了。先看一段XUL文件
```xml
<?xml version="1.0" encoding="GB2312"?>
<?xml-stylesheet href=""chrome://chrome/skin" type="text/css"?>
<?xml-stylesheet href=""chrome://chrome/skin/contact.css" type="text/css"?>
<window
  id="QQ_setup"
  class="sb_faceplate"
  title="QQ2006设置"
  xmlns:html="http://www.w3.org/1999/xhtml"
  xmlns="http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul"
 >
 ```
<!--more-->
解释一下
1. `<?xml version="1.0" encoding="GB2312"?>`
  XUL也是XML文件，每个XML都必须有一个声明，这里声明了版本和编码
2. `<?xml-stylesheet href=""chrome://chrome/skin" type="text/css"?>`
  `<?xml-stylesheet href=""chrome://chrome/skin/contact.css" type="text/css"?>`
  指明样式表所在位置，chrome是Mozilla一种协议，就像http一样，只有当包被注册到Mozilla后才可以通过chrome访问资源，所以在测试XUL时，可以用相对或绝对路径，比如
  `<?xml-stylesheet href="../skin/contact.css" type="text/css"?>`
  作用是一样的
3. `<window` 每个文件只能有一个window标签，相当于html的body
  `id="QQ_setup"` 用于唯一识别元素，可以为JS所调用
  `class="sb_faceplate"` 引用的样式表类名
  `title="QQ2006设置"` 窗体标题

```
xmlns:html=http://www.w3.org/1999/xhtml
xmlns=http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul
```
引入XUL所用的名字空间。照抄就行。

除了上面，还有其它属性，常用的有
* `hidechrome="true"`		窗口标题栏是否隐藏
* `height="200"`			窗口高度
* `sizemode="maximized | minimized| normal"`		窗口初始化模式，最大化、最小化和普通

#### 4.2按钮 <button>

`<button id="button" label="Find" default="true"/>`

按钮比较简单，label是显示的文字，如果有多种语言版本，就要在文件头载入DTD文件，然后用引用形式显示文字，后面将做介绍，default是布尔值，表示窗口打开时按回车所触发的按钮

#### 4.3文本标签<label>和<description>

`<label value=" DisplayName " class="label"/>`

Value是显示文本，同时可以用class来为标签定义样式

`<description value=" DisplayName " class="label"/>`

Description基本上跟label是相同的，比如上面2行代码显示效果是一样的，但description标签可以显示多行文字，例如
```xml
<description>
	This is a multi-line description.
	It should wrap if there isn't enough room to put it in one line.
	Let's put in another sentence for good measure.
</description>
```
跟html一样，多个空格将被当成一个空格，回车和’\n’也是不起作用的，只有当文本宽度超过上级元素围的宽度了，才会换行，但是也可以用width属性来定义宽度，例如
```html
<description style="max-width:20px;">
	This is a multi-line description.
	It should wrap if there isn't enough room to put it in one line.
	Let's put in another sentence for good measure.
</description>
```
例外，文本标签可以使用control属性来跟按钮、输入框之类的元素绑定，下面代码点击label后会触发button，accesskey是快捷键，用Alt+D即可使该label为当前焦点
```html
<label control="DisplayName" value=" DisplayName " accesskey="d" class="label"/>
<button id=" DisplayName " label="MyName"/>
```

#### 4.4图片<image>

`<image src=”cat.png” width=”20” height=”26”/>`

Image标签跟html一样，不过这不是常用的作法。常用的作法是放在skin目录下，通过样式表来控制。
例如：`<image id="cat"  />`

同时在CSS里面指定属性
```css
#cat{
  height: 20px;
  width: 26px;
  background-image: url(images/cat.gif);
}
```
当然，你也可以用这样显示图片，但我觉得这样并不灵活，比较难控制
```css
#cat{
  height: 20px;
  width: 26px;
  list-style-image: url(images/cat.gif);
}
```

4.5输入组件<textbox>、<checkbox>和<radio>