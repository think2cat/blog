---
title: TypeScript学习笔记(2)
tags:
  - typescript
  - javascript
categories:
  - Web
abbrlink: 383e290e
date: 2018-05-16 16:29:33
---
### 数组

#### []表示法
在类型定义后面加上[]即表示数组
定义的数组不允许出现其它类型，同时支持联合类型
```ts
let catArr: number[] = [1,2,3];
let catName: string[] = ["one","tow","three"];
let catColor: (string|number)[] = [1,"tow",0xFFF];
catColor.push("four");
```

#### 泛型表示法
```ts
let catArr: Array<number> = [1,2,3];
let catName: Array<string> = ["one","tow","three"];
let catColor: Array<boolean|number> = [0,1, true];
catColor.push(3);
```
<!--more-->
#### 接口描述法
前面的interface可以描述和约束对象，所以也可以描述一个数组
```ts
interface TestArray {
    [proName: number]: string;
}
let t: TestArray = ["a","b"];
t[2]="c";
```
但是当** t.push("d") **会发现，报错了
> error TS2339: Property 'push' does not exist on type 'TestArray'

因为定义的只是值类型，没有push这个方法

#### any数组
如果向往数组塞任意类型数据，就使用any类型
```ts
let catCategory: any[] = ["white", 0xfff, "small", 2000];let catCategory: any[] = ["white", 0xfff, "small", 2000];
catCategory.push({"name":"gavin"});
catCategory.push(true);
```