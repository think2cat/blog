title: WebSocket入门（2）API使用
tags:
  - http
  - socket
  - tcp
id: 1904
categories:
  - Web
date: 2018-03-30 17:19:47
---
Websocket有2种URI格式，ws:// 和 wss://，端口分别为80和443，类似 http:// 和 https://

看个demo

↓↓↓ 客户端 ↓↓↓

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>WebSocket</title>
<meta content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" name="viewport">
<script>
var showLog = (str) => {
	let dom = document.getElementById("container");
	let tmp = dom.innerHTML;
	tmp = str + "<br />" + tmp;
	dom.innerHTML = tmp;
};
var ws = new WebSocket('ws://localhost:8080');
ws.onopen = (event) => {
	console.log('ws onopen', event);
	ws.send('from client: hello');
};
ws.onmessage = (event) => {
	console.log('ws onmessage');
	console.log('from server: ', event);
	showLog(event.data);
};
ws.onclose = (event) => {
	console.log("close", event);
};
ws.onerror = (event) => {
	console.log("error", event);
};

</script>
</head>

<body>
<div id="container"></div>
</body>
</html>
```
<!-- more -->
↓↓↓ 服务端 ↓↓
```javascript
var app = require('express')();
var server = require('http').Server(app);
var WebSocket = require('ws');

var wss = new WebSocket.Server({port: 8080});

wss.on('connection', function connection(ws) {
	console.log('server: receive connection.');

	ws.on('message', function incoming(message) {
		console.log('server: received: %s', message);
	});

	setInterval(() => {
		ws.send('world: ' + new Date());
	}, 5000);
});

app.get('/', function (req, res) {
	res.sendfile(__dirname + '/index.html');
});

app.listen(3000);
```
如果不启动服务端，打开客户端后就能看到error错误信息，因为握手失败了

![](/images/2018/03/websocket2_1.png)

启动服务端再来看下http请求
```
GET ws://localhost:8080/ HTTP/1.1
Host: localhost:8080
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Origin: file://
Sec-WebSocket-Version: 13
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3355.4 Safari/537.36
DNT: 1
Accept-Encoding: gzip, deflate, br
Accept-Language: en,zh-CN;q=0.9,zh;q=0.8,en-US;q=0.7,pt-PT;q=0.6,pt;q=0.5,ko;q=0.4
Cookie: JSESSIONID=f0ce5da6-557e-447f-b0a8-3563d771c6f3
Sec-WebSocket-Key: n9l56FSbQi57pkACi36qfg==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```

因为websocket前期握手还是基于http协议，所以头也差不多，其中connection和upgrade为固定参数，用于通知服务端这是websocket请求，其中的key为生成的随机标识，而extensions为扩展字段非必须
服务端收到请求后返回101状态响应

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: 1qW7tCNMaZ33U1DCQPFVbUgudmg=
```
握手成功后，虽然端口可以不变，但是协议已经从http://切换到 ws://（或者https:// 切换到 wss://），可以相互使用websocket api发送文本或二进制数据

此时刷新下浏览器看下打印，可以看到依次触发的事件，onopen > onmessage > onclose

![](/images/2018/03/websocket2_2.png)

这是最基本的socket通讯，双方都可以用send来发送数据，特别要说的是最后的onclose，触发这个事件是因为手动把server关了，正常情况下会下发断开的通知，如果是网络问题或是服务器宕机等异常，是不会有通知，此时需要客户端发送心跳来检测连接是否断开