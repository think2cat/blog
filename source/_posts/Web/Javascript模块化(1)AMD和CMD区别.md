---
title: 'Javascript模块化(1) AMD和CMD区别'
tags:
  - module
  - javascript
categories:
  - Web
abbrlink: d4129ce4
date: 2016-08-10 11:39:24
---

Javascript发展壮大到一定程度，代码量和复杂度今非昔比，于是从最初的单页面里写function，到使用立即执行函数写模块防止污染全局，到模块化规范标准

** CMD全称是Common Module Definition **，通用模块加载规范，使用同步加载，node.js和sea.js就采用这种，在需要的时候直接引入然后就可以调用，比如
```js
var cat = require("./animal.js");
cat.say("miao");
```
因为node.js是服务端执行的，加载的时间也就读取文件的时间，所以基本不会造成堵塞，但是客户端则不同，有网速的影响，就可能导致卡住假死，此时AMD就因此而生
<!--more-->
** AMD全称是Asynchronous Module Definition** ，异步模块加载规范，跟CMD使用上多了一个callback参数，比如
```js
var cat = require("./animal.js", function(cat){
cat.say("miao");
});
//do something
```
在加载模块后代码并不会卡主，而是继续往下执行，当模块加载完毕后再执行回调函数，类似ajax，所以AMD适合在浏览器环境下，可以避免假死一些极差的用户体验


** UMD全称是Universal Module Definition **，通用模块规范
兼容CMD和AMD同时，还兼容原始的全局引用
但是缺点也是显而易见，就是为了兼容性需要写一些额外代码

```js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery', 'underscore'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS-like
        module.exports = factory(require('jquery'), require('underscore'));
    } else {
        // Browser globals (root is window)
        root.returnExports = factory(root.jQuery, root._);
    }
}(this, function ($, _) {
    //    methods
    function a(){};    //    private because it's not returned (see below)
    function b(){};    //    public because it's returned
    function c(){};    //    public because it's returned

    //    exposed public methods
    return {
        b: b,
        c: c
    }
}));
```

本文仅为笔记总结，详细教程请看 [JS模块化开发(requireJS)](https://blog.csdn.net/liuyan19891230/article/details/50844319)