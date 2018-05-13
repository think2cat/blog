---
title: SVN学习笔记
tags:
  - cvs
  - svn
  - 笔记
  - 管理
  - 软件工程
id: 996
categories:
  - IT
abbrlink: a7642b15
date: 2008-01-08 19:21:39
---

之前公司因为VSS不能满足需求，所以打算用SVN来管理代码，老猫受命写了这篇文档，好让一些同事快速入门，还没写完。

半年过去了，公司还在用着龟速的VSS 6.0，这文档也没有写下去的必要了，还是发出来吧

&nbsp;

## SVN学习笔记

<span class="Title"></span>创建于：2007年7月25日

最后更新： 2007年8月3日 3:59 PM

### 目录

一、版本管理基础

&nbsp;&nbsp;&nbsp; 1.版本控制介绍

&nbsp;&nbsp;&nbsp; 2.常用软件

二、Subversion基础

&nbsp;&nbsp;&nbsp; 1.发展历史

&nbsp;&nbsp;&nbsp; 2.特性

&nbsp;&nbsp;&nbsp; 3.安装

&nbsp;&nbsp;&nbsp; 4.组成

&nbsp;&nbsp;&nbsp; 5.配置

三、基本操作

&nbsp;&nbsp;&nbsp; 1.检出Check Out 

&nbsp;&nbsp;&nbsp; 2.提交Commit 

&nbsp;&nbsp;&nbsp; 3.解决冲突

&nbsp;&nbsp;&nbsp; 4.标签Tag 

四、分支与合并

&nbsp;&nbsp;&nbsp; 1.版本库的概念

&nbsp;&nbsp;&nbsp; 2.什么是分支？

&nbsp;&nbsp;&nbsp; 3.使用分支

五、进阶

&nbsp;&nbsp;&nbsp; 1.以后台服务方式启动

&nbsp;&nbsp;&nbsp; 2.从CVS转换到SVN 

六、附录

&nbsp;&nbsp;&nbsp; A. 参考资源

&nbsp;&nbsp;&nbsp; B. 相关资源

&nbsp;

### &nbsp;

### 一、版本管理基础

#### 1\. 版本控制介绍

在多人协作开发软件的时候，经常出现员工A的代码被员工B覆盖，或者今天做了小修改明天却发现需要把代码改回去，版本控制正是为此而诞生的，它是软件开发团队高效协作的重要管理工具。

简单来说，版本控制服务器是一个特殊的文件服务器，不仅控制用户对文件的读取、写入等操作，更记录了每一次修改的内容。

#### 2\. 常用软件

A. **Microsoft Visual SourceSafe**，2005新特征

1.  通过 HTTP 进行远程 Web 访问2.  增强的性能和可伸缩性3.  增加的容量，数据存储增至 4 GB4.  区域性时区和语言B. **CVS**，开源的

C. **Clearcase**，IBM开发的企业级管理软件

D. 其它的还有**PVCS**、**MKS**和企业级的**CCC Harvest**

### &nbsp;

### 二、Subversion基础

#### 1\. 发展历史

Subversion是在CVS基础发展而来的，2000年的时候，CollabNet公司的协作软件采用CVS作为版本控制系统，因为CVS本身一些局限性，从而需要一个代替品，然后邀请了Karl Fogel（Open Source Development with CVS）参与开发，14个月后，2001年8月31日，新的版本管理系统Subversion诞生，开始不再用CVS进行版本管理，而使用自己管理自己了。

#### 2\. 特性

