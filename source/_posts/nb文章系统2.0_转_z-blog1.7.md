---
title: NB文章系统2.0 转 Z-Blog1.7
tags:
  - asp
  - blog
  - cms
  - 转换
id: 937
categories:
  - Web
abbrlink: 4d80ea3f
date: 2007-04-10 23:38:38
---

[17google](http://www.17google.com.cn)是在05年制作的，现有文章1630篇，基本是人工整理进去的，后来因时间有限，一直搁着没更新，实在有点遗憾

今天写了个程序把数据库转为Z-Blog 1.7，以后应该会不定期更新下的

NB文章系统2.0 MSSQL数据库&nbsp;&gt;&gt; Z-Blog 1.7 Access数据库
<textarea rows="15" cols="80" name="code" class="XML"><%@LANGUAGE="VBSCRIPT" CODEPAGE="65001"%>
<%Response.Charset="UTF-8"
Session.CodePage="65001"
Response.buffer=False
server.ScriptTimeout = 999999

'NB文章系统2.0 change to Z-Blog 1.7
'梦游的猫
'建立时间 2007年4月10日 15:53:49
'最后修改 2007年4月10日 22:53:41
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="UTF-8">
<head>
	<style>
		body {FONT-FAMILY: 宋体; FONT-SIZE: 12px;padding:30px 0 0 30px;}
		#title {border:1px solid #EAEAEA;padding:10px;}
	</style>
</head>

NB文章系统2.0 change to Z-Blog 1.7

作者：梦游的猫

[梦游的猫博客](/blog/) http://www.cat-snow.com

[梦游的猫网站](/) http://www.21ido.com

<p>&nbsp;
<div id = "title"></div>

&nbsp;
<div id = "log"></div><%

db1 = "cat|pass|daName|(local)"		'NB文章系统数据库链接，Access的请参考下面设置
db2 = "data/cat.mdb"						'博客数据库地址

'进行替换，可自行定义
Function filtContent(str)
	If IsNull(str) Or str = "" Then
		filtContent = ""
	Else
		str = Replace(str,"=""/UserFiles/","=""/UPLOAD/")
		str = Replace(str,"=""/editor/UploadFile/","=""/UPLOAD/")

		Do While InStr(str,"<IFRAME") > 0
			p1 = InStr(str,"<IFRAME")
			p2 = InStr(p1,str,"</IFRAME>")
			If p2 > p1 then
				p2 = p2 + Len("</IFRAME>")
			Else
				p2 = InStr(p1,str,">")
			End If
			If p2 > p1 then
				strTemp = Mid(str,p1,p2-p1)
				str = Replace(str,strTemp,"")
			End if
		loop

		Do While InStr(str,"<SCRIPT") > 0
			p1 = InStr(str,"<SCRIPT")
			p2 = InStr(p1,str,"</SCRIPT>")
			If p2 > p1 then
				p2 = p2 + Len("</SCRIPT>")
			Else
				p2 = InStr(p1,str,">")
			End If
			If p2 > p1 then
				strTemp = Mid(str,p1,p2-p1)
				str = Replace(str,strTemp,"")
			End if
		loop
		filtContent = str
	End if
End Function

'以下部分一般不用修改
'=========================================================================
timeStart = timer
dbArr = Split(db1,"|")

showTitle "开始
"
Set conn1 = CreateObject("ADODB.Connection")
Set conn2 = CreateObject("ADODB.Connection")
If UBound(dbArr) > 0 then
	ConnStr = "Provider = Sqloledb; User ID = " & dbArr(0) & "; Password = " & dbArr(1) & "; Initial Catalog = " & dbArr(2) & "; Data Source = " & dbArr(3) & ";"
Else
	ConnStr = "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" & server.MapPath(db1)
End if
conn1.Open connstr
connstr="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" & server.MapPath(db2)
conn2.Open connstr

Set rs = Server.CreateObject("adodb.recordset")
Set rsTemp = Server.CreateObject("adodb.recordset")

'---------------------------------------------------------------转换文章
showTitle "正在转换文章。。。"

sql = "select T1.id,T2.title as className,T1.title,T1.addDate,T1.content,T1.keyword,T1.viewNum,T1.author,T1.source,T1.sourceUrl,T1.summary from NB_Content T1 inner join NB_Column T2 on T1.columnId = T2.id order by T1.adddate"
rs.open sql,conn1,1,1

sql = "select top 1 * from [blog_Article]"
rsTemp.open sql,conn2,1,3
For i = 1 To rs.recordCount
	showLog "正在添加 [" & rs("className") & "] <span style='color:blue;bold-weight:bolder;'>" & Replace(rs("title"),"&nbsp;"," ") & "</span> 。。。"
	rsTemp.Addnew
	rsTemp("log_CateID") = findCatID(rs("className"))
	rsTemp("log_AuthorID") = 1
	rsTemp("log_Level") = 4
	rsTemp("log_Title") = replace(rs("title"),"&nbsp;"," ")
	content = filtContent(rs("content"))
	summary = rs("summary")
	If IsNull(summary) or Len(summary) < 10 Then summary = Left(noHTML(content),150)
	rsTemp("log_Intro") = summary
	author = rs("author")
	source = rs("source")
	sourceUrl = rs("sourceUrl")
	If Not IsNull(source) And source <> "" Then
		If Not IsNull(sourceUrl) And sourceUrl <> "" Then
			content = "转自：[" & source & "]()

" & vbCrlf & content
		else
			content = "转自：" & source & "

" & vbCrlf & content
		End if
	End if
	If Not IsNull(author) And author <> "17google.com.cn" And author <> "" Then
		content = "作者：" & author & "
" & vbCrlf & content
	End if
	rsTemp("log_content") = content
	rsTemp("log_ip") = "127.0.0.1"
	rsTemp("log_postTime") = rs("addDate")
	rsTemp("log_viewNums") = rs("viewNum")
	rsTemp("log_Tag") = addTag(rs("keyword"))
	showLog "<span style='color:red'>完成</span>
"
	rsTemp.update
	If i Mod 20 = 0 Then Call clsLog
	rs.movenext
next
rsTemp.close
rs.close
Call clsLog
showTitle "<span style='color:red'>完成</span>
"

'---------------------------------------------------------------转换评论
showTitle "转换评论。。。"
sql = "select T1.addDate,T1.content,T1.username,T2.title,T1.ip from [NB_Review] T1 inner join [NB_Content] T2 on T1.id = T2.id order by T1.adddate"
rs.open sql,conn1,1,1
For i = 1 To rs.recordCount

	str=Replace(rs("title"),"&nbsp;"," ")
	str=replace(str,"'","''")
	showLog "正在添加 [<span style='color:blue;bold-weight:bolder;'>" & str & "</span>]。。。"
	sql = "select top 1 log_ID from [blog_Article] where log_title = '" & str & "'"

	rsTemp.open sql,conn2,1,1
	If Not rsTemp.eof Then
		blog_ID = rsTemp("log_ID")
		rsTemp.close
		sql = "select top 1 * from [blog_comment]"
		rsTemp.open sql,conn2,1,3
		rsTemp.addnew
		rsTemp("log_id") = blog_id
		rsTemp("comm_authorID") = 0
		rsTemp("comm_author") = rs("username")
		rsTemp("comm_content") = rs("content")
		rsTemp("comm_postTime") = rs("adddate")
		rsTemp("comm_IP") = rs("ip")
		rsTemp.update
		sql = "update [blog_Article] set log_commNums = log_commNums + 1 where log_ID = " & blog_ID
		conn2.execute(sql)
		showLog "<span style='color:red'>完成</span>
"
	Else
		showLog "<span style='color:red'>出错，跳过</span> " & sql & "
"
	End If
	rsTemp.close
	If i Mod 20 = 0 Then Call clsLog
	rs.movenext
next
rs.close
'Call clsLog
showTitle "<span style='color:red'>完成</span>
"

'---------------------------------------------------------------重新统计博客数据
showTitle "重新统计博客数据。。。"
sql = "select log_cateid,count(1) from [blog_article] group by log_cateid"
rs.open sql,conn2,1,1
cateCount = rs.getrows
rs.close

For i = 0 To UBound(cateCount,2)
	sql = "select cate_id,cate_name,cate_count from [blog_Category] where cate_id = " & cateCount(0,i)
	rs.open sql,conn2,1,3
	rs("cate_count") = cateCount(1,i)
	rs.update
	showLog "[" & rs("cate_name") & "]有文章数：" & cateCount(1,i) & "
"
	rs.close
next

Set rs = Nothing
showLog "

转换完成，请登录博客后台[初始化数据]
"
showTitle "<span style='color:red'>完成</span>，耗时 " & Int((timer - timeStart)*10)/10 & " 秒"
%>

<%
'----------------------------------------------------------------------------------------------------------------------------
'--
--------------------------------------------------------------------------------------------------------------------------
Function findCatID(str)
	If isnull(str) Or str = "" Then
		catID = 0
	Else
		Set rsT = server.CreateObject("adodb.recordset")
		sql = "select top 1 cate_id,cate_name from [blog_Category] where cate_name = '" & str & "'"
		rsT.open sql,conn2,1,3
		If Not rsT.eof Then
			catID = rsT("cate_id")
		Else
			rsT.addNew
			rsT("cate_name") = str
			rsT.update
			rsT.close
			showLog "创建类别 [" & str & "]。。"
			sql = "select top 1 cate_id from [blog_Category] where cate_name = '" & str & "'"
			rsT.open sql,conn2,1,1
			catID = rsT("cate_id")
		End If
		rsT.close
		Set rsT = Nothing
	End If
	findCatID = catID
End Function

Function addTag(str)
	If IsNull(str) Or Trim(str) = "" Then
		addTag = ""
	else
		str = Trim(str)
		str = Replace(str,",","|")
		str = Replace(str,"，","|")
		str = Replace(str," ","|")
		Do While InStr(str,"||") > 0
			str = Replace(str,"||","|")
		Loop
		If Left(str,1) = "|" Then str = Right(str,Len(str)-1)
		If Right(str,1) = "|" Then str = left(str,Len(str)-1)
		tagArr = Split(str,"|")
		addTag = ""
		Set rsT = server.CreateObject("Adodb.recordset")
		For iTag = 0 To UBound(tagArr)
			tagArr(iTag) = Trim(tagArr(iTag))
			sql = "select top 1 * from [blog_Tag] where tag_name = '" & tagArr(iTag) & "'"
			rsT.open sql,conn2,1,3
			If Not rsT.eof Then
				rsT("tag_count") = rsT("tag_count") + 1
				rsT.update
			Else
				showlog " [" & tagArr(iTag) & "]"
				rsT.addNew
				rsT("tag_name") = tagArr(iTag)
				rsT("tag_count") = 1
				rsT.update
				showlog "创建标签[" & tagArr(iTag) & "]"
				rsT.close
				sql = "select top 1 * from [blog_tag] where tag_name = '" & tagArr(iTag) & "' order by tag_id desc"
				rsT.open sql,conn2,1,1
			End If
			If InStr(addTag,"{" & rsT("tag_id") & "}") > 0 Then
				'skin
			else
				addTag = addTag & "{" & rsT("tag_id") & "}"
			End if
			rsT.close
		Next
		Set rsT = Nothing
		showlog addTag & "。。"
	End if
End Function

Sub showLog(str)
	response.write "<script language='javascript'>document.all.log.innerHTML += '" & Replace(Replace(str,"'","\'"),"""","\""") & "';</script>"
	'response.write str
End Sub

Sub showtitle(str)
	response.write "<script language='javascript'>document.all.title.innerHTML += '" & Replace(Replace(str,"'","\'"),"""","\""") & "';</script>"
	'response.write str
End Sub

Sub clsLog
	response.write "<script language='javascript'>document.all.log.innerHTML = '';</script>"
End Sub

function noHTML(str)
	dim re
	Set re=new RegExp
	re.IgnoreCase =true
	re.Global=True
	re.Pattern="(\<.[^\<]*\>)"
	str=re.replace(str," ")
	re.Pattern="(\<\/[^\<]*\>)"
	str=re.replace(str," ")
	nohtml=str
	set re=nothing
end function
%></textarea>