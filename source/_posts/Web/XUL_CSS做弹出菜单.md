---
title: XUL+CSS做弹出菜单
tags:
  - css
  - XUL
id: 882
categories:
  - Web
abbrlink: 4867d35e
date: 2006-04-04 23:06:49
---

![](/images/2006/04/04_12758_12758.gif)

很多软件都用这种效果，使用的时候很简单，给<popup>标签加上css定义就可以了，如下面代码所示
<!--more-->
菜单我挑了一个代码少的，效果一样哈

```xml
  <popup id=”emailAddressPopup” popupanchor=”bottomleft” onpopupshowing=”goUpdateCommand(‘cmd_createFilterFromPopup’);” class=”popMunu”>
  <menuitem label=”&amp;SendMailTo.label;” accesskey=”&amp;SendMailTo.accesskey;” oncommand=”SendMailToNode(document.popupNode)”/>
  <menuitem label=”&amp;CreateFilterFrom.label;” accesskey=”&amp;CreateFilterFrom.accesskey;” command=”cmd_createFilterFromPopup”/>
  <menuitem label=”&amp;AddToAddressBook.label;” accesskey=”&amp;AddToAddressBook.accesskey;” oncommand=”AddNodeToAddressBook(document.popupNode)”/>
  <menuitem label=”&amp;CopyEmailAddress.label;” accesskey=”&amp;CopyEmailAddress.accesskey;” oncommand=”CopyEmailAddress(document.popupNode)”/>
</popup>
```

下面是CSS文件部分，其中popmune_left.png是蓝色的渐变图

```css
.popMunu{
  border: 1px solid #A0A0A0;
  background-color: #FAFAFA;
  padding: 0 2px 0 26px;;
  margin: 0;
  color: Black;
  line-height: 1.5em;
  background-image: url(images/popmune_left.png);
  background-repeat: repeat-y;
}
.popMunu menuitem{
  margin:1px 0;
  padding: 2px 0;
}
.popMunu menuitem:hover{
  background-color: #D5DFF1;
  color: Black;
}
```