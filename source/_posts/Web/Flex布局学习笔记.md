---
title: Flex布局学习笔记
tags:
  - css
  - flex
  - html
id: 1815
categories:
  - Web
abbrlink: d15bd75e
date: 2017-08-18 16:47:05
---
[阮大的教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) | [实战篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

[Flex布局的开源库](https://github.com/Coffcer/flex-layout)

CSS传统布局基于盒子模型，依靠display + position + float属性进行定位，在一些特殊布局显得不灵活

Flex布局，容器内的元素自动成为flex item，容器基于XY轴进行布局

其容器属性有
*   flex-direction X主轴排列方向
*   flex-wrap 元素换行
*   flex-flow 上面2个的简写
*   justify-content 主轴对齐方式
*   align-items Y轴对齐方式
*   align-content 多轴线对齐方式

item属性有
*   order 排列顺序
*   flex-grow 剩余空间膨胀值
*   flex-shrink 空间不足缩小值
*   flex-basis 预分配大小值
*   flex 以上3个的简写
*   align-self 对齐方式
<!--more-->

![](/images/2017/08/1.png)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style>
			html,body{
				width:100%;
				height:100%;
				margin:0;
				padding:0;
			}
			body {
				display: flex;
				justify-content:center;
				align-items:center;
				flex-direction:column;
			}
			body > div{
				background-color: #00A6C7;
				border-radius: 10px;
				margin:10px;
				padding:10px;
				width:500px;
				align-self: center;
				text-align:center;
			}
		</style>
	</head>
	<body>
		<div>这是内容区1</div>
		<div>这是内容区2<br/>换行测试</div>
	</body>
</html>
```

![](/images/2017/08/2.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style>
			html,body{
				width:100%;
				height:100%;
				margin:0;
				padding:0;
			}
			body {
				display: flex;
				flex-wrap: wrap;
			}
			body > div{
				order: 2;
				min-width:80%;
				min-height:600px;
				flex-grow:2;
			}
			aside{
				order:1;
				border:1px solid;
				background-color:#00caff;
				width:200px;
				align-items: baseline;
			}
			header{
				border:1px solid;
				background-color:#EEE;
				width:100%;
				height:80px;
				order:0;
			}
			footer{
				order:99;
				align-self: flex-end;
				border:1px solid;
				background-color:#000;
				color:#FFF;
				height:50px;
				width:100%;
			}
		</style>
	</head>
	<body>
		<div>这是内容区</div>
		<aside>这是菜单</aside>
		<header>这是头部</header>
		<footer>这是尾部</footer>
	</body>
</html>
```

![](/images/2017/08/3.png)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style>
			html,body{
				width:100%;
				height:100%;
				margin:0;
				padding:0;
			}
			body {
				background-color: #f2f2f2;
				display: flex;
			}
			body > div{
				background-color: #00A6C7;
				border-radius: 10px;
				margin:10px;
				padding:10px;
				width:50%;
				height:50%;
				text-align:center;
			}
			div:nth-child(1){
				align-self: flex-start;
			}
			div:nth-child(2){
				align-self:center;
			}
			div:nth-child(3){
				align-self:flex-end;
			}
			div:nth-child(4){
				height:100%;
				margin:0;
				padding:0;
			}
		</style>
	</head>
	<body>
		<div>这是内容区1</div>
		<div>这是内容区2</div>
		<div>这是内容区3</div>
		<div>这是内容区4</div>
	</body>
</html>
```

![](/images/2017/08/4.png)
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<style>
			html,body{
				width:100%;
				height:100%;
				margin:0;
				padding:0;
			}
			body {
				background-color: #f2f2f2;
			}
			div > div{
				background-color: #00A6C7;
				border-radius: 10px;
				margin:10px;
				padding:20px 10px;
				text-align: center;
				flex-grow:1;
			}
			body > div {
				display: flex;
			}
		</style>
	</head>
	<body>
		<div>
			<div>1/5</div>
			<div>1/5</div>
			<div>1/5</div>
			<div>1/5</div>
			<div>1/5</div>
		</div>
		<div>
			<div>1/7</div>
			<div>1/7</div>
			<div>1/7</div>
			<div>1/7</div>
			<div>1/7</div>
			<div>1/7</div>
			<div>1/7</div>
		</div>
		<div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
			<div>1/8</div>
		</div>
		<div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
			<div>1/9</div>
		</div>
	</body>
</html>
```