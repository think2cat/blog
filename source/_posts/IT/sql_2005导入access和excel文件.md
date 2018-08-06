---
title: SQL 2005导入Access和Excel文件
tags:
  - sql
id: 902
categories:
  - IT
abbrlink: 208d647d
date: 2006-11-03 01:03:43
---

晚上在给SQL 2005导入Access数据库，语句如下

```sql
INSERT INTO [dbo].[ixunlei]......
 SELECT ......

 FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0','Data Source="....mdb"')...[.....]
 ```

然后提示出错

    SQL Server 阻止了对组件 'Ad Hoc Distributed Queries' 的 STATEMENT'OpenRowset/OpenDatasource' 的访问，因为此组件已作为此服务器安全配置的一部分而被关闭。系统管理员可以通过使用 sp_configure 启用 'Ad Hoc Distributed Queries'。有关启用 'Ad Hoc Distributed Queries' 的详细信息，请参阅 SQL Server 联机丛书中的 "外围应用配置器"。

因为SQL2005默认是没有开启 **'Ad Hoc Distributed Queries'** 组件，开启方法如下

```sql
EXEC sp_configure 'show advanced options', 1
GO

RECONFIGURE

GO

EXEC sp_configure 'Ad Hoc Distributed Queries', 1

GO

RECONFIGURE

GO
```

接着就可以导入Access了，导入Excel也一样需要开启