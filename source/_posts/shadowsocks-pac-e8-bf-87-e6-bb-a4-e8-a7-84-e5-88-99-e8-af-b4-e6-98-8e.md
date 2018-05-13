---
title: ShadowSocks PAC过滤规则说明
tags:
  - PAC
  - ShadowSocks
id: 1855
categories:
  - Web
abbrlink: 7868958c
date: 2017-12-07 14:48:48
---
SS小飞机文件夹有个pac.txt，打开会发现是一个JS格式的文件，这就是自动代理配置文件
PAC全称Proxy Auto Config File，用于根据URL选择不同代理服务器或是不代理直接访问，相比于传统代理可以加快速度，也减少代理服务器的流量

![](/images/2017/12/pac.png)
<!-- more -->
PAC最早是Ad Block使用的一种规则，用于屏蔽网页各种广告，所以参数除了URL外还支持各种DOM元素，详细参数看这里 https://adblockplus.org/en/filter-cheatsheet
SS小飞机只用了URI部分的规则，完整文件在根目录下pac.txt，如果需要增加规则，则修改user-rule.txt，然后SS就会自动跟GFW合并生成pac.txt
user-rule.txt格式每行一个规则，! 开头表示注释

1.  支持通配符，比如 *.21ido.com，但这句其实用 .21ido.com 即可
2.  || 协议通杀， ||.21ido.com 匹配 ftp://21ido.com，http://21ido.com，https://21ido.com
3.  @@ 例外，比如上面的||匹配了所有21ido.com，如果其中blog.21ido.com不希望被匹配，则加一行 @@blog.21ido.com
4.  | 开头或结束，上面的例外规则，如果遇到 blog.21ido.com.cn其实还是会被例外放行，如果不希望.com外的域名也被匹配到，改为 @@blog.21ido.com|，同理也可以放在开头如@@|blog.21ido.com|
5.  ^ 标记分隔符，跟结束符 | 相似，但^表示非字母数字及_ - . %外的字符，比如 21ido.com^ 除了匹配 21ido.com/ 外，还可以匹配 21ido.com:8080/ 或是 21ido.com?t=2017
6.  正则，斜杠/开头并斜杠/结束，比如/^https?:\\/\\/[^\\/]+blogspot\\.(.*)/