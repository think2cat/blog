---
title: jsPlumb学习笔记（1）
tags:
  - javascript
  - jsPlumb
id: 1867
categories:
  - Web
abbrlink: d3edd405
date: 2018-01-02 17:13:29
---
jsPlumb是一个DOM连线JS库，采用SVG绘制连线

[官网](https://jsplumbtoolkit.com/) | [GitHub](https://github.com/jsplumb/jsplumb) | [API](https://jsplumbtoolkit.com/docs.html)

这是前阵子做的DEMO，用来操作沙盘演示用，做得比较粗糙

本文代码均放[GitHub](https://github.com/think2cat/jsplumb_demo)，另外学习中参考了网上其他大牛，特别感谢这二位

[【看完想不会都难的系列教程】- (3) JQuery+JQueryUI+Jsplumb 实现拖拽模块，流程图风格](http://www.cnblogs.com/techborther/archive/2012/04/17/2454101.html)

[jsPlumb的简单使用](http://qkxue.net/info/182575/jsPlumb)

![](/images/2018/01/js2.gif)
<!-- more -->
下面一步步来玩弄这玩意

首先是页面代码结构，一个source表示原始元素，container表示编辑区画布

```html
<body>
<div class="root">
    <div class="source">
        <ul>
            <li>Red</li>
            <li>Blue</li>
        </ul>
    </div>
    <div class="container"></div>
</div>
</body>
```
![](/images/2018/01/js1.gif)

首先要完成拖动，用到jQuery UI的droppable
```js
$(".source").find("li").draggable({
    helper: "clone"
});
```
左侧元素可以拖动了，相当于在DOM元素加了属性 draggable="true"
这里draggable加了clone参数表明元素是克隆一个，而不是拖动原始DOM
接着需要设置接受拖动元素的容器

```js
$(".container").droppable({
	drop: (event, ui) => {
		let sourceDom = ui.draggable.first();
		let newDom = $("<div style='border-color:" + sourceDom.text() + "'>" + sourceDom.text() + "</div>");
		$(".container").append(newDom);
	}
});
```

为接收容器设置了拖放事件，当元素放到容器时触发drop事件，传入event事件和dom，本例只拖动一个元素所以source只有一个元素，取得原始DOM后进行组装成新DOM，再插入到容器

![](/images/2018/01/js3.gif)

完成之后会发现一个问题，就是拖到容器后的元素，没法再拖动了，因为我们给左边source设置了拖动，但是容器里的元素是新生成的，并没有设置拖动属性，所以在容器新增DOM后还需要加入拖动

```js
newDom.offset({
	"left": ui.offset.left,
	"top": ui.offset.top
}).draggable({
	containment: $(".container")
});

```
这句作用是拖动到容器后，设置新增DOM的XY坐标，并限制拖动范围，刷新看下效果

![](/images/2018/01/js4.gif)

凌乱了是吧，因为容器的drop事件设置的是有元素拖进来则clone，而没有判断拖动的元素是否是容器内部，只需加入父元素判断即可
```js
if (sourceDom.parent()[0] == document.getElementsByClassName("container")[0]) {
    //如果拖动的是容器里面的，则重绘
    jsPlumb.repaintEverything();
    return;
}
```

最终拖动效果如下

![](/images/2018/01/js5.gif)