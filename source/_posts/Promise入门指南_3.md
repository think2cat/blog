---
title: Promise入门指南(3)
categories:
  - Web
tags:
  - javascript
  - promise
abbrlink: 57c1ef1d
date: 2018-07-15 18:44:55
---
Promise是JavaScript异步编程解决方案，之一
以往我们所习惯的回调函数也是之一，还有事件监听和观察者模式（也叫发布订阅模式）也算
而Promise是遵循的是Promise/A+规范，像Axios也是遵循这个规范，提供了相同的api

Promise/A+规范解决了回调函数的一些弊端，比如
* 代码书写逻辑
* 更改回调顺序的噩梦
* 多步回调的魔鬼嵌套
* 回调函数的跟踪

但Promise也不是万能的，作为解决方案之一，并不是适合所有的异步场景

前面说到Promise特征之一就是状态一旦变成 fulfilled 或是 rejected ，则不会再改变，而当执行 then 进行 resolve 和 rejected 回调注册时，返回的其实是一个新的Promise对象

```js
var p = new Promise(resolve => {
	setTimeout( () => {
		resolve(new Date().getTime());
    },1000);
});
console.dir(p);
console.dir(p.then((v) => {
	return new Date().getTime();
}));
```
<!--more-->
![img](/images/2018/07/promise_newobj1.png)

分别打印2次执行返回的Promise对象，发现并不一样，第一个是 pending ，而第二个是 resolved 而且 PromiseValue 是 undefined
其实这是因为Promise是异步执行，代码是先返回新的Promise对象，再执行异步处理，所以在打出第一个Promise对象那一刻确实是 pending 状态，而第二个打出的一刻其实也是 pending 状态，只是没有设置延迟，所以等我点开对象时已经变成 resolved
如果分别延迟打印这2个返回的对象，应该都是 resolved ，测试一下这个猜想

```js
var p = new Promise(resolve => {
	setTimeout(() => {
		resolve(new Date().getTime());
    },1000);
});
setTimeout(() => {
	console.dir(p);
}, 2000);
setTimeout(() => {
	console.dir(p.then((v) => {
		return new Date().getTime()
	}));
}, 3000);
```
![img](/images/2018/07/promise_newobj2.png)

在注册 resolved 和 rejected 函数时，函数体本身的return其实是Promise对象里的PromiseValue，可以由下级注册的回调函数所捕获
如果需要在级联也保持异步回调 resolved 或 rejected，则需要返回一个Promise对象，而不是then生成的
```js
var p1 = new Promise(resolve => {
	console.log("exec 1", new Date().getSeconds());
	setTimeout(() => {
		console.log("resolve 1", new Date().getSeconds());
		resolve(new Date().getTime());
    },2000);
});
var p2 = p1.then((v) => {
	console.log("rev", v, new Date().getSeconds());
	return new Promise(resolve => {
		console.log("exec 2", new Date().getSeconds());
		setTimeout(() => {
			console.log("resolve 2", new Date().getSeconds());
			resolve(new Date().getTime());
	    },2000);
	})
});
console.log(p2, new Date().getSeconds());
```
![img](/images/2018/07/promise_newobj3.png)