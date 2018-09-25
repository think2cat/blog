---
title: Promise入门指南(2)
categories:
  - Web
tags:
  - javascript
  - promise
abbrlink: 7cecbcde
date: 2018-07-13 18:01:16
---
```js
var p = new Promise(function(resolve, reject){
    //do something
    setTimeout(function(){
        console.log('finished');
        resolve('someone');
    }, 2000);
});
```
前面提到的这种写法，实际不会这么直接把Promise赋值给p，因为大部分function调用需要传入参数，所以习惯在function里面 `new Promise` 示例并return
```js
var getUrl = param => {
    return new Promise(resolve => {
        //do something
        $.ajax(url, response => {
            resolve(response);
        });
    });
}
getUrl(1).then(response => {
    render(response);
});
```
这是比较标准的写法，还有比较偷懒的写法
<!--more-->
```js
var getUrl = Promise.resolve();
getUrl.then(response => {
});
```
前面的 `resolv()` 中传什么东西，到了 `then` 中的 `response` 就是前面传入的东西
如果为空，则 `response = undefined`，如果是 function 则 `response = function`
```js
var getUrl = Promise.resolve(() => {
	console.log("hello");
});
getUrl.then(cb => {
	console.log("It's raining day");
	cb();
});
// => It's raining day
// => hello
```

而在实例化Promise时，回调 resolve 和 reject 传入的参数，只能是一个，多出的是获取不到的
```js
var getUrl = new Promise(resolve => {
	setTimeout( () => {
		resolve("a", "b");
    },2000);
});
getUrl.then((i, k) => {
	console.log(i, k);
});
// => a undefined
```

在 then 传入的参数，也必须是 function，如果非 function 则忽略，而传入的 function 最多2个，分别为 onFulfilled 和 onRejected 时调用，也可以不传

前面说到状态一旦变为 fulfilled 或 rejected 则不会再改变，那如果已经执行 then 中的 resolve，此时再调用 then 是否不会执行 resolve，测试一下
```js
var p = new Promise(resolve => {
	setTimeout( () => {
		resolve(new Date().getTime());
    },2000);
});
p.then((v) => {
	console.log(v);
});
setTimeout(() => {
	p.then((v) => {
		console.log(v);
	});
},4000);
// => 99
// => 1531586556347
// => 1531586556347
```
在第二次调用 then 时，resolve 依然再次执行了
