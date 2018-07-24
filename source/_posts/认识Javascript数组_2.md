title: 认识Javascript数组(2)
tags:
  - javascript
  - 笔记
id: 1090
categories:
  - Web
abbrlink: 75f744fe
date: 2009-08-10 14:44:45
---
## 2. 数组元素的操作

上面已经说过，可以通过 数组[下标] 来读写元素

下标的范围是 0 – (23<sup>2</sup>-1)，当下标是负数、浮点甚至布尔值的时候，数组会自动转换为对象类型，例如
```js
var b    = new Array();
b[2.2]    = "XXXXX";
alert(b[2.2]);  //-> XXXXX
```

此时相当于```b["2.2"]  = "XXXXX"```

### 2.1 数组的循环
```js
var a = [1,2,3,4,5,6];
for(var i =0; i<a.length; i++){
    alert(a[i]);
}
```

这是最常用的，历遍数组，代码将依次弹出1至6
<!--more-->
还有一种常用的
```js
var a = [1,2,3,4,5,6];
for(var e in a){
    alert(e);
}
```

还是依次弹出1至6，for…in是历遍对象（数组是特殊的对象）对象，用在数组上，因为数组没有属性名，所以直接输出值，这结构语句用在对象上，例如下面
```js
var a = {x:1,y:2,z:3};
for(var e in a){
    alert(e    + ":" + a[e]);
}
```

此时e取到的是属性名，即 x、y、x，而要取得值，则采用 ** 数组名[属性] **，所以 a[e] 等同于`a["x"]、a["y"]、a["z"]`

### 2.2 数组常用函数
#### concat

在现有数组后面追加数组，并返回新数组，不影响现有数组
```js
var a = [123];
var b = "sunnycat";
var c = ["www",21,"ido"];
var d = {x:3.14, y:"SK"};
var e = [1,2,3,4,[5,6,[7,8]]];
alert(a.concat(b));         // -> 123,sunnycat
alert(a);   //    -> 123
alert(b.concat(c, d));            // -> sunnycatwww,21,ido[object    Object]
alert(c.concat(b));         // -> www,21,ido,sunnycat
alert(e.concat(11,22,33).join(" # "));            // -> 1 # 2 # 3    # 4 # 5,6,7,8 # 11 # 22 # 33
```

需要注意的是只能用于数组或字符串，如果被连接（前面的a）的是数值、布尔值、对象，就会报错，字符串连接数组时，字符串会跟数组首元素拼接成新元素，而数组连接字符串则会追加新元素（这点我也不清楚原委，知情者请透露），对于数组里面包含数组、对象的，连接后保持原样

#### join

用指定间隔符连起来，把数组转为字符串
```js
var a = ['a','b','c','d','e','f','g'];
lert(a.join(","));     // -> a,b,c,d,e,f,g 相当于a.toString()
alert(a.join(" x "));  // -> a x b x c x d x e x f x g
```

这个很容易理解，但需要注意的是只转换一维数组里面，如果数组里面还有数组，将不是采用join指定的字符串接，而是采用默认的toString()，例如
```js
var a =    ['a','b','c','d','e','f','g',[11,22,33]];
alert(a.join(" * "));  // -> a * b * c * d * e * f * g *    11,22,33
```

数组里面的数组，并没用 * 连接

#### pop

删除数组最后一个元素，并返回该元素
```js
var a =    ["aa","bb","cc"];
document.write(a.pop());      // -> cc
document.write(a);               // -> aa, bb
```

如果数组为空，则返回undefined

#### push

往数组后面添加数组，并返回数组新长度
```js
var a =    ["aa","bb","cc"];
document.write(a.push("dd"));      // -> 4
document.write(a);               // -> aa,bb,cc,dd
document.write(a.push([1,2,3]));  // -> 5
document.write(a);               // -> aa,bb,cc,dd,1,2,3
```

跟concat的区别在于，concat不影响原数组，直接返回新数组，而push则直接修改原数组，返回的是数组新长度

#### sort

数组排序，先看个例子
```js
var a = [11,2,3,33445,5654,654,"asd","b"];
alert(a.sort()); // -> 11,2,3,33445,5654,654,asd,b
```

结果是不是很意外，没错，排序并不是按整型大小，而是字符串对比，就是取第一个字符的ANSI码对比，小的排前面，相同的话取第二个字符再比，如果要按整型数值比较，可以这样
```js
var a = [11,2,3,33445,5654,654];
a.sort(function(a,b) {
return a - b;
});
alert(a);   //    -> 2,3,11,654,5654,33445
```

sort()方法有个可选参数，就是代码里的function，这是个简单的例子，不可对非数字进行排序，非数字需要多做判断，这里就不多讲

#### reverse

对数组进行反排序跟，sort()一样，取第一字符ASCII值进行比较
```js
var a = [11,3,5,66,4];
alert(a.reverse());   // -> 4,66,5,3,11
```

如果数组里面还包含数组，则当为对象处理，并不会把元素解出来
```js
var a = ['a','b','c','d','e','f','g',[4,11,33]];
alert(a.reverse());   // -> 4,11,33,g,f,e,d,c,b,a
alert(a.join(" * "));  // -> 4,11,33 * g * f * e * d * c * b * a
```

按理应该是11排最后面，因为这里把 4,11,33 当做完整的对象比较，所以被排在第一位
看不明白的话，用join()串起来，就明了多

#### shift

删除数组第一个元素，并返回该元素，跟pop差不多
```js
var a =    ["aa","bb","cc"];
document.write(a.shift());     // -> aa
document.write(a);               // -> bb,cc
```

当数组为空时，返回undefined

#### unshift

