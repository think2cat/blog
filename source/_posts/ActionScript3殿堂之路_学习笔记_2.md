---
title: 《ActionScript 3殿堂之路》学习笔记(2)
tags:
  - actionscript
  - Adobe
  - 书
  - flash
  - flex
  - Macromedia
  - xml
  - 书
  - 笔记
id: 1048
categories:
  - Flash
abbrlink: 85f5a119
date: 2008-09-10 09:40:49
---

作者：SK猫
创建时间：2008年9月2日
最后修改时间：2008年9月10日 8:20:51 AM

&nbsp;

<span style="font-size: large;">三、ActionScript 3 流程控制</span>
&nbsp;

<span style="font-size: medium;">1.&nbsp;条件判断</span>

判断结果只有2种：true和false，AS3中允许表达式的值不是布尔值，如果一个条件表达式的值不是布尔值，会自动执行类型转换，转换成相应布尔值
a)&nbsp;if-else
b)&nbsp;if&hellip;else if&hellip;else
&nbsp;

<span style="font-size: medium;">2.&nbsp;循环</span>

*   a)&nbsp;while
*   b)&nbsp;do-while
*   c)&nbsp;for
*   d)&nbsp;for&hellip;in 和 for each&hellip;in
    for&hellip;in输出对象成员的名字（键）
    for. each&hellip;in输出对象成员的值
*   e)&nbsp;break 和 continue
    配合在循环加标签，可退出某个子或父循环

<span style="font-size: medium;">3.&nbsp;switch</span><span style="font-size: small;">
</span>
跟其它语言一样，不加break的话，会继续执行语句
&nbsp;

&nbsp;

<span style="font-size: large;">四、&nbsp;ActionScript 3 的函数</span>
&nbsp;

&nbsp;

<span style="font-size: medium;">1.&nbsp;定义函数2种方法</span>

*   函数语句定义法 function xxx():int{}
*   函数表达式定义法 var xxx:function = function():int{}

区别在于语句定义法编译时会提升到最起码，而表达式定义法不会，如果定义之前执行不会成功，

<span style="font-size: medium;">2.&nbsp;参数</span>
&nbsp;

AS3中如果参数是基元数据类型，可以看做是传值，如果不是基元数据类型，就是传引用，函数内部的操作将直接
函数中传入的参数被保留在一个arguments数组对象，AS2可以无视函数定义传入任意多参数，AS3则不可以，但可以用新关键字&hellip;(rest)接受任意多参数，rest可以另外命名
&nbsp;

&nbsp;

<span style="font-size: medium;">3.&nbsp;函数本质</span>

ActionScrip 3 中，一切皆对象（Everything is an Object）。函数本身是Function类型的对象，一旦执行将建立一个特殊对象Active Object，该对象是不可访问的，同时每个函数都有一个内置的范围链（Scopes chain）。