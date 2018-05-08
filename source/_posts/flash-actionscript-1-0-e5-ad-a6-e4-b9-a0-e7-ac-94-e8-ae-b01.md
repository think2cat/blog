title: Flash ActionScript 1.0 学习笔记(1)
tags:
  - actionscript
  - flash
id: 839
categories:
  - Flash
date: 2005-04-29 22:35:00
---
![闪客实战](/images/2005/04/29_12750.jpg)
Luar的《闪客实战》，买了好久了，都没认真看过，现在开始学习AS1.0，虽然过时了点，当为AS2.0打基础也好

1. enterFrame 不管play还是stop，都会不停执行

2. this.loadVariables(“test.txt”);
载入变量时用 onClipEvent(data){…}事件可以判断变量是否已经载入

3. 尽量避免多个 if ,改用 if…else if … else
同事最可能匹配的条件放前面

4. 测试代码执行速度
```as
var i = 0;
var loopCount = 2000; //程序循环次数
t = getTimer();
while(i<LOOPCOUNT){
 //代码
i++;
}
trace(getTimer()-t); //0.001 s为单位
```
<!--more-->
5. _global需要FlashPlayer6或以上版本才支持
```as
_global..myVar = 1000 //定义全局变量并赋值1000
```

6. 加载到Root用loadMovie()，加载到level用loadMovieNum()
最底层是_level0，数字越大越靠上
如果加载到_level0就会取代原来的影片，同时原影片中的所有数据（变量、属性、方法、子影片、事件函数）除定义为_global的变量和函数，其它均被删除
加载到target ，是加载swf到目标影片取代原影片，原影片的数据均会被删除

7. 动态路径
```as
this.mc1._visible = 0;
this.mc0._visible = 0;
…...
this.mc100._visible = 0;

//==>

for(i=1;i&lt;=100;i++){
this["mc"+i]._visible=0;
}
```

8. random(x)产生 0～(x-1)间的随机数
从Flash5起，建议使用Math.random()代替，Math.random()产生0～0.9999…之间的随机数
Math.floor()取下限值（等于或小于指定数最接近的整数）

9. sort()默认排序是先将数组各元素第一个字符转成ASCII码再比较
```as
//数字排序专用函数
function scrtno(a,b){
if(a&gt;b){
return 1;
}else if(a<B){
 return -1;
}else{
return 0;
} 
}

//用法
foo=[50,1,5,100,10];
foo.sort(sortno);
```

10. hitTest()不支持不规则形状碰撞检查
无论影片剪辑的形状是如何不规则，hitTest()是以一个矩形包围着影片边框做检查范围的
点对不规则形状检查可用movieClip.hitTest(x,y,true)，x,y是全局座标

11. 随机产生-1和1
```as
Math.pow(-1,random(2));
```

12. 函数执行时，所有参数储存在数组arguments内，所以在函数内可以用arguments[i]来引用参数，0<I<ARGUMENTS.LENGTH

13. 当执行return后，之后的程序不再执行，函数结束

14. 将数字转换成货币格式，每千位以“,”分隔
```as
function formatno(n) {
n = String(n);
if (n.indexOf(".") != -1) {
Num = n.substring(0, n.indexOf("."));
} else {
Num = n;
}
var arr = new Array('0'), i = 0;
if (n.indexOf(",") == -1) {
while (Num&gt;0) {
arr[i] = ''+Num%1000;
Num = Math.floor(Num/1000);
i++;
}
arr = arr.reverse();
for (var i in arr) {
if (i&gt;0) {
while (arr[i].length&lt;3) {
arr[i] = '0'+arr[i];
}
}
}
} else {
trace("d");
arr[i] = Num;
}
if (n.indexOf(".") != -1) {
Dec = n.substring(n.indexOf("."));
} else {
Dec = "";
}
if (Dec.length == 0 or Number(Dec == 0)) {
arr += ".00";
} else if (Dec.length == 2) {
arr += Dec+"0";
} else {
arr += Dec;
}
delete Num, Dec, i;
return arr;
}
var1 =1234567.8;
var2 = formatno(var1);
trace(var2);
```

15. #include “gavin.as” 后面没有“;”
发布后外部as文件已经包括在swf里面，无须再上传

16. 静态对象Key,Math,Mouse,Selection不能利用原形来扩建方法

17. ScrollPane绑定的对象，无论之前什么名称，绑定之后都位tmp_mc，例如ScrollPane名称为sp，绑定myMC影片剪辑后，myMC路径为sp.tmp_mc

18. 要知道该影片剪辑的路径，用
```as
mcfc = this._parent[this._targetInstanceName]
```
将路径存储在变量mcfc中，以后mcfc调用