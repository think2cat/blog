---
title: 异步回调之generator（1）入门篇
tags:
  - javascript
  - generator
categories:
  - Web
abbrlink: ddbd4da6
date: 2018-09-11 17:39:39
---
### 什么是Generator

Generator 是ES6新特性，是另一种异步编程解决方案，提供了与之前传统函数不一样的语法行为
如果说Promise解决了回调函数的地狱式嵌套，Generator则解决了Promise.then()的多级链式调用

### 语法

`function* Generator()` Generator写法跟普通函数一样，`function` 和函数名 `Generator` 中间多了个`*`号，同时函数内部可以使用 `yield` 进行暂停并输出值

```js
// *号可随意放置，推荐使用第一种写法
function* Generator() {}

function * Generator() {}

function *Generator() {}

function*Generator() {}
```
<!--more-->
### 迭代器

```js
function* Generator() {
    let i = 0;
    i ++;
    yield i;
    i = i * 2;
    yield { i }
}
let hello = Generator()
```

`Generator()` 之后函数并不会立马执行，而是返回一个迭代器对象(Iterable Object)

![generator](/images/2018/09/generator1.png)

返回对象是暂停状态，调用 `next()` 才会开始执行函数，并在遇到 `yield` 时再次暂停，如果没有则一直执行到函数结束或者遇到 `return`
同时 `yield` 会像 `return` 一样会返回后面的值
`yield` 是Generator特有的，函数内可以多次使用，在非Generator函数内执行会报错

```js
hello.next()
// {value: 1, done: false}

hello.next()
// {value: { i: 2 }, done: false}

hello.next()
// {value: undefined, done: true}
```

因为函数结尾没有 `return` 所以最后返回 `value: undefined`，此时 `done: true` 表示函数执行完毕，hello的状态变成由 `suspended` 变成 `closed`
此时若再次执行 `hello.next()` 也依旧返回 `value: undefined`，

### next() 传参

`next()` 支持传参，`yield` 默认是无返回，即是undefined，如果传入值，则会被当做 `yield` 返回值

```js
function* Generator(){
	let i = 10
	while (i > 0) {
		console.log(yield i--)
    }
}
let hello = Generator()
hello.next('a')
// {value: 10, done: false}

hello.next('b')
// b
// {value: 9, done: false}

hello.next('c')
// c
// {value: 8, done: false}
```

第一个 `next('a')` 传入'a'，此时函数开始运行，直到第一次遇到 `yield i--` ，此时暂停并返回 `i = 10` ，因为已经暂停，所以并不会执行 `console.log()`，实际上第一次执行 `next()` 只相当于 `start()` 的效果，所以传参并没有意义
第二个 `next('b')` 传入'b'，执行第一次循环被暂停的 `console.log()` 打印 `b`，接着循环到第二次 `yield i--`，并返回 `i = 9`

### throw() 抛出异常

迭代器对象自身有 `throw()` 方法，可以在外面抛出异常到函数内部
```js
function* Generator(){
	let i = 10
	while (i > 0) {
		try {
			console.log(yield i--)
        } catch(e) {
			console.log('error', e)
        }
    }
}
let hello = Generator()
hello.next()
// {value: 10, done: false}

hello.throw('b')
// error b
// {value: 9, done: false}
```

上面代码迭代器对象的 `throw()` 只对内部抛出异常，而当内部没有 `catch` 到时会产生外部的异常，而全局的 `throw()` 则只能对外部抛出异常
需要特别注意的是要先执行 `next()` 后才可以执行 `throw()`，如果直接执行 `throw()` 外部会异常，此时迭代器对象直接变成 `close` 状态

### return() 终止迭代器

相当于函数内的 `return`，迭代器对象不会再执行未执行的代码，状态变为 `close` 并返回传入值

```js
function* Generator(){
    let i = 0
    while(true) {
        yield i++
    }
}
let hello = Generator()
hello.next()
// {value: 0, done: false}

hello.return(100)
// {value: 100, done: true}

hello.next()
// {value: undefined, done: true}

hello.return()
// {value: undefined, done: true}

hello.return('abc')
/// {value: "abc", done: true}
```

执行了 `return()` 后，虽然执行 `next()` 已经无效果，但 `return()` 依然可以再次执行，并再次返回传入的值

### yield*

在Generator如果嵌套了另一个Generator，此时如果想执行内部Generator的 `yield` 进行迭代，则需要用到 `yield*`，先看不使用的情况

```js
function* Generator(){
    yield 1
    yield inner()
    yield 2
    yield 3
}

function* inner(){
    yield 'a'
    yield 'b'
    return 'c'
}

let hello = Generator()
hello.next()
// {value: 1, done: false}

hello.next()
// {value: inner, done: false}

hello.next()
// {value: 2, done: false}
```
在执行第二次 `next()` 时实际返回的 value是 `inner` 对象，而不是 `inner` 里面的 `yield` 返回值

然后换成 `yield*` 再执行

```js
function* Generator(){
    yield 1
    yield* inner()
    yield 2
    yield 3
}

function* inner(){
    yield 'a'
    yield 'b'
    return 'c'
}

let hello = Generator()
hello.next()
// {value: 1, done: false}

hello.next()
// {value: "a", done: false}

hello.next()
// {value: "b", done: false}

hello.next()
// {value: 2, done: false}
```

可以看出 `yield*` 把 `inner()` 也迭代执行了，一直执行到 `return` ，嵌套函数结束，然后把控制权交回主函数
