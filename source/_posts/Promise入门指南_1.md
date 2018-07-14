---
title: Promise入门指南(1)
categories:
  - Web
tags:
  - javascript
  - promise
abbrlink: 57c1ef1d
date: 2018-06-04 14:48:28
---
## Promise是异步编程的解决方案

Promise有3种状态
* pending
* fulfilled
* rejected

对象的状态是不受外界影响的，而且状态的改变只有两种
1. pending -> fulfilled
2. pending -> rejected

一旦变成fulfilled或rejected，则终止，不会再改变
而在pending阶段，是不能取消，即一旦开始执行，就只能等状态改变，无法中途取消操作

## 代码示例

```js
var p = new Promise(function(resolve, reject){
    //do something
    setTimeout(function(){
        console.log('finished');
        resolve('someone');
    }, 2000);
});
```
<!--more-->
创建一个Promise实例，其构造函数接收一个function作为参数，而function又有2个参数，分别为resolve和reject
状态在pending变为fulfilled时执行 resolve，变为rejected则执行 reject，可以理解为成功和失败异常的回调
这样看起来跟老式callback好像差不多啊
打印看下

![img](/images/2018/07/promise_dir.png)

除了包含上面提到的resolve和reject，可以看到原型还包含then和catch，这些是最常用到的

如果只是简单的回调，使用起来确实差不多
```js
function getList(){
    loadfile(url1, function(response){
       //do something
    });
}
```
这是最常见的ajax异步回调，一次请求即可完成，如果是特殊一点，需要先从A地址获取A参数，然后提交到B地址，甚至提交到B地址成功后再回写到C地址，那则需要层层嵌套

```js
function add(){
    ajax(url, function(url){
        ajax(url, function(url){
            ajax(url, function(response){
                //do something
            });
        });
    });
}
```

用Promise来重写这段代码
```js
let p = new Promise((resolve, reject) =>{
    //do something
    setTimeout(()=> {
       resolve("url1");
    },2000);
});
p.then(res => {
    console.log("cb1", res);
    return "url2"
}).then(res => {
    console.log("cb2", res);
    return "url3"
});
```
控制台先打印了Promise对象，2秒后打出cb1和cb2
```
[Promise]

cb1 url1
cb2 url2
```
虽然是链式，看起来跟平时用的 ajax 异步回调有点不一样啊，链式写法多步执行后面会详细讲，这节先讲基本语法

前面提到 then 可以传入2个function作为参数，分别代表 resolve 和 reject 的回调
同时有个 catch 用于捕获异常，如果传入 then 只有 resolve 函数，在状态变为 rejected 时会调用传入 catch 的函数
```js
let p = new Promise((resolve, reject) =>{
    //do something
    setTimeout(()=> {
       reject("url");
    },2000);
});
p.then(res => {
    console.log("cb1", res);
    return "url2"
}).catch(res => {
	console.log("err",res);
	return "error";
});
// -> err url
```
值得说的是即使状态从pending变为fulfilled，理应执行resolve，但是如果执行resolve时异常了，同样会调用catch传入的function，如下面代码
```js
let p = new Promise((resolve, reject) =>{
    //do something
    setTimeout(()=> {
       resolve("url");  // <= 执行resolve
    },2000);
});
p.then(res => {
    console.log("cb1", aaaaaa);     // <- 打印不存在的变量，报异常
    return "url2"
}).catch(res => {
	console.log("err",res);
	return "error";
});
// -> err url
```