*   **目录版本化**

        CVS只能管理单个文件的更改，而Subversion可以跟踪到目录树的变更*   **真实的版本历史**

        CVS无法实现文件移动改名操作，所以但一个文件搬到另一个地方或者改名，版本号将重新编，而Subversion可以实现移动、改名等操作*   **原子提交**

        将一系列文件的修改组成一个逻辑整体，以后可以一次性还原*   **版本号元数据**

        Subversion不仅能管理文件内容，也能管理文件和目录属性（meta-data）*   **可选的网络层**

        Subversion可以作为扩展模块嵌入Apache，自身还实现了一个轻型独立的服务器软件*   **一致的数据操作**

        Subversion用了二进制差异算法描述文件的变化，所以对于文本文件和二进制文件，操作方法是一样的*   **高效的分支和标签操作**

        Subversion使用一种类似硬链接（Unix名词）的机制拷贝整个工程来对分支和标签进行操作，消耗的时间跟修改部分成正比，而不是整个文件的大小，因此这些操作的开销很小，花费时间很少*   **可扩展性**

        Subversion由一系列结构化的模块组成，具有定义良好的API，使得和其它语言互动操作性很强，而且非常方便维护*   **可以选择数据库和纯文件的版本库实现**

        版本库可以使用嵌入的数据库((DBD，Berkeley database)，也可以使用定义格式的纯文件(FSFS，Native filesystem)*   **对象链接的版本化**

        Unix用户可以在版本控制里置入对象链接，这个链接会在Unix的工作拷贝里重建，但不会在win32工作拷贝里创建。*   **可解析的输出**

        Subversion所有的命令行客户端输出的内容都经过精心的设计，适合人们读取和自动解析；脚本化也具备较高的优先级。*   **本地化信息**

        Subversion会根据本地设置使用gettext()来显示传输错误、报告和帮助信息。

#### 3\. 安装

Subversion建立在APR（Apache Portable Runtime library）可移植层上，APR是独立运行的库，所以Subversion可以运行在所有运行Apache的服务器平台上（Windows、Lunix、BSD、MacOSX、Netware&hellip;&hellip;等等）。

Subversion官网提供了各种操作系统的相应版本，可以从以下地址获得最新版本，目前最新是1.4.4，[http://subversion.tigris.org/project_packages.html](http://subversion.tigris.org/project_packages.html)

针对Windows系统，提供了GUI界面的安装包，安装时只需一路Next下去即可

这里使用的SVN客户端是TortoiseSVN-1.4.4，将SVN完全整合到资源浏览器，使用非常方便，而且是完全免费的，从官方下载 [http://tortoisesvn.net/](http://tortoisesvn.net/)

注意有32位和64位不同版本，官方网站同时提供了各种语言版下载，其中包括可爱的简体中文。

安装过程也非常简单，一路Next下去就可，安装后会提示重启电脑，重启只是为了在资源管理器显示特有图标，不重启也是可以正常使用的。

#### 4\. 组成

*   <font color="#0000ff">**Svn

        **</font>命令行客户端。*   **<font color="#0000ff">Svnversion</font>**

        报告工作拷贝状态（当前修订版本的项目）的工具。*   **<font color="#0000ff">Svnlook</font>**

        检查版本库的工具。*   **<font color="#0000ff">Svnadmin</font>**

        建立、调整和修补版本库的工具。*   **<font color="#0000ff">Svndumpfilter</font>**

        过滤Subversion版本库转储文件的工具。*   **<font color="#0000ff">mod_dav_svn</font>**

        Apache HTTP服务器的一个插件，可以让版本库在网络上可见。*   **<font color="#0000ff">svnserve</font>**

        一种单独运行的服务器，可以作为守护进程由SSH调用，另一种让版本库在网络上可见的方式。

#### 5\. 配置

##### 1) 建立版本库（Repository）
> 在CMD命令行模式下，运行
> 
> svnadmin create f:\svn\test
> 
> 即可在 F:\svn目录下建立一个名称为test的版本库
> 
> 也可以使用TortoiseSVN图形化的完成这一步：
> 
> 在目录F:\svn\下新建一文件夹test，然后右击test，选**TortoiseSVN &gt;&gt; Create Repository here...**， 然后可以选择版本库模式， 这里使用默认即可， 然后就创建了一系列目录和文件。</
> p><p><font color="#ff0000">运行命令行之前Test目录必须是空的或者尚未有此目录。
> 
> 不推荐把版本库创建在网络共享上，如果非要这样做，请使用FSFS格式，而不要用BDB格式！</font>

##### 2) 配置用户和权限
> 用记事本打开新建的test\conf目录下面的svnserve.conf 
> 
> **<font style="BACKGROUND-COLOR: #c0c0c0"># password-db = passwd</font>**
> 
> 去掉前面# 号，即改为
> 
> **<font style="BACKGROUND-COLOR: #c0c0c0">password-db = passwd</font>**
> 
> 表示用密码文件进行权限管理，对应文件名为同目录下的passwd文件
> 
> 保存，接着再用记事本打开同目录下面passwd文件
> 
> **<font style="BACKGROUND-COLOR: #c0c0c0"># harry = harryssecret
> 
> # sally = sallyssecret</font>**
> 
> 同样去掉#号，改为
> 
> **<font style="BACKGROUND-COLOR: #c0c0c0">harry = harryssecret
> 
> sally = sallyssecret</font>**
> 
> 这里表示2个用户，其中harry表示登录用户名，harryssecret表示密码，你可以自己修改
> 
> 在svnserve.conf文件打开**<font style="BACKGROUND-COLOR: #c0c0c0"># auth-access = write</font>**表示匿名用户可写数据，这不推荐！

##### 3) 运行独立服务器
> 在命令行窗口输入
> 
> svnserve -d -r f:\svn

##### 4) 初始化导入
> 在要引入的工程文件夹右击，选**TortoiseSVN &gt;&gt; Import...**
> 
> URL of repository输入&ldquo;svn://localhost/test&rdquo;，点【OK】之后会提示输入用户名密码，按照前面设置的输入即可，如果没报错，就可以看到文件哗啦哗啦的导进版本库了。
> 
> <font color="#ff0000">注意这里是在本机操作，所以地址输入svn://localhost/test，如果是远程提交的话，要换成相应IP地址，例如svn://222.111.26.26/test</font>

### &nbsp;

### 三、基本操作

#### 1\. 检出 Check Out

在开始干活前，你必须得到一个最新的工作副本，在资源管理器右击空白处，选 **TortoiseSVN &gt;&gt; checkout**

输入目录名后点OK，如果输入的目录名是不存在的，程序会自动创建。

![svn](/images/2008/01/08_12782.jpg)

#### 2\. 提交 Commit

当完成一个或若干个文件的修改后，选中要提交的文件或文件夹，右击选** TortoiseSVN &gt;&gt; Commit**

输入备注信息后点OK就可以了，为了方便日后查看，备注信息请尽量写详细。

![svn](/blog/upload/2008/1/200801081933002252.jpg)

<font color="#ff0000">有时不清楚具体修改了哪些文件，或者新增了哪些文件，又或者要提交的文件太多了，可以直接右击文件夹进行提交，TortoiseSVN将把需要提交的文件清单列出来，进行选择后再提交即可</font>

#### 3\. 解决冲突

&nbsp;

#### 4\. 标签 Tag

### &nbsp;

### 四、分支与合并

#### 1\. 版本库的概念

**Subversion没有项目的概念，只有版本库。**

![svn](/blog/upload/2008/1/200801081933044758.jpg)

版本库记录了每一次修改内容，通过版本库，可以很方便的回朔到某个以前的版本，可以很容易的查看上月某天的某人改了什么东西。

版本号初始值为0，每次成功提交后递增1，Subversion的版本号是针对整个目录树的，而不是单个文件

#### 2\. 什么是分支？

例如重庆上的算命网站代码，加入了SVN版本管理，现在新疆也要上算命业务，页面代码上基本是相同的，只有在某些接口存在差异，这时就需要新建一个分支，来对2个网站代码进行管理。

又例如IBS后台在进行升级改造，这可能需要比较长的时间，而同时又需要对平时出现的一些问题修修补补，最佳方案就是建立分支，让升级和日常维护分开来，虽然这样在升级结束后代码的合并会有些难度，但至少可以保证在升级过程中的代码不会跟日常维护的修改造成混乱。

**<font color="#ff0000">分支跟版本库的差别？</font>**

分支跟版本库本来就是不同的东西，不过估计有人看了上面那段话后会问，建立分支是不是等于把版本库重新拷贝一份，所以有了下面的思考

1.  版本库是重新复制整个文件夹，而分支用了类似Unix下硬链接的技术，只是在已存在的目录树多建一个入口，而不是全盘复制，所以不管是在磁盘空间、消耗时间付出的代价都是非常小的。2.  分支之间是相互联系的，可以把某一分支的修改合并到另一分支去，例如某天出了一个SuperSVN的东西，于是有人把本文档建立分支进行修改，于是就有SVN和SuperSVN两个分支了，改着改着发现有段话写错了，这样2个分支就都可能存在这种错误了，如果是2个版本库的话，你就要改2次了，而如果是分支，你可以把一个分支的修改合并到另一分支去，这就是分支的相互联系3.  新建版本库，意味着新的权限关系，当然，你也可以把配置文件复制一份到新的版本库4.  维护带来的问题？

#### 3\. 使用分支

##### 1) 创建分支
> 选择要创建分支的文件或文件夹，右击选TortoiseSVN &gt;&gt; Branch/Tag，在 To Url输入分支新路径

##### 2) 查看分支

### &nbsp;

### 五、进阶

#### 1\. 以后台服务方式启动

#### 2\. 从CVS转换到SVN

**本节引用《cvs2svn 把CVS档案库转换为SVN档案库》，作者yasakya**

cvs2svn工具是用来把CVS档案库转换为SVN档案库的。

1.  **安装**

        *   **python2.4**

                [http://www.python.org/download/](http://www.python.org/download/)

                说明：下载最新版本的Python for Windows的安装程序python-2.4.1.msi，按照默认的方式安装Python，假设安装目录是C:\Python。*   **cvs2svn**

                [http://cvs2svn.tigris.org/servlets/ProjectDocumentList?folderID=2976](http://cvs2svn.tigris.org/servlets/ProjectDocumentList?folderID=2976)

                说明：用WinRAR解压到任一个目录下。打开命令行窗口转到cvs2svn所在的目录先测试一下python，执行C:\python\python cvs2svn，这时候会输出cvs2svn的帮助信息。*   **UnxUtils.zip**

                [http://unxutils.sourceforge.net/](http://unxutils.sourceforge.net/)

                说明：由于cvs2svn用到了GUN sort工具，因此必须下载UnxUtils.zip，把该压缩包下的usr/local/wbin/sort.exe文件解压到cvs2svn目录中。*   **rcs57pc1.zip**

                [http://www.cs.purdue.edu/homes/trinkle/RCS/](http://www.cs.purdue.edu/homes/trinkle/RCS/)

                说明：需要用到RCS的一个工具co.exe，下载rcs57pc1.zip，把该压缩包中的bin/win32下的rcslib.dll以及co.exe这两个文件同样解压到cvs2svn目录中。2.  **使用cvs2svn把CVS档案库转换为SVN档案库**> 接下来我们开始转换资源库，输入以下命令
> 
> C:\Python\python cvs2svn &ndash;s d:\svn\repository\project1 \project1
> 
> 其中我们假设project1是原有CVS资源库中的一个项目。
> 
> 下面是在我的机器上转换完毕后cvs2svn显示详细的统计信息：
> 
> cvs2svn Statistics:
> 
> ------------------
> 
> Total CVS Files: 7
> 
> Total CVS Revisions: 7
> 
> Total Unique Tags: 0
> 
> Total Unique Branches: 0
> 
> CVS Repos Size in KB: 2261
> 
> Total SVN Commits: 2
> 
> First Revision Date: Sat Sep 03 15:05:26 2005
> 
> Last Revision Date: Sat Sep 03 15:05:27 2005
> 
> ------------------
> 
> Timings:
> 
> ------------------
> 
> pass 1: 0 seconds
> 
> pass 2: 0 seconds
> 
> pass 3: 0 seconds
> 
> pass 4: 0 seconds
> 
> pass 5: 0 seconds
> 
> pass 6: 0 seconds
> 
> pass 7: 0 seconds
> 
> pass 8: 1 second
> 
> total: 3 seconds
> 
> 转换完毕后我们用浏览器打开 http://localhost/svn/project1 即可看到SVN仓库
> 
> <font color="#ff0000">注意如果要用 --encode=gb2312，必须使用python2.4以上版本</font>

### &nbsp;

### 六、附录

#### A. 参考资源

*   **《Subversion特性》**

        [http://www.subversion.org.cn/index.php?option=com_content&amp;task=view&amp;id=72&amp;Itemid=9](http://www.subversion.org.cn/index.php?option=com_content&amp;task=view&amp;id=72&amp;Itemid=9)*   **《使用Subversion进行版本控制》中文版***   **《TortoiseSVN帮助手册中文版 1.5版》***   **《把CVS档案库转换为SVN档案库》**

        [http://www.iusesvn.com/bbs/viewthread.php?tid=245](http://www.iusesvn.com/bbs/viewthread.php?tid=245)

#### B. 相关资源

*   **Subversion**

        [http://subversion.tigris.org/](http://subversion.tigris.org/)*   **TortoiseSVN**

        [http://www.tortoisesvn.net](http://www.tortoisesvn.net)*   **SVNmanager**

        基于PHP的SVN管理工具

        [http://svnmanager.sourceforge.net/](http://svnmanager.sourceforge.net/)*   **Easysvn**

        一个开源的SVN客户端

        [http://code.google.com/p/easysvn/](http://code.google.com/p/easysvn/)