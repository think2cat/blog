---
title: 《SQL Server 2000存储过程与XML编程》学习笔记
tags:
  - sql
  - xml
  - 笔记
  - 读后感
id: 1019
categories:
  - IT
abbrlink: e5b237f1
date: 2008-04-16 10:04:40
---

[![](http://otho.douban.com/mpic/s2226371.jpg)](http://www.douban.com/subject/1144051/)

SQL Server 2000存储过程与XML编程（第2版）
又名: SQL Server 2000 Stored Procedure &amp; XML Programming,Second Editon

译者: 石朝江 / 谢俊 / 陈浩奎
作者: [美]桑德里克
ISBN: 9787302077343

&nbsp;

&nbsp;

**<span style="color: rgb(0,0,255)">去年的时候拿着同事的借书证有幸一读，无奈没多久同事就去了外地，匆匆还了书取消了借书证，还没读完，有点遗憾，只留下几句笔记</span>**

创建于：2007年6月13日星期三
最后更新：2007年7月8日星期日
&nbsp;

<span style="font-size: x-large">一、&nbsp;&nbsp;&nbsp; 什么是存储过程</span>

&nbsp;

一个T-SQL语法集，被编译并存储为一个单一的数据库对象，供以后重复使用

&nbsp;

<span style="font-size: large">1.&nbsp;&nbsp;&nbsp; 组成</span>

*   头部，定义名称、输入参数和输出参数，以及其它处理选项
*   主体，包含一个或多个T-SQL语句
*   例子：
    Create Procedure prGetBookList
    &nbsp;&nbsp;&nbsp; @booktype int
    AS
    &nbsp;&nbsp;&nbsp; Select *
    &nbsp;&nbsp;&nbsp; From book
    &nbsp;&nbsp;&nbsp; Where type = @booktype

&nbsp;

<span style="font-size: large">2.&nbsp;&nbsp;&nbsp; 命名规范</span>

通常用pr开通表示存储过程，再结合动词和名次，用于描述该存储过程，例如 proGetBookList

&nbsp;

<span style="font-size: large">3.&nbsp;&nbsp;&nbsp; 限制</span>

1.  过程名称最大长度128个字符
2.  最多可以包含2100个输入参数和输出参数
3.  主体最大不超过128MB

&nbsp;

<span style="font-size: large">4.</span><span style="font-size: large">&nbsp;&nbsp;&nbsp; 功能</span>

1.  返回信息给调用者
2.  修改数据库中的数据
3.  在数据层实现业务逻辑
4.  控制数据访问权限
5.  改善系统性能
6.  降低网络流量
7.  执行其它动作和操作（比如处理电子邮件、执行各类操作系统命令和进程、管理其它的SQL Server对象）

&nbsp;

<span style="font-size: large">5.&nbsp;&nbsp;&nbsp; 执行</span>

运行任何T-SQL语句时，SQL Server都要执行以下3步

1.  解析批处理
2.  编译批处理
3.  执行批处理

与查询相比，存储过程效率更好的原因就是执行计划可以重用，因为如果执行3次查询，那么SQL Server就要3次对它进行解析、重新编译并执行，而存储过程只需重新编译一次。

执行存储过程语句为：
Ececute prGetBooklist 2
&nbsp;

如果存储过程在批处理第一条语句执行，关键字Execute可以省略，但建议使用该关键字，如以下2句是一样的
prGetBooklist 2
Ecec prGetBooklist 2
&nbsp;

&nbsp;

<span style="font-size: x-large">二、&nbsp;&nbsp;&nbsp; T-SQL基本编程结构</span>
&nbsp;

&nbsp;

<span style="font-size: large">1.&nbsp;&nbsp;&nbsp; 变量</span>

&nbsp;

**<span style="font-size: medium">局部变量</span>**

作用域位于批处理中，一个存储过程不能访问其它存储过程中定义的变量

**声明**

变量以@开头，例如 Declare @LastName varchar(50)
Declare语句可以一次声明多个变量，中间用逗号分开，例如
Declare @LastName varchar(50), @FirstName varchar(50), @BirthDate smalldatetime

**赋值**

早期SQL Server唯一赋值方法就是select，一个select语句可以同时给多个变量赋值，例如
Select @LastName = &lsquo;Kuo&rsquo;, @FirstName=&rsquo;Gavin&rsquo;, @BirthDate=&rsquo;6/13/2007&rsquo;
早期只可以使用Set语句声明游标变量，现在微软推荐使用set语句来声明变量，例如
Set @LastName = &lsquo;Kuo&rsquo;
但是Set语句一次只能声明一个变量

&nbsp;

**<span style="font-size: medium">全局变量</span>**

可以从任何位置（存储过程或批处理中）对它们进行分析。
全局变量名称以@@ 开头，它们属于系统定义的函数，不能对它们进行声明。

最重要的几个全局变量

*   @@identity
    每个表都有一个可以定义为Identity列的列，值为编号，并且在表里面是唯一的。
    该变量允许你获取当前对话中生成的最后一个标示值。例如在insert一条或若干条记录之后可以用 select @@identity获取最后一条记录的标示值
*   @@error
    返回整型值，每执行一个SQL语句后系统都会重置该值
    0表示执行成功
*   @@rowcount
    每执行一个SQL语句后系统将该值设置为语句所影响的总记录数，可以用来确认操作的成功与否
    &nbsp;

<span style="font-size: x-large">2.&nbsp;&nbsp;&nbsp; 流控制语法</span>

&nbsp;

**注释**

单行注释 --
多行注释 /*&hellip;&hellip;*/
多行注释不可相互嵌套，但可以嵌套单行注释
多行注释里面，一般开头使用 ** 表示，表示该行为注释，便于阅读

&nbsp;

**语句块Begin&hellip;end**

主要对逻辑单元进行分组，可以嵌套使用，但最好是使用缩进避免出错。

&nbsp;

<span style="font-size: medium">**If&hellip;else**</span>

根据条件值确定代码流向，else为可选，if语句可以相互嵌套
注意，if语句并没有end if结尾

&nbsp;

<span style="font-size: medium">**While&hellip;break**</span>

T-SQL唯一一个循环语句，中间可以使用break中断循环，或continue跳过下面语句重新循环，一般跟begin&hellip;end配套使用

&nbsp;

<span style="font-size: medium">**Goto**</span>

强制跳到某一label，例如
Goto label2
&hellip;
Label2&rdquo;
&hellip;

&nbsp;

<span style="font-size: medium">**Waitfor**</span>

延时，参数有两种变量
Waitfor delay &rsquo;00:01:00&rsquo;&nbsp;&nbsp;&nbsp; 表示等待1分钟后再执行下面语句，时间参数必须小于24小时
Waitfor time &rsquo;17:55&rsquo;&nbsp;&nbsp;&nbsp; 表示直到17:55才执行下面语句
&nbsp;

<span style="font-size: large">3.&nbsp;&nbsp;&nbsp; 游标</span>

&nbsp;