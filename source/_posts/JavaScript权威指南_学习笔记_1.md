---
title: 《JavaScript权威指南》学习笔记(1)
tags:
  - javascript
  - 笔记
id: 884
categories:
  - Web
abbrlink: d5ef69c5
date: 2006-04-10 00:32:30
---

![](/images/2006/04/10_2006-4-410230953_12728.gif) 

一直很想系统的学习下JS，这次有幸在公司借到本**《JavaScript权威指南(第四版)》**，看了1天，还不错，但借的终究要还，好书还是希望自己手头有的好，决定自己去买一本。原价99大洋，打折后也要七、八十，真的蛮贵，相比之下，第三版就便宜多了，淘宝上三十多块就可以买了，比第四版便宜了50大洋，不过正好明天发工资，干脆买第四版好了，毕竟<font color="#ff0000">**书中自有黄金屋**</font>，知识带来的价值是远大于书本身的价格的。

### 一、概述

JavaScript是一种轻型的、解析型语言，具有面向对象能力。最先由Netscape开发，而并非SUN公司，但JavaScript和Java在语法上有很多相似之处。

JavaScript最新版是1.5，解析器有Mozilla组织维护，其中一个版本使用C语言编写，称为SpiderMonkey，另一个用Java编写，称为Rhino

#### 1.主要特性

1.  控制文档外观和内容
2.  对浏览器控制
3.  与表单数据交互
4.  与用户交互（通过定义事件例如submit、onclick）
5.  读写cookie数据

#### 2.安全性

JavaScript本身是无法读本地文件和执行网络操作，但可以利用XPCOM进行操作
<!--more-->
### 二、语法结构

1.  **编码**
    JavaScript程序是用16位的Unicode字符集编写，可以表示地球上每一种书面语言
1.  **大小写敏感**
    JavaScript和Java都是区分大小写的语言，例如HTML里面通常用onClick，但在JavaScript里面只能用onclick
3.  **空白符**
    JavaScript会忽略程序中记号之间的空格、制表符和换行符，除非是字符串或正则表达式的一部分，但为了使代码容易阅读和理解，请合理使用制表符和换行符
4.  **句末分号**
    跟C、C++、Java一样，JavaScript在语句末尾通常带有分号&rdquo;;&rdquo;，主要为了分割语句，但在JavaScript，如果语句位于不同行，分号可以省略，但推荐养成句末加分号的编程习惯
5.  **注释**
    JavaScript跟C、C++、Java一样，支持使用&rdquo;//&rdquo;作为注释标记，也可以用&rdquo;/*&rdquo;和&rdquo;*/&rdquo;注释多行文本，但不能嵌套注释。
6.  **识标符**
    用来命名变量和函数，或是某些循环的标签。识标符第一个字符必须使字母或下划线，在JavaScript1.1版本后可以使用 $ 作为第一个字符，但尽量避免使用美元字符，接下来的字符可以是字母、数字、下划线或美元符号
7.  **保留字**
    以下的保留字不可以用作变量，函数名，对象名等，其中有的保留字是为以后 JavaScrip 扩展用的
    ```js
    break delete function return typeof
    case do if switch var
    catch else in this void
    continue false instanceof throw while
    debugger finally new true with
    default for null try
    ```

    JavaScript还有一些未来保留字，这些字虽然现在没有用到JavaScrip语言中，但是将来有可能用到

    **JavaScrip未来保留字列表：**
    ```js
    abstract double goto native static
    boolean enum implements package super
    byte export import private synchronized
    char extends int protected throws
    class final interface public transient
    const float long short volatile
    ```

    此外，每个特定JavaScript嵌入的客户端或服务器端会有自己的全局属性，也不能作为识标符。

    **其它避免使用的识标符**
    ```js
    arguments encodeURI Infinity Object String
    Array Error isFinite parseFloat SyntaxError
    Boonean escape iNaN parseInt TypeError
    Date eval Math RangeError undefined
    decodeURI EvalError NaN ReferenceError unescape
    decodeURIomponent Function Number RegExp URIError
    ```

### 三、数据类型

JavaScript支持基本数据类型－数字、文本字符串和布尔值，小数据类型null和undefined，复合型数据类型－函数、对象（数组、已命名的值的无序集合）。

#### 1.数字

1.  **整型**，包括-253和253之间所有整数（包括-253和253），但是要注意JavaScript中某些整数运算（例如逐位运算符）范围从-231和231-1
2.  **八进制和十六进制**，十六进制以&rdquo;0X&rdquo;或&rdquo;0x&rdquo;开头，例如 0xfff、0XCAE3；八进制以0开头，例如0377
3.  **浮点型**，最大可以到&plusmn;1.7976931348623157x10308，最小到&plusmn;5x10-324
        可以采用传统科学记数法，例如3.14、.333，也可以使用指数记数法，例如6.02e23、1.43E-32
4.  **特殊数值**，大于浮点值所能表示范围时JavaScript输出Infinity，当负无穷大比所能表示负值还小时输出-Infinity，另外还有非数字特殊值NaN，例如一个数除与0将返回NaN，它和任何数值不相等，包括本身，可以使用isNaN()函数来检测这个值
5.  **使用方法**
    ```js
    sine_of_x = Math.sin(x);
    var x = 26;
    ```

#### 2.字符串

有单引号或双引号括起来的unicode字符序列，但字符串包括单引号或双引号时，要用反斜线符号(\)进行转移，例如&rdquo;O&rsquo;Reilly&rsquo;s&rdquo;应写为&rdquo;O\&rsquo;Reilly\&rsquo;s&rdquo;

**JavaScript 转移序列**

\0 NUL字符
\b 退格 
\f 走纸换页 
\n 换行 
\r 回车 
\t 水平制表符
\v 垂直制表符 
\' 单引号 
\&quot; 双引号 
\\ 反斜杠变量
\xXX 2位十六进制指定Lation-1字符
\uXXXX 4位十六进制指定Unicode字符

**使用方法 **

Msg = &ldquo;Hello,&rdquo; + &ldquo;world&rdquo;;

myColor = &ldquo;Blue&rdquo;

#### 3.布尔值

数据类型只有true和false，表示真或假，常用在if 判断。

有时可以看作on和off，0和1

#### 4.函数

函数是一个可执行的JavaScript代码段，定义一次就可以被多次调用执行。

JavaScript中的函数是一个真正的数据类型，所以函数可以被存储在变量、数组和对象中，还可以作为参数传递给其它函数。

#### 5.对象

对象是已命名的数据集合，数据通常作为对象的属性来引用。

例如
```js
var a = image.width;
```

对象里面包含方法，例如
```js
document.write("Hello, world");
```

#### 6.数组

JavaScript中数组下标值是从0开始的，例如a[3]表示a数组中第四个元素

使用方法：
```
var a = new Array(3); //创建3个元素的数组
a[0] = 3;
a[1] = true;
a[2] = "hello";
```

也可以这样创建
```js
var a = new Array(3, true, "hello");
```

### 四、变量

变量是一个和数值相关的名字。

#### 1.类型

在C、C++、Java中，变量只能存放所声明的数据类型，但在JavaScript中，变量是无类型的，你可以先把一个数值赋给变量，然后再把字符串赋给它，这是合法的。

基本类型和引用类型，&hellip;&hellip;

#### 2.声明

使用一个变量之前必须先声明，如果没有指定初始值那变量初始值是undefined，重复使用var声明同一个变量是合法的。

如果给一个未声明的变量赋值，JavaScript会隐式声明该变量，并且该变量会被创建为全局变量，但如果程序读取一个未声明的变量，将会报错。