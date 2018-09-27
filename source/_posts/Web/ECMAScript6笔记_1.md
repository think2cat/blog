---
title: ECMAScript6笔记（1）
tags:
  - ES6
  - javascript
id: 1732
categories:
  - Web
abbrlink: 1768cee0
date: 2017-03-06 00:54:57
---
[阮老师的《ECMAScript6入门》](http://es6.ruanyifeng.com/)


#### let
声明块级作用域变量
`let` 用在花括号 { }里面
在用 `let` 声明之前，变量不可用，会报错，而 `var` 声明之前的变量则返回 `undefined`

#### class
更接近传统OOP的写法
```js
class Cat{
    constructor(){
        this.name = "cat";
        this.color = "black";
    }
}
class YellowCat extends Cat{
    constructor(){
        super();
        this.color = 'yellow';
    }
}
```
<!--more-->
#### 箭头函数 =>
`i => i+1;`
等于ES5的 `function(i){return i+1}`
简洁的无法接受！跟ES5不同的是function中的 `this` 指向定义function时的对象，而不是运行时的所在对象
比如ES5经常用的
```js
var self = this;
setTimeout(function(str){
    self.hello(str);
},1000);
```
到了ES6就没必要定义self，而是
```js
setTimeout( (str) => this.hello(str) , 1000)
```

#### destructuring
这是我见过最无法适应的写法，但确实简洁
```js
var a = 123, b = 456;
var c = {a,b};
// c = {a:123, b:456}
```
还可以倒过来赋值
```js
var d = {a:"cat", b:"monkey"}
var {a, b} = d;
// a = "cat", b = "monkey"
```

#### default
设置默认值，比如常见的这个
```js
function say(str){
    str || str = "hello";
    console.log(str);
}
```
到了ES6，就可以这么写
```js
function say(str = "hello"){
    console.log(str);
}
```

#### rest
相当于arguments，比如
```js
function test(...types){
    console.log(types);
    console.log(arguments);
}
test(1,3,5,6,"a","b");
// --> [1, 3, 5, 6, "a", "b"]
// --> [1, 3, 5, 6, "a", "b"]
```