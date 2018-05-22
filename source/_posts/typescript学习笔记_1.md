title: TypeScript学习笔记（1）
author: 猫大叔
abbrlink: 1f5ada0a
tags:
  - typescript
categories:
  - Web
date: 2018-05-14 11:44:00
---
[《TypeScript入门教程》](https://github.com/xcatliu/typescript-tutorial)学习笔记


## TypeScript是什么

是JavaScript的超集
安装nodejs后执行
```cmd
npm i -g typescript
```
TypeScript是后缀名为ts的文本文件，编译ts命令为
```cnd
tsc hello.ts
```
然后就会生成对应js文件

## 数据类型

### 原始数据类型

* 布尔值
```ts
let isCat: boolean = true;
```
<!--more-->
* 数值
```ts
let catNum: number = 9;
```

* 字符串
```ts
let catName: string = "Gavin";
```
* null 和 undefined
这2个类型比较特殊，值只能是null和undefined
```ts
let n: null = null;
let u: undefined = undefined;
```
但是其它类型的值可以是null或undefined
```ts
let catAge: number = 10;
catAge = undefined;
```
* void
JS是没有Void，但TS可以有，表示没有返回值
```ts
function sayHello(): void{
    console.log("hello");
}
```
* any任意型
JS没有严格的类型规定，实际使用中可以随意转换类型
```js
let myCat = "hello";
myCat = 10;
```
这样在JS中并不会报错，因为JS会隐性类型转换，但TS严格定义了类型，使用过程是不允许改变类型的
所以有了任意值类型
```ts
let myCat: any = "hello";
myCat = 10;
myCat.name = "Gavin";
console.log(myCat);
```

### 类型推论

如果定义是没有指明类型，则自动根据值判断类型
```ts
let catAge = 10;
//等同于
let catAge: number = 10;
```
如果没有赋值，则是any类型

### 联合类型

赋予变量多种类型，同时取值可以为多种类型中一种
比如在JS函数里头，往往希望传入参数是数值，但偏偏传进一个带引号的数值'10'，于是变成字符串
在TS中则可以这么定义
```ts
let myCat: string | number = 20;
myCat = '10';
```

甚至可以指定3个类型，这还要看具体需要
```ts
let myCat: string | number | boolean = "hello";
myCat = true;
```

## 接口interface

接口是用来描述和约束一个对象的东西，说起来比较抽象
```ts
interface Cat{
    name: string;
    color: string;
}
let blackCat: Cat = {
    name: "Gavin";
    color: "black";
}
```
接口规定了2个属性和属性类型，如果增加第三个属性，比如
```ts
let blackCat: Cat = {
    name: "Gavin";
    color: "black";
    age: 10;
}
// 报错，因为age不在接口定义里面
```
可以定义可选属性
```ts
interface Cat{
    name: string;
    color: string;
    age?: number;
}
```
可以定义只读属性
这里的只读属性是指给属性赋值后只读，不可再次修改
```ts
interface Cat{
    readonly id: number;
    name: string;
    color: string;
    age?: number;
}
let myCat: Cat = {
    id: 1,
    name: "Gavin",
    color: "Black"
}
myCat.id = 2;
//(12,7): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
//报错了，只读属性不可再次赋值
```
还可以定义任意类型任意名称的属性
```ts
interface Cat{
    readonly id: number;
    name: string;
    color: string;
    age?: number;
    [propName: string]: any;
}
```