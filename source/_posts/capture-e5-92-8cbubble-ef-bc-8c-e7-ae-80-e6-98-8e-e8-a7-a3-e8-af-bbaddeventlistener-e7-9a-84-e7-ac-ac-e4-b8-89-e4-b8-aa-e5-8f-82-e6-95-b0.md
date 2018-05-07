title: Capture和Bubble，简明解读addEventListener的第三个参数
tags:
  - javascript
id: 1605
categories:
  - Web
date: 2013-03-13 22:42:42
---
**target.addEventListener(type, listener[, useCapture]);**

前两个参数很好理解，这里不多说，第三个参数是个布尔值，代表Capture和Bubble
网上解释很多，啰哩啰嗦的，这里用一段代码来解读
```html
<!DOCTYPE html>
<html>
<head>
</head>
<body>
    <div style="border:1px solid red" id="aa">aaaa
        <div style="border:1px solid yellow" id="bb">bbbb
            <div style="border:1px solid blue" id="cc">cccc
            <div style="border:1px solid green" id="dd">dddd</div>
            </div>
        </div>
    </div>

    <script language="javascript">
        var dom = document.getElementsByTagName("div");
        for(var i =0; i<dom.length; i++){
            dom[i].addEventListener("click",function(evt){
                console.log(this.id);
            },false);
        }
    </script>
</body>
</html>
```
<!--more-->
addEventListener第三个参数为false，也是最常用的

在chrome下执行后点击 dddd，控制台依次打印 dd > cc > bb > aa

接着把第三个参数改为true，再点击dddd，控制台依次打印 aa > bb > cc > dd

LOOK，明了，可以这么理解Capture和Bubble的事件触发顺序

> Capture: document > body > div

> Bubble: div > body > document

转一张w3c的图，来源地址 [http://www.w3.org/TR/DOM-Level-3-Events/#event-flow](http://www.w3.org/TR/DOM-Level-3-Events/#event-flow)

[![Capture](/images/2013/03/Capture.jpg)](/images/2013/03/Capture.jpg)