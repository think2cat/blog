---
title: ASP调SOAP接口注意事项
tags:
  - asp
  - soap
  - wsdl
  - xml
id: 1002
categories:
  - Web
abbrlink: a60b2e49
date: 2008-01-25 12:25:48
---

公司一个ASP网站，要调用Java写的SOAP接口，本来想用微软那个 SOAP Toolkit提交，可是一直报错（参数错误），也不明白什么问题，只好用最原始的http请求，期间碰到许多问题，做下摘记，希望能让些人少走弯路

* post过去后服务器报错，例如** tomcat报500错误 **，可能是post的编码不对，检查下
* SOAP返回
  > &lt;faultstring&gt;The endpoint reference (EPR) for the Operation not found is http://xxx.xxx.xxx.xxx/services/ScoreService and the WSA Action = null&lt;/faultstring&gt;
  那是http头没加action项，试试添加一行
  ```vbs
  objXML.setRequestHeader &quot;SOAPAction&quot;, &quot;Update&quot;
  ```
  其中 update 是对应WSDL的方法
* 提示
  > Transport level information does not match with SOAP Message namespace
  检查xmlns属性，别搞混了
  http://schemas.xmlsoap.org/soap/envelope/ 是 SOAP 1.1
  http://www.w3.org/2003/05/soap-envelope 是 SOAP 1.2


附上相关文档地址

[SOAP 1.1](http://www.w3.org/TR/2000/NOTE-SOAP-20000508/#_Toc478383490) | [SOAP 1.2](http://www.w3.org/TR/soap12-part1/ "SOAP 1.2")

[Security](http://eroomtest.cneroom.com/security.asmx) 可以参考里面post和返回的格式等