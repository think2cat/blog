---
title: Promise入门指南(5)
categories:
  - Web
tags:
  - javascript
  - promise
abbrlink: 33ad2a19
date: 2018-08-10 00:26:24
---
### Promise.catch

`catch` 是用来捕获异常的，但是网上很多教程代码把 `catch` 当 `rejected` 使用，这是不当的

前面说了 Promise 有3种状态， pending, fulffilled, rejected
并没有一种状态叫异常，实际使用中，能确定的状态用 resolve / reject 来回调，而不是丢到 catch

```js
// 错误示例
var p1 = new Promise(resolve => {
    // try something
	throw new Error('something error')
})
p1.then(v => {
    console.log('p2', v)
}).catch(v => {
    console.log('p3', v)
})
```
这样当报错时，会抛到 catch 去执行，而实际有些错误是可预见的，所以当可预见和不可预见的错误，都往 catch 报的时候，就没法判断了，因此建议用 reject 来处理可预见性的错误
<!--more-->
而如果 `then()` 回调有 reject，不管有没有定义 `catch()` 捕获错误，都会通过 reject 抛出异常
```js
var p1 = new Promise(resolve => {
    // try something
	throw new Error('something error')
})
p1.then(v => {
    console.log('resolve', v)
}, v => {
    console.log('reject', v)
}).catch(v => {
    console.log('catch', v)
})
// => reject Error: something error
```

这看起来有点奇怪，其实是因为Promise链式写法，每次都返回一个新的Promise对象，而且只能捕获来自上一级的异常


```js
var p1 = new Promise(resolve => {
	setTimeout(() => {
		resolve('p1');
	}, 1000);
})
var p2 = p1.then(v => {
    console.log('p2', v)
})
var p3 = p2.catch(v => {
    console.log('p3', v)
})
p1 === p2  // -> false
p2 === p3  // -> false
p1 === p3  // -> false
```
上面代码中，每一级 `then()` 或是 `catch()` 其实都返回了一个新的Promise对象，类似Git分支的意思

本文最开始的错误示范，在 p1 抛出异常后，正常情况会调用 `then()` 中的 `reject` 回调，因为在这一级并没有设置 reject 回调，导致异常又抛给后面一级的 `reject`，而后面同样没有设置回调，所以最终落到 `catch()`
修改后的代码如下
```js
var p1 = new Promise(resolve => {
    // try something
	throw new Error('something error')
})
var p2 = p1.then(v => {
    console.log('p2', v)
})
var p3 = p2.then(v => {
    console.log('p3:resolve', v)
},v => {
    console.log('p3:reject', v)
})
```
p1抛出了错误给 reject 回调，并返回了新 Promise 对象即p2，而 p2 因为没有 reject 回调又抛给了下一级 reject， 此时有reject所以打印出 `p3:reject`， 并返回新 Promise 对象p3，此时 p3 并没有异常所以是 resolved 状态