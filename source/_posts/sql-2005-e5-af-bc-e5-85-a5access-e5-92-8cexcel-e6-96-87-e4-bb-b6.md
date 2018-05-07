---
title: SQL 2005导入Access和Excel文件
tags:
  - sql
id: 902
categories:
  - IT
date: 2006-11-03 01:03:43
---

晚上在给SQL 2005导入Access数据库，语句如下
<div style="border: 1px solid rgb(138, 138, 138); margin: 1px; padding: 6px; overflow: auto; font-size: 12px; font-family: Courier New; background-color: rgb(238, 238, 238);"><span style="color: rgb(0, 0, 255);">INSERT</span>&nbsp;<span style="color: rgb(0, 0, 255);">INTO</span>&nbsp;[dbo].[ixunlei]......&nbsp; 

&nbsp;<span style="color: rgb(0, 0, 255);">SELECT</span>&nbsp;......&nbsp; 

&nbsp;<span style="color: rgb(0, 0, 255);">FROM</span>&nbsp;OPENDATASOURCE(<span style="color: rgb(255, 0, 255);">'Microsoft.Jet.OLEDB.4.0'</span>,<span style="color: rgb(255, 0, 255);">'Data&nbsp;Source=&quot;....mdb&quot;'</span>)...[.....] 

</div>

然后提示出错
<div style="border: 1px solid rgb(138, 138, 138); margin: 1px; padding: 6px; overflow: auto; font-size: 12px; font-family: Courier New; background-color: rgb(238, 238, 238);">SQL&nbsp;Server&nbsp;阻止了对组件&nbsp;<span style="color: rgb(255, 0, 255);">'Ad&nbsp;Hoc&nbsp;Distributed&nbsp;Queries'</span>&nbsp;的&nbsp;STATEMENT<span style="color: rgb(255, 0, 255);">'OpenRowset/OpenDatasource'</span>&nbsp;的访问，因为此组件已作为此服务器安全配置的一部分而被关闭。系统管理员可以通过使用&nbsp;sp_configure&nbsp;启用&nbsp;<span style="color: rgb(255, 0, 255);">'Ad&nbsp;Hoc&nbsp;Distributed&nbsp;Queries'</span>。有关启用&nbsp;<span style="color: rgb(255, 0, 255);">'Ad&nbsp;Hoc&nbsp;Distributed&nbsp;Queries'</span>&nbsp;的详细信息，请参阅&nbsp;SQL&nbsp;Server&nbsp;联机丛书中的&nbsp;&quot;外围应用配置器&quot;。&nbsp;</div>

因为SQL2005默认是没有开启<font face="Verdana"><font color="#ff0000">**'Ad Hoc Distributed Queries'**</font> 组件，开启方法如下</font>
<div style="border: 1px solid rgb(138, 138, 138); margin: 1px; padding: 6px; overflow: auto; font-size: 12px; font-family: Courier New; background-color: rgb(238, 238, 238);">EXEC&nbsp;sp_configure&nbsp;<span style="color: rgb(255, 0, 255);">'show&nbsp;advanced&nbsp;options'</span>,&nbsp;1 

GO 

RECONFIGURE 

GO 

EXEC&nbsp;sp_configure&nbsp;<span style="color: rgb(255, 0, 255);">'Ad&nbsp;Hoc&nbsp;Distributed&nbsp;Queries'</span>,&nbsp;1 

GO 

RECONFIGURE 

GO</div>

接着就可以导入Access了，导入Excel也一样需要开启