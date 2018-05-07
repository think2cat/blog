---
title: 《ActionScript 3殿堂之路》学习笔记(1)
tags:
  - actionscript
  - Adobe
  - book
  - flash
  - flex
  - Macromedia
  - xml
  - 书
  - 笔记
id: 1047
categories:
  - Flash
date: 2008-09-04 09:24:33
---

作者：SK猫
创建时间：2008年9月2日
最后修改时间：2008年9月3日 10:18:58 PM

&nbsp;

<span style="font-size: large">一、ActionScript 3 语言介绍</span>
&nbsp;

ActionScript 3基本是ActionScript引擎的完全重写，代码执行效率最快可以比原有快10倍。
AVM2（ActionScript Virutal Machine 2）支持AS3，并向前兼容。
&nbsp;

<span style="font-size: medium">1.新特性</span>

1.  运行时异常处理机制
2.  运行时类型
3.  密封类
4.  闭包方法
5.  使用E4X理论处理XML数据
6.  正则表达式
7.  命名空间
8.  新基元数据类型

&nbsp;

<span style="font-size: medium">2.AS3开发工具</span>

1.  Flash CS3
2.  Flex 2、Flex 3、Flex SDK

&nbsp;

<span style="font-size: medium">3.编译</span>
&nbsp;

AS3被编译成ActionScript bytecode，简称ABC文件，ABC文件放入SWF方可被Flash Player执行。SWF是Flash文件格式，容纳媒体资源和ABC字节码。
Flash CS3源文件后缀名为 .fla，Flex Builder使用了MXML语言

&nbsp;

<span style="font-size: large">二、ActionScript 3 基本元素
</span>

<span style="font-size: medium">1.AS3中的数据类型</span>
&nbsp;

*   基本数据类型
    Boolean、int、Numbers、String、uint
*   复杂数据类型
    Array、Date、Error、Function、RegExp、XML、XMLList、自定义类

&nbsp;

<span style="font-size: medium">2.变量命名规则
</span>

1.  使用有含义的英文单词作为变量名
2.  采用骆驼式命名法
3.  命名符合 min-length &amp;&amp; max-information 原则
4.  尽量避免变量名出现数字符号

<span style="font-size: medium">3.值类型和引用类型</span>
&nbsp;

基本类型都是值类型，其余则为引用类型。值类型不用new来创建，必须用new创建的为引用类型。
AS3变量本身是不能持有值的，值类型变量持有的是指向值类型数据的引用，引用类型变量持有的是指向引用类型数据的引用。
不论值类型数据还是引用类型数据，实质都是对象。
&nbsp;

<span style="font-size: medium">4.使用int、uint、Number注意事项</span>

1.  整数值有正负之分时，使用int，只处理正整数或颜色相关数值时，使用uint
2.  有小数点时使用Number
3.  当心整型数值的边界
4.  小数相加不一定能得到证书，可以用Math.round()修正
5.  不要让数值差距过大的浮点数相加减，结果可能有偏差

&nbsp;

<span style="font-size: medium">5.运算符</span>

*   赋值运算符：=
*   算术运算符：加、减、乘、除、模运算、求反运算
*   算术赋值运算符：+=、-=、*=、/=、%=
*   关系运算符：==、!=、===、!==
*   关系运算符：&gt;=、&lt;=、&gt;、&lt;
*   逻辑运算符：&amp;&amp;、||、!
*   三元if-else运算符：?:
*   typeof、is、as（is返回布尔值，as直接返回值）
*   in
    &nbsp;