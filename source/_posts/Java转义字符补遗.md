---
title: Java转义字符补遗
tags:
  - java
  - jdk
  - tomcat
  - 技巧
  - 转义
id: 916
categories:
  - Web
abbrlink: 93538d04
date: 2007-02-05 11:41:35
---

常见的Java的转义字符，如下，在大部分文章都提到了

\n 回车(\u000a) 

\t 水平制表符(\u0009) 

\b 空格(\u0008) 

\r 换行(\u000d) 

\f 换页(\u000c) 

\' 单引号(\u0027) 

\&quot; 双引号(\u0022) 

\\ 反斜杠(\u005c) 

\ddd 三位八进制 

\udddd 四位十六进制

上周发现还有其它需要转义的字符，例如

<font color="#ff6600"><font color="#000080">String sName = &quot;Java转义字符(补遗)&quot;;

sName = sName.replaceFirst(&quot;(补遗)&quot;,&quot;&quot;);

out.println(sName);</font>

</font>

如果你以为会输出&ldquo;<font color="#ff0000">Java转义字符</font>&rdquo;，那你就错了，事实上输出&ldquo;<font color="#ff0000">Java转义字符()</font>&rdquo;，我也很奇怪，以为是中英文括号的问题，可是并不是，我不确定是否转义问题，解决方法是

<font color="#000080">sName = sName.replaceFirst(&quot;\\(补遗\\)&quot;,&quot;&quot;);</font>

以上测试环境为 jdk 1.5.0.06 + tomcat 5.5.9