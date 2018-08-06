---
title: WebSocket入门（3）心跳检测
tags:
  - http
  - socket
categories:
  - Web
abbrlink: 30381c3b
date: 2018-05-11 10:41:26
---
websocket自带onerror回调，当异常会触发，但是在目前阶段，某些时候浏览器并不会触发，包括chrome，直接close
最好就是加一个心跳检测来判断网络异常
websocket有一个readyState状态值，代表的状态如下（摘自[阮一峰博客](http://www.ruanyifeng.com/blog/2017/05/websocket.html)）
* CONNECTING：值为0，表示正在连接
* OPEN：值为1，表示连接成功，可以通信了
* CLOSING：值为2，表示连接正在关闭
* CLOSED：值为3，表示连接已经关闭，或者打开连接失败

当成功建立连接后，即启动心跳检测，大概的流程如下图
![flow](/images/2018/05/websocket_heart.png)
<!--more-->
画完图觉得很简单，就花了十分钟敲完代码
```js
var heartCheck = {
  timer: 0,
  _obj : null,
  _callback:null,
  init: function(wsObj, callback) {
	console.log("init");
	this._obj = wsObj;
	callback && (this._callback = callback);
	this.sayHi();
  },
  sayHi: function() {
	clearTimeout(this.timer);
	this.timer = setTimeout(() => {
	  this.onError();
	}, 10000);
	this._obj.send("hi," + this.timer);
	console.log("sayHi:" + this.timer);
  },
  clear: function(flag) {
	console.log("clear:" + this.timer);
	clearTimeout(this.timer);
	if(true === flag){
	  console.log("heartCheck finished");
	  return;
	}
	setTimeout(()=>{
	  this.sayHi();
	}, 3000);
  },
  onError: function() {
	console.log("onError:",this.timer);
	clearTimeout(this.timer);
	this._callback && this._callback();
  }
};
//let hc = new heartCheck();
let uri = "ws://localhost:8080";
var ws = new WebSocket(uri);
ws.onopen = (event) => {
  console.log('ws onopen', event);
  ws.send('from client: hello');
  heartCheck.init(ws);
};
ws.onmessage = (event) => {
  console.log('ws onmessage');
  console.log('from server: ', event);
  showLog(event.data);
  heartCheck.clear();
};
ws.onclose = (event) => {
  console.log("ws close", event);
  console.log(ws);
  heartCheck.clear(true);
};
ws.onerror = (event) => {
  console.log("ws error", event);
  console.log(ws);
};
```
调试一下发现一些问题
1）每次发心跳都是重复2次
2）心跳结束后还在发心跳
3）未考虑多个事物连续发信息的优化

![](/images/2018/05/websocket_console.png)

重新理下思路，首先模拟一个正常的Socket通讯

```js
let uri = "ws://localhost:8080";
var ws = new WebSocket(uri);
ws.onopen = (event) => {
  console.log('ws onopen', event);
  MsgBegin && MsgBegin();
};
ws.onmessage = (event) => {
  console.log('ws onmessage', event);
  showLog(event.data);
};
ws.onclose = (event) => {
  console.log("ws close", event, ws);
};
ws.onerror = (event) => {
  console.log("ws error", event, ws);
};
let MsgBegin = ()=>{
  let n = Math.random() * 100000;
  setTimeout(()=>{
	ws.send("send:" + n);
	MsgBegin();
  },n);
}
```
WebSocket建立成功后，随机发送消息，服务端收到后返回（服务端实际设置了延迟1秒返回）

  websocket server is listening...
  server: receive connection.
  server: received: send:17534.353070269426
  server: received: send:80161.1894501739
  server: received: send:84509.49454040357
  server: received: send:29288.908671059355
  server: received: send:8404.852392415196
  server: received: send:45471.163768915
  server: received: send:16825.41444159582

加入心跳检测，设为间隔10秒
```js
var heartCheck = {
  timer: 0,
  _obj : null,
  _callback:null,
  _time: 10000, //心跳间隔10秒
  init: function(wsObj, callback) {
	console.log("init");
	this._obj = wsObj;
	callback && (this._callback = callback);
	this.sayHi();
  },
  sayHi: function() {
	clearTimeout(this.timer);
	this.timer = setTimeout(() => {
	  if(1 == this._obj.readyState){
		this._obj.send("check : " + this.timer);
	  }
	}, this._time);
	console.log("sayHi:" + this.timer);
  },
  clear: function(flag) {
	console.log("clear:" + this.timer);
	clearTimeout(this.timer);
  },
  onError: function() {
	console.log("onError:",this.timer);
	this.clear();
	this._callback && this._callback();
  }
};

let uri = "ws://localhost:8080";
var ws = new WebSocket(uri);
ws.onopen = (event) => {
  console.log('ws onopen', event);
  showLog("go~");
  MsgBegin && MsgBegin();
  heartCheck.init(ws, ()=>{
	console.log("reconnect...");
	ws = new WebSocket(uri);
  });
};
ws.onmessage = (event) => {
  console.log('ws onmessage1', event,ws.readyState);
  showLog(event.data);
  heartCheck.sayHi();
};
ws.onclose = (event) => {
  console.log("ws close", event, ws);
  heartCheck.clear();
};
ws.onerror = (event) => {
  console.log("ws error", event, ws);
  heartCheck.onError();
};
let MsgBegin = ()=>{
  let n = Math.random() * 100000;
  setTimeout(()=>{
	if(1 == ws.readyState){
	  ws.send("send:" + n);
	}
	MsgBegin();
  },n);
}
```
再来看服务端打印

  websocket server is listening...
  server: receive connection.
  server: received: send:4723.126142686907
  server: received: check : 4
  server: received: check : 5
  server: received: send:29661.621846562404
  server: received: check : 8
  server: received: check : 9
  server: received: check : 10
  server: received: check : 11
  server: received: check : 12
  server: received: check : 13
  server: received: check : 14
  server: received: check : 15
  server: received: send:97094.08453462382
  server: received: check : 16
  server: received: send:26387.016092298032

好像可以了哦，本例代码在 [Github](https://github.com/think2cat/practice/tree/master/WebSocket)
有空闲了再写个断线重连的