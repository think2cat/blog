---
title: Less入门
tags:
  - css
  - less
categories:
  - Web
abbrlink: 8ba397ba
date: 2016-11-08 20:16:05
---
## 1) LESS 是什么
首先，CSS是什么，CSS是层叠样式表，用于描述HTML和XML样式表现的语言

Less 是一种动态的样式语言。Less扩展了CSS的动态行为，增加了变量（Variables）、混合书写模式（mixins）、操作（operations）和功能（functions）等等

## 2) 示例
```css
@bgColor: #fff;
@mainPadding: 15px;
@fontColor: #444;
@fontSize: 20px;

.main {
  background:@bgColor;
  padding: @mainPadding;
  border-top: 1px solid #d2d6de;
  p {
    margin:0;
    color: @fontColor;
    padding: @mainPadding;
    font-size: @fontSize;
      span{
        font-size: @fontSize + 10;
      }
  }
}
```
<!--more-->
经过编译后是这样子
```css
.main {
  background: #fff;
  padding: 15px;
  border-top: 1px solid #d2d6de;
}
.main p{
  margin:0;
  color : #444;
  padding: 15px;
  font-size:20px;
}
.main p span{
  font-size:30px;
}
```

## 3) 安装
LESS基于Node.js开发，需要先安装Node.js
命令行执行 `npm install less`
```cmd
C:\Users\gavin.guo>npm install less
less@2.7.1 node_modules\less
├── graceful-fs@4.1.9
├── mime@1.3.4
├── image-size@0.5.0
├── source-map@0.5.6
├── errno@0.1.4 (prr@0.0.0)
├── mkdirp@0.5.1 (minimist@0.0.8)
└── promise@7.1.1 (asap@2.0.5)

C:\Users\gavin.guo>lessc -v
lessc 2.7.1 (Less Compiler) [JavaScript]
```

## 4) 编译方式
1. 命令行 `lessc style.less > style.css`
2. 第三方工具，如Koala
3. IDE集成
4. 此外还可以通过载入less.js运行库
  `<link href="study.less" rel="stylesheet/less" type="text/css" /><script src="less.min.js" type="text/javascript"></script>`

## 5) 基本语法

### 变量 Variables
在Less中的变量实际上就是一个“常量”，因为只能被定义一次

* 变量可以是字符串 @{name}
* 变量名可以作为类名称或者属性名
* 变量可以是数组，用extract来获取某元素

```css
@bgColor: #fff;
@mainPadding: 15px;
@fontColor: #444;
@fontSize: 20px;

.main {
	background:@bgColor;
	padding: @mainPadding;
	border-top: 1px solid #d2d6de;
	color: @fontColor;
	font-size: @fontSize; 
}
```

### 混入 Mixins
混入是一种嵌套，可以把一个类里的样式嵌入另一个类
```css
.radius(@radius:5px) {
	-moz-border-radius: @radius;
	-webkit-border-radius: @radius;
	border-radius: @radius;
}
#header {
	font-size: 20px;
	.radius();
}
#footer {
	.radius(30px);
}
```

### 嵌套 Nested
模仿DOM结构进行多层嵌套
```
ul{
    list-style:none;
    li{
    	padding : 0 auto;
    	a{
    		color: blue;
    		&:hover{
    			color: red;
    		}
    	}
    	span{
    		color: #111;
    	}
    }
}
```
编译后样式如下
```css
ul{
	list-style:none;
}
ul li{
	padding : 0 auto;
}
ul li a{
	color: blue;
}
ul li a:hover{
	color: red;
}
ul li a:visited{
	color: red;
}
ul li span{
	color: #111
}
```

### 函数 Function
Less自带许多常用函数，包括字符串、转换、数学、颜色等
(点这里看更多)[http://less.bootcss.com/functions/]
```css
convert(12cm, mm);
color(red);
format-a-d: %("repetitions: %a file: %d", 1 + 2, "study.less");
lighten(@color, 10%);
darken(@color, 10%);
saturate(@color, 10%);
desaturate(@color, 10%);
fadein(@color, 10%);
fadeout(@color, 10%);
spin(@color, 10);
spin(@color, -10); 
```

此外也可以自行创建函数
```css
@color:yellow;
.size(@c){
	width: 50px;
	height: 50px;
	margin:20px;
	float:left;
	background-color:@c;
}
.test1{
	.size(lighten(@color, 20%));
}
.test2{
	.size(darken(@color, 20%));
}
.test3{
	.size(saturate(@color, 20%));
}
.test4{
	.size(desaturate(@color, 20%));
}
.test5{
	.size(fadein(@color, 20%));
}
.test6{
	.size(fadeout(@color, 20%));
}
.test7{
	.size(spin(@color, 20));
}
.test8{
	.size(spin(@color, -20)); 
}
```

### 变量作用域和命名空间
为了更好组织 CSS 或者单纯是为了更好的封装，将一些变量或者混合模块打包起来，一些属性集之后可以重复使用
```css
.news {  
    @color: red;
    .news_tab () {  
        display: block;  
        &:hover { background-color: white }  
    }  
    .news_visited () {  
        color: @color;
        width:100px;  
    }  
    .news_more () {  
        height:200px;  
    }  
}  
div {  
    .news > .news_tab;  
}  
h1 {  
    .news > .news_visited();  
} 
```