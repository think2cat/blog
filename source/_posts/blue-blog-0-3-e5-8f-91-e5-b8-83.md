title: Blue Blog 0.3 发布
tags:
  - blog
id: 817
categories:
  - Web
date: 2004-08-27 19:16:00
---
### Blue Blog Readme
最后更新：ver 0.3   2004-08-27 >>这天是我生日
作者：梦游的猫 (Gavin / C.A.T.)
网址：http://www.21ido.com
论坛：http://bbs.21ido.com
Blog：http://blog.21ido.com
Email：gavin2026cn[at]163.com

Blue Blog原名Gavin's Blog ，最大的特点就是跟LeadBBS结合，实现会员数据库共享，该Blog写于2003年9月，期间因为在广州考试无法上网而中止了很长时间，春节过后因网站空间被停，又停了很长时间，现在快一年了，其实很早就想公开源码，让更多人参与，共同来完善，但又怕因程序漏洞自己网站受攻击，看着别人写的Blog渐渐流传开来而自己的Blog却无人访问，自己蛮难受的。
现在正在学习XHTML，准备重新设计Blue Blog，或许需要很长一段时间，所以这个版本就公开吧。我知道存在很多bug，希望大家能一起来完善，有问题或建议请发Email给我，还有就是因为我现在不方便上网，只能访问www，无法上FTP，所以拜托请别拿我网站做测试。一旦被黑我就只能欲苦无泪了
这里推荐几个很厉害的Blog程序
Dlog : http://www.duoluo.com
Poorlish's Blog : http://www.dcshooter.com/dlog/
L-Blog : http://www.loveyuki.com/
最后，引用Poorfish的话：** BLOG最重要的不是程序，而是它的内容和背后的Blogger **

** 你可以自由传播、修改该Blog，但我希望能保留我的网页链接以及版权信息 ^_^ **
<!--more-->
---

## 如何安装？
1. 备份原有的数据库
2. 打开BlueBlog.asp文件，修改下面字段
```vbs
db = "../bbs/data/LeadBBS.mdb" '数据库位置
```
然后运行该文件，进行数据库转换
3. 打开conn.asp文件，修改下面字段
```vbs
db = db_path & "../bbs/data /BlueBBS.mdb" '数据库地址
DEF_MasterCookies = "LeadBBS" 'Cookie名称，保持与LeadBBS论坛的Cookie名称一样
DEF_BlogPath = "http://www.21ido.com/blog/" 'BlueBlog网址
DEF_LeadBBSPath = "../bbs/" 'LeadBBS论坛相对BlueBlog的路径
bfnum = 16 '笑脸图像数量，一般默认即可
DEF_UBBiconNumber = 14 '表情图像数量
```
4. 打开rss.asp 和 rss1.asp，修改>channel>字段相关属性
5. 你可以运行你的Blog了，请用站长身份登陆，对Blog进行个性设置

## 已知问题：

1. 首页截取日志显示时，UBB标记会被截断；
2. RSS解析未能通过http://feedvalidator.org/check 的验证，但可正确被RSS阅读软件识别；
3. 之前对PingBack理解错误，造成这一功能跟被误用；

> 用户权限(即荣誉):
> 0:会员:发表评论
> 1:版主:发表评论
> 2:贵宾:发表评论,发布Blog,编辑自己Blog
> 3:成员:发表评论,发布Blog,编辑自己Blog
> 4:站长:发表评论,发布Blog,编辑所以成员Blog,修改网站设置

---

## 更新记录：

* 2004-8-15
月历跟Blog日期连接上了

* 2004-01-02
修正:
    1) 后台用户Blog计数修复sub
    2) 其它小问题
增加:
    1) 全文阅读TrackBack显示; 
    2) 删除单篇评论(其实这个脚本已经写了只是网页显示没加入链接
    3) 编辑评论,因为评论表只建了一个文本的字段,之前是写入数据库时进行UBB编码,现在要考虑要不要原文写入,显示时再进行UBB

* 2003-12-01
重新取名Blue Blog
修正: 
    1) 加快页面处理速度
    2) 修正及增加UBB代码
    3) 关于图像尺寸大小的问题
    4) RSS2描述字段
增加: 
    1) 用户注册,跟论坛(LeadBBS)共享一个成员数据库
    2) 用户评论,多建一个表,效率可能会很低

* 2003-10-21
增加RSS

* 2003-10-08
重新制作界面

* 2003-09-30
完成框架设计，取名Gavin's Blog
Gavin是我的英文名

~~[点击下载](/blog/upload/2004/08/27/27_1914886220.rar)~~