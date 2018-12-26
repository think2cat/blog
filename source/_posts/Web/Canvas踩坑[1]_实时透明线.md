---
title: 'Canvas踩坑(1)实时绘制透明线'
categories:
  - Web
tags:
  - javascript
  - canvas
abbrlink: 4d59cb14
date: 2018-11-23 16:51:24
---
![canvasOpacity_1](/images/2018/11/canvasOpacity_1.png)

Canvas画线算是最基础的操作，涉及的方法也比较简单

```js
_cv = _canvas.getContext('2d');
_cv.lineWidth = 20
_cv.strokeStyle = 'red'
_cv.beginPath()
_cv.moveTo(point.x, point.y)
_cv.lineTo(new.x, new.y)
_cv.stroke()
```
点很多的话，加个 `for` 循环也轻松搞定，如果画的是透明线，只需用 RGBA 格式即可，比如上图颜色为
```js
_cv.strokeStyle = 'rgba(253, 150, 38, 0.41)'
```
<!--more-->
这个方法在已知画笔路径点的时候，是可以画出透明线，透明叠加起来效果也很好
但是如果是实时作图的，因为 `lineTo()` 只是画路径，并不会描线，所以需要不断调用 `stroke()` 来画出可见线条

但这么做的话，如果是透明的线，就会出现问题

![canvasOpacity_2](/images/2018/11/canvasOpacity_2.png)

只有线最后一点是透明，其它部分全部变成不透明了

![canvasOpacity_3](/images/2018/11/canvasOpacity_3.png)

因为在 `lineTo()` 的时候，每加一点，就 `stroke()` ，这样实际上变成

1. 1 => 2 渲染
2. 1 => 2 => 3 渲染
3. 1 => 2 => 3 => 4 渲染
4. 1 => 2 => 3 => 4 => 5 渲染

所以原来透明的线，渲染覆盖了几次后，变成不透明

然后我就想着，那可以分段渲染，每次 `stroke()` 时都先跳到最后渲染的点，然后画新路径，这样就只渲染新增的部分

1. 1 => 2 渲染
2. moveTo 2 => 3 渲染
3. moveTo 3 => 4 渲染
4. moveTo 4 => 5 渲染

然而就是这样

![canvasOpacity_4](/images/2018/11/canvasOpacity_4.png)

线头跟线头重叠，透明度有变化，导致了很多圆点，线头设置的是圆头 `_cv.lineCap = 'round'`
如果改成平头 `_cv.lineCap = 'butt'`

是这样子

![canvasOpacity_5](/images/2018/11/canvasOpacity_5.png)

无意间发现Canvas有个属性 `globalAlpha`，是全局透明度，想到可以用2个canvas叠加
上层是实时作画区，下层则是已画好的线，上层设置全局透明，`strokeStyle` 则设置不透明，就可以实现实时画透明线

画出的线提笔后(mouseUp)，再把上层图像合成到下层去，同时清空上层canvas

```js
_cv1 = can1.getContext('2d')
_cv2 = can2.getContext('2d')
img = _cv1.getImageData(0,0, _cv1.width, _cv1.height)

_cv2.putImageData(img, 0, 0)
_cv1.clearRect(0, 0, _cv1.width, _cv1.height)
```

然后某天在 [知乎](https://www.zhihu.com/question/279376053/answer/519236463) 有大佬提供了另一个方法 [stackoverflow](https://stackoverflow.com/questions/20474706/drawing-semi-transparent-lines-on-mouse-movement-in-html5-canvas)

大概就是先把当前canvas保存起来，然后每画一个点的时候
1. 恢复存档，1 渲染
2. 恢复存档，1 => 2 渲染
2. 恢复存档，1 => 2 => 3 渲染
3. 恢复存档，1 => 2 => 3 => 4 渲染

```js
paths.push([pos]); // Add new path, the first point is current pos.
refresh()

function refresh() {
    // Clear canvas and draw the cat.
    ctx.clearRect(0, 0, ctx.width, ctx.height);
    if (globImg)
        ctx.drawImage(globImg, 0, 0);

    for (var i=0; i<paths.length; ++i) {
        var path = paths[i];

        if (path.length<1)
            continue; // Need at least two points to draw a line.

        ctx.beginPath();
        ctx.moveTo(path[0].x, path[0].y);
        ...
        for (var j=1; j<path.length; ++j)
            ctx.lineTo(path[j].x, path[j].y);
        ctx.stroke();

    }
}
```

我把这个加到项目里，canvas 实际分辨率是 1512*1080，处理起来丝毫不会延迟

![canvasOpacity_6](/images/2018/11/canvasOpacity_6.gif)