---
title: Promise入门指南(4)
categories:
  - Web
tags:
  - javascript
  - promise
abbrlink: 2ab61b58
date: 2018-07-21 23:25:12
---
### Promise.all
有一些场景，需要同时调用几个接口，拿到全部返回数据，才能接着往下调用
以往遇到这种情况，需要设置一些临时变量来记录每个接口是否返回数据，每次返回时再判断是否数据已经齐全，再往下接着走
而 all 方法就是为此而生的，接受参数为 Promise对象数组
```js
let pArr = [];
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p1 resolve");
		resolve("resp1");
	}, 1000);
}))
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p2 resolve");
		resolve("resp2");
	}, 5000);
}));
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p3 resolve");
		resolve("resp3");
	}, 3000);
}));
let p = Promise.all(pArr);
p.then(resp => {
	console.log("all is done, doing next");
});

// => p1 resolve
// => p3 resolve
// => p2 resolve
// => all is done, doing next
```
<!--more-->
而如果在Promise对象数组里，有任一个出现rejected状态，则会立即执行外层注册的rejecte回调，但是剩下未完成的Promise也依旧会继续执行，前面说过了Promise一旦开始，就会持续到状态变成fuil或rejected，并不受外界影响，也无法取消执行
```js
let pArr = [];
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p1 resolve");
		resolve("resp1");
	}, 1000);
}))
pArr.push(new Promise((resolve, reject) => {
	setTimeout(() => {
		console.log("p2 reject");
		reject("resp2");
	}, 3000);
}));
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p3 resolve");
		resolve("resp3");
	}, 5000);
}));
let p = Promise.all(pArr);
p.then(resp => {
	console.log("all is done, doing next");
},resp => {
	console.log("reject!",resp);
});

// => p1 resolve
// => p2 reject
// => reject! resp2
// => p3 resolve
```

### Promise.race

通过 race 掺入的Promise对象数组，其中任何一个Promise的状态改变，则立即返回状态给外层回调
如果最先完成的Promise是resolved，则调用外层 resolve， 反之如果是 rejected，则调用外层 rejecte回调

```js
let pArr = [];
pArr.push(new Promise((resolve, reject) => {
	setTimeout(() => {
		console.log("p1 reject");
		reject("resp1");
	}, 1000);
}))
pArr.push(new Promise((resolve, reject) => {
	setTimeout(() => {
		console.log("p2 reject");
		resolve("resp2");
	}, 3000);
}));
pArr.push(new Promise(resolve => {
	setTimeout(() => {
		console.log("p3 resolve");
		resolve("resp3");
	}, 5000);
}));
let p = Promise.race(pArr);
p.then(resp => {
	console.log("resole!", resp);
},resp => {
	console.log("reject!", resp);
});

// => p1 reject
// => reject! resp1
// => p2 reject
// => p3 resolve
```
