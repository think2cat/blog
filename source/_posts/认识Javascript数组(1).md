title: 认识Javascript数组(1)
tags:
  - javascript
  - 笔记
id: 1089
categories:
  - Web
abbrlink: 5eda173d
date: 2009-07-30 20:20:33
---
## 1.认识数组

数组就是某类数据的集合，数据类型可以是整型、字符串、甚至是对象
Javascript不支持多维数组，但是因为数组里面可以包含对象（数组也是一个对象），所以数组可以通过相互嵌套实现类似多维数组的功能

### 1.1 定义数组

声明有10个元素的数组
```js
var a = new Array(10);
```

此时为a已经开辟了内存空间，包含10个元素，用数组名称加 [下标] 来调用，例如 a[2] 但此时元素并未初始化，调用将返回 ** undefined **

以下代码定义了个可变数组，并进行赋值

```js
var a = new Array();
a[0] = 10;
a[1] = "aaa";
a[2] = 12.6; 
```
<!--more-->

上面提过了，数组里面可以放对象，例如下面代码
```js
var a = new Array();
a[0] = true;
a[1] = document.getElementById("text");
a[2] = {x:11, y:22};
a[3] = new Array();
```

数组可以实例化的时候直接赋值，例如
```js
var a = new Array(1, 2, 3, 4, 5);
var b = [1, 2, 3, 4, 5];
```
a 和 b 都是数组，只不过b用了隐性声明，创建了另一个实例，此时如果用alert(a==b)将弹出false

### 1.2 多维数组

其实Javascript是不支持多维数组的，在asp里面可以用 dim a(10,3)来定义多维数组，在Javascript里面，如果用```var a = new Array(10,3)```将报错
但是之前说过，数组里面可以包含对象，所以可以把数组里面的某个元素再声明为数组，例如
```js
var a = new Array();
a[0] = new Array();
a[0][0] = 1;
alert(a[0][0]);  //弹出 1
```

声明的时候赋值
```js
var a = new Array([1,2,3], [4,5,6], [7,8,9]);
var b = [[1,2,3], [4,5,6], [7,8,9]];
```
效果一样，a采用常规实例化，b是隐性声明，结果都是生成一个多维数组

### 1.3 Array literals

这个还真不知中文怎么叫，文字数组？
说到数组，不得不说到Array Literals，数组其实是特殊的对象，对象有特有属性和方法，通过 **对象名.属性** 、**对象.方法()** 来取值和调用，而数组是通过下标来取值，Array Literals跟数组有很多相似，都是某数据类型的集合，但是Array Literals从根本来说，是个对象，声明和调用，跟数组是有区别
```js
var aa = new Object();
aa.x = "cat";
aa.y = "sunny";
alert(aa.x);      //弹出cat
```

创建一个简单的对象，一般调用是通过aa.x，而如果当成Array literals的话，用```alert(aa["x"])```一样会弹出cat
```js
var a = {x:"cat", y:"sun"};
alert(a["y"]);   //弹出sun
```
这是另一种创建对象的方法，结果是一样的