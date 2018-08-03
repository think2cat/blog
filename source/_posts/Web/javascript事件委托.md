---
title: javascript事件委托
tags:
  - delegate
  - javascript
id: 1894
categories:
  - Web
abbrlink: c1495b93
date: 2018-01-17 17:43:52
---
本例代码在[Github](https://github.com/think2cat/practice/tree/master/delegate)

事件委托简单来说，就是触发事件的DOM元素本身不执行绑定的事件，而是交给父元素或上上级甚至根元素去处理

先看个例子
```html
 <div>
 	<ul>
     <li>AA <span>A1</span> <span>A2</span></li>
     <li>BB <span>B1</span> <span>B2</span></li>
     <li>CC <span>C1</span> <span>C2</span></li>
   </ul>
   <ul>
     <li>DD <span>D1</span> <span>D2</span></li>
     <li>EE <span>E1</span> <span>E2</span></li>
     <li>FF <span>F1</span> <span>F2</span></li>
   </ul>
 </div>
 ```
 <!-- more -->
假设这个列表的LI和SPAN均需要加上click事件，常规做法是在每个DOM都绑定事件，比如
```js
let liArr = document.getElementsByTagName("li");
for(let i = 0; i < liArr.length; i++){
 liArr[i].addEventListener("click",(evt)=>{
 console.log("click li");
 });
}
let spanArr = document.getElementsByTagName("span");
for(let i = 0; i < spanArr.length; i++){
 spanArr[i].addEventListener("click",(evt)=>{
 console.log("click span");
 evt.stopPropagation();
 });
}let liArr = document.getElementsByTagName("li");
for(let i = 0; i < liArr.length; i++){
	liArr[i].addEventListener("click",(evt)=>{
		console.log("click li");
	});
}
let spanArr = document.getElementsByTagName("span");
for(let i = 0; i < spanArr.length; i++){
	spanArr[i].addEventListener("click",(evt)=>{
		console.log("click span");
		evt.stopPropagation();
	});
}
```
这种做法，简单页面还挺适用，遇到DOM元素多的，或者需要有删除增加需求的，就需要不断执行绑定操作，这样一来是会消耗很多内存，二来是容易出错

这时就需要事件委托，把事件绑定在父级元素

浏览器的事件冒泡看这里，{% post_link capture-e5-92-8cbubble-ef-bc-8c-e7-ae-80-e6-98-8e-e8-a7-a3-e8-af-bbaddeventlistener-e7-9a-84-e7-ac-ac-e4-b8-89-e4-b8-aa-e5-8f-82-e6-95-b0 %}

因为LI和SPAN都在DIV里面，所以只需绑定DIV即可
```js
document.getElementsByTagName("div")[0].addEventListener("click",(evt)=>{
 	console.log(evt);
 	if("span" == evt.target.nodeName.toLocaleLowerCase()){
 		console.log("is a span", evt.target.innerText);
 	}else if("li" == evt.target.nodeName.toLocaleLowerCase()){
 		console.log("is a li", evt.target.innerText);
 	}
});
```
在参数即event事件可以找到是哪个DOM触发了事件

看起来跟一般事件一样，多了一个触发元素的判断

为此jQuery从1.4.2新增了delegate函数，但到了1.7版本就换成 on函数，用法是一样的

```$.on()``` 和 ```$.click()``` 最大的区别是执行语句之后，新增加的DOM元素，只要符合选择器，也会绑定on函数指定的事件，而click则不会

on的语法，```$.on( events [, selector ] [, data ], handler )```

选择器用于选择后代元素，所以事件的触发来自于后代元素，而非绑定on的元素

对于本例来说，改成jquery.on也没啥变化

```js
$("div").on("click", "li, span", (evt) => {
	console.log("span", evt);
	if ("span" == evt.target.nodeName.toLocaleLowerCase()) {
 		console.log("is a span", evt.target.innerText);
 	} else if ("li" == evt.target.nodeName.toLocaleLowerCase()) {
 		console.log("is a li", evt.target.innerText);
 	}
});
```