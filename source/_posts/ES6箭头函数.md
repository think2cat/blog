---
title: ES6箭头函数
tags:
  - ES6
  - javascript
id: 1809
categories:
  - Web
abbrlink: 5b0ab7b9
date: 2017-06-10 00:31:48
---
第一次见到这箭头=>，我觉得这是很奇葩的写法，因为作为一个老男人来说，会影响阅读和理解代码的速度=_=||

作为ES6新特性之一，箭头函数特点如下

a）简洁
b）this指向
c）rest取代arguments

#### 1. 简洁

```js
var showTips = function(d){
    $("#Tip").text(d).show();
}
```
这段代码换成箭头写法是这样
```js
var showTips = (d) => {$("#Tip").text(d).show()};
```
<!-- more -->
因为只有一个参数 d 所以括号可以省去，因为function只有一句所以花括号也可以省去，如下
```js
var showTips = d => $("#Tip").text(d).show();
```

但是这2个function并不完全相同，去掉花括号后，其实等价于
```js
var showTips = function(d){
    return $("#Tip").text(d).show();
}
```
也就是带花括号的是原汁原味的code，而省了花括号后会自动加上return，比如做价格运算时
```js
function getTotalPrice(price, count){
    return price * count;
}
```
用箭头函数写法就是
```js
var getTotalPrice = (price,count) => price * count;
```
确实相当简洁

#### 2. this指向

```js
function Test(s){
    this.dom = $("#Tip");
    this.dom.text(s).show();
    setTimeout(function(){
        this.dom.hide();
    },2000);
}
```
这段代码看起来没大问题，甚至直接执行Test()也没问题，不报错，2秒后dom也会自动隐藏，但是这里面的this指向了window，也就是本来应该局部变量却提升到全局变量，造成污染了
所以一般采用闭包或是实例化，例如
```js
var t = new Test("Hi Function");
```
此时function里的this指向对象 t 本身，但是setTimeout里面的this却指向window，会导致2秒后找不到dom对象
正常做法是setTimeout前var一个变量来保存this
```js
function Test(s){
    this.dom = $("#Tip");
    this.dom.text(s).show();
    var self = this;
    setTimeout(function(){
        self.dom.hide();
    },2000);
}
var t = new Test("Hi Function");
```
这是比较常见但也比较丑陋的写法，还有一种是这样
```js
function Test(s){
    this.dom = $("#Tip");
    this.dom.text(s).show();
    setTimeout(function(){
        this.dom.hide();
    }.bind(this),2000);
}
var t = new Test("Hi Function");
```
把外面的this强行绑定到里面去，这样里面调用this则会变成调外面
到了ES6箭头函数，this并不会指向函数本身，而且往上层查找this，包括arguments
所以用箭头函数可以这样写
```js
function Test(s){
    this.dom = $("#Tip");
    this.dom.text(s).show();
    setTimeout( () => {
        this.dom.hide();
    },2000);
}
var t = new Test("Hi Function");
```

#### 3. argumengts

如上面说的，箭头函数里arguments会往上层查找arguments，所以下面这个最后输出Hi，而不是Hello
```js
function Test(s){
    return (() => {
        console.log(arguments)
    })("Hello");
}
Test("Hi");
```
再修改一下多套一层箭头函数
```js
function Test(s){
    return () => {
        return (() => {
            console.log(arguments)
        })("Hello");
    }
}
Test("Hi")();
```
一样打印Hi，因为会顺着作用域链往上查找，套了2个return看着有点乱，换个写法
```js
function Test(s){
    setTimeout( () => {
        console.log(arguments)
    },200);
}
Test(4,5,6); // -> [4,5,6]
```
如果想用arguments怎么办，可以用ES6新特征rest
```js
function Test(s){
    return (() => {
        return (...r) => {
            console.log(r)
        };
    })(1,2,3)
}
Test(4,5,6)(7,8); // -> [7,8]
```
所以有个面试经常用到的题目，写个计时器每一秒打印个数字，并递增1，就可以不用闭包写法了
```js
var i =0;
setInterval( () => {
    if(i<10) console.log(i++);
},1000);
```