跟shift相反，往数组最前面添加元素，并返回数组新长度
```js
var a =    ["aa","bb","cc"];
document.write(a.unshift(11));     // -> 4 注：IE下返回undefined
document.write(a);               // -> 11,aa,bb,cc
document.write(a.unshift([11,22]));     // -> 5
document.write(a);               // -> 11,22,11,aa,bb,cc
document.write(a.unshift("cat"));  // -> 6
document.write(a);               // -> cat,11,22,11,aa,bb,cc
```

注意该方法，在IE下将返回undefined，貌似微软的bug，我在firefox下则能正确发挥数组新长度

#### slice

返回数组片段
```js
var a = ['a','b','c','d','e','f','g'];
alert(a.slice(1,2));   // -> b
alert(a.slice(2));      // -> c,d,e,f,g
alert(a.slice(-4));    // -> d,e,f,g
alert(a.slice(-2,-6));       // -> 空
```

a.slice(1,2)，从下标为1开始，到下标为2之间的数，注意并不包括下标为2的元素
如果只有一个参数，则默认到数组最后
-4是表示倒数第4个元素，所以返回倒数的四个元素
最后一行，从倒数第2开始，因为是往后截取，所以显然取不到前面的元素，所以返回空数组，如果改成  a.slice(-6,-2) 则返回b,c,d,e

#### splice

从数组删除某片段的元素，并返回删除的元素
```js
var a = [1,2,3,4,5,6,7,8,9];
document.write(a.splice(3,2));      // -> 4,5
document.write(a);               // -> 1,2,3,6,7,8,9
document.write(a.splice(4));  // -> 7,8,9 注：IE下返回空
document.write(a);               // -> 1,2,3,6
document.write(a.splice(0,1));      // -> 1
document.write(a);               // -> 2,3,6
document.write(a.splice(1,1,["aa","bb","cc"]));    // -> 3
document.write(a);               // -> 2,aa,bb,cc,6,7,8,9
document.write(a.splice(1,2,"ee").join("#")); // -> aa,bb,cc#6
document.write(a);               // -> 2,ee,7,8,9
document.write(a.splice(1,2,"cc","aa","tt").join("#"));  // -> ee#7
document.write(a);               // -> 2,cc,aa,tt,8,9
```

注意该方法在IE下，第二个参数是必须的，不填则默认为0，例如a.splice(4)，在IE下则返回空，效果等同于a.splice(4,0)

#### toString

把数组转为字符串，不只数组，所有对象均可使用该方法
```js
var a =    [5,6,7,8,9,["A","BB"],100];
document.write(a.toString());       // -> 5,6,7,8,9,A,BB,100

var b = new Date()
document.write(b.toString());       // -> Sat Aug 8 17:08:32 UTC+0800    2009

var c = function(s){
	alert(s);
}
document.write(c.toString());       // -> function(s){ alert(s); }
```

布尔值则返回true或false，对象则返回[object objectname]
相比join()方法，join()只对一维数组进行替换，而toString()则把整个数组（不管一维还是多维）完全平面化
同时该方法可用于10进制、2进制、8进制、16进制转换，例如
```js
var a =    [5,6,7,8,9,"A","BB",100];
for(var i=0; i&lt;a.length; i++){
	document.write(a[i].toString()    + " 的二进制是 "    + a[i].toString(2) + " ，八进制是 " + a[i].toString(8) + " ，十六进制是 " + a[i].toString(16));   //    -> 4,5
}
```

输出结果
```
5 的二进制是 101 ，八进制是 5 ，十六进制是 5
6 的二进制是 110 ，八进制是 6 ，十六进制是 6
7 的二进制是 111 ，八进制是 7 ，十六进制是 7
8 的二进制是 1000 ，八进制是 10 ，十六进制是 8
9 的二进制是 1001 ，八进制是 11 ，十六进制是 9
A 的二进制是 A ，八进制是 A ，十六进制是 A
BB 的二进制是 BB ，八进制是 BB ，十六进制是 BB
100 的二进制是 1100100 ，八进制是 144 ，十六进制是 64
```

转换只能在元素进行，如果对整个数组进行转换，则原封不动返回该数组

#### toLocaleString

返回本地格式字符串，主要用在Date对象上
```
var a = new Date();
document.write(a.toString());       // -> Sat Aug 8 17:28:36 UTC+0800    2009
document.write(a.toLocaleString());     // -> 2009年8月8日 17:28:36
document.write(a.toLocaleDateString());     // -> 2009年8月8日
```

区别在于，toString()返回标准格式，toLocaleString()返回本地格式完整日期（在【控制面板】>>【区域和语言选项】，通过修改[时间]和[长日期]格式），toLocaleDateString()跟toLocaleString()一样，只是少了时间

#### valueOf

根据不同对象返回不同原始值，用于输出的话跟toString()差不多，但是toString()是返回string类型，而valueOf()是返回原对象类型
```js
var a = [1,2,3,[4,5,6,[7,8,9]]];
var b = new Date();
var c = true;
var d = function(){
	alert("sunnycat");
};
document.write(a.valueOf());       // -> 1,2,3,4,5,6,7,8,9
document.write(typeof (a.valueOf()));  // -> object
document.write(b.valueOf());       // -> 1249874470052
document.write(typeof(b.valueOf()));   // -> numberdocument.write(c.valueOf());       // -> true
document.write(typeof(c.valueOf()));   // -> boolean
document.write(d.valueOf());       // -> function () {    alert("sunnycat"); }
document.write(typeof(d.valueOf()));   // -> function
```

数组也是对象，所以typeof  (a.valueOf())返回object，返回的依然是个多维数组
```js
var a = [1,2,3,[4,5,6,[7,8,9]]];
var aa = a.valueOf();
document.write(aa[3][3][1]); // -> 8
```

Date对象返回的是距离1970年1月1日的毫秒数，
**Math**和**Error**对象没有valueOf方法