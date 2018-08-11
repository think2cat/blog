---
title: '[未与信任 SQL Server 连接相关联]解决方法'
id: 845
categories:
  - IT
abbrlink: 9bdb9439
date: 2005-07-28 00:08:00
tags:
---

企业管理器可以正常登陆，可是ASP页面却出错，无论怎么改用户/密码、别名，都没用，出错信息如下

> Microsoft VBScript 编译器错误 错误 '800a03f6' 缺少 'End'
>
> /iisHelp/common/500-100.asp，行242
>
> Microsoft OLE DB Provider for ODBC Drivers 错误 '80040e4d'
>
> [Microsoft][ODBC SQL Server Driver][SQL Server]用户 'sa' 登录失败。原因: 未与信任 SQL Server 连接相关联。
>
> /conn.asp，行7</DIV>

该错误产生的原因是由于SQL Server使用了"仅 Windows"的身份验证方式，因此用户无法使用SQL Server的登录帐户（如 sa ）进行连接。解决方法如下所示：

1. 在服务器端使用企业管理器，并且选择"使用 Windows 身份验证"连接上 SQL Server；
2. 展开"SQL Server组"，鼠标右键点击SQL Server服务器的名称，选择"属性"，再选择"安全性"选项卡；
3. 在"身份验证"下，选择"SQL Server和 Windows "。
4. 重新启动SQL Server服务。

![MSSQL](/images/2005/07/28_12753.gif)

在以上解决方法中，如果在第 1 步中使用"使用 Windows 身份验证"连接 SQL Server 失败，那么我们将遇到一个两难的境地：首先，服务器只允许了 Windows 的身份验证；其次，即使使用了 Windows 身份验证仍然无法连接上服务器。这种情形被形象地称之为"自己把自己锁在了门外"，因为无论用何种方式，用户均无法使用进行连接。实际上，我们可以通过修改一个注册表键值来将身份验证方式改为 SQL Server 和 Windows 混合验证，步骤如下所示：

1. 点击"开始"-"运行"，输入regedit，回车进入注册表编辑器；
2. 依次展开注册表项，浏览到以下注册表键：
`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\MSSQLServer]`
3. 在屏幕右方找到名称"LoginMode"，双击编辑双字节值；
4. 将原值从1改为2，点击"确定"；
5. 关闭注册表编辑器