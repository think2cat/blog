---
title: 《XUL指南》第四章：更多布局元素
tags:
  - xml
  - XUL
  - XUL指南
id: 903
categories:
  - Web
abbrlink: fb42f1fd
date: 2006-11-25 18:18:48
---

翻译时间：06年4月10日，本章未翻译完，不过可能不会再译了

## 四、更多布局元素

### 4.1 Stacks 和 Decks

有时需要将内容重叠起来，就像重叠的卡片，stack和deck元素可以实现这效果。

#### 容器

每个XUL里面BOX都是一个可以包含任何内容的容器 ，容器里面有许多不同类型的元素，例如工具栏、标签面板（Tabbed Panels）。BOX标签只是创建一个简单的容器，而里面其它元素将按默认规则工作，但它们有另外特征。

事实上，很多控件可以包含其它元素，例如我们之前看过的按钮。在容器里面滚动条（Scroll Bar）是一个特殊元素， 在接下来几节里，我们将介绍一些服务于其它元素的元素，它们都是特殊元素以及拥有容器的所有属性。

#### Stacks

stack元素是一个简单的容器，工作方式跟其它容器一样但有特殊性质，它子级元素是重叠排列的。第一个子元素渲染底部，第二个渲染第二层，接下来的元素渲染第三层，一层一层覆盖上去。

在stack容器中很少使用orient属性来规定子元素重叠或并行排列。stack容器的大小决定了子级元素最大值，但你可以使用CSS属性width，height，min-width和其它相关属性来描述stack和子级元素。 stack元素可以用来在现有元素上加状态显示，例如在进度条上加标签。
<!--more-->
了解一些CSS属性可以更方便的使用stack元素，例如你可以做个类似text-shadow属性的简单效果，像这样：
```xml
<stack>
  <description value="Shadowed" style="padding-left: 1px; padding-top: 1px; font-size: 15pt"/>
  <description value="Shadowed" style="color: red; font-size: 15pt;"/>
</stack>
```

两个 `description` 元素创建了15磅文本，首先用padding属性使它向右下方偏移1个象素，然后再用默认方式描绘shadowed文本，设为红色以便有明显效果。

这个方法比text-shadow有优势，因为可以很完美的描绘主文字的阴影部分。它可以设置自己的字体、下划线或大小（你甚至可以使阴影发光）。在Mozilla当前无法支持CSS文本阴影属性的情况下这个方法是很有用的。唯一的缺点就是阴影会使stack容器尺寸比较大。阴影效果在创建无效按钮的时候非常有用。
```xml
<stack style="background-color: #C0C0C0">
  <description value="Disabled" style="color: white; padding-left: 1px; padding-top: 1px;"/>
  <description value="Disabled" style="color: grey;"/>
</stack>
```

![](/images/2006/11/25_12763.jpg)在一些平台上，这种文本的排列方法加上阴影颜色创建了类似失效的文本效果。

必须记住的是鼠标点击和敲击按键只能激活最上层元素的事件，也就是最后加载的元素，在stack底部的的元素例如按钮是不起作用的。

#### Stacks

deck跟stack元素一样用于布置子级元素重叠，但是deck在同一时间只能显示其中一个子元素。这在有相似面板的向导界面用于显示流程非常有用。比起创建独立窗口再增加相同的导航按钮到每个窗口，你可以只创建一个窗口然后使用deck来更换内容。

在deck元素里显示页类似stack元素中的子级。比如deck元素有3个子级，你可以用selectedIndex属性来更换显示页，索引用来识别显示哪个内容页，从0开始，所有第一个显示页的索引值是0，第二个是1，依此类推。

请看下面关于deck的例子：
```xml
<deck selectedIndex="2">
  <description value="This is the first page"/>
  <button label="This is the second page"/>
  <box>
    <description value="This is the third page"/>
    <button label="This is also the third page"/>
  </box>
</deck>
```

deck元素包含3个页面，初始页是第三个，它是包含2个元素的容器，容器以及包含的元素都将显示在最上层，deck将跟子级一样大，在这里会跟第三个页面一样大。

你可以使用教本改变selectedIndex属性来切换页面，更多内容将在《事件和DOM》章节做介绍。