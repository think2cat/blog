---
title: zblog文章归档修改
tags:
  - asp
  - blog
  - zblog
  - 修改
id: 1004
categories:
  - Web
date: 2008-02-18 17:26:12
---

![](/blog/upload/200802181731042353.gif)

默认是按月划分的，时间久了就一大堆，占空间也影响美观，改了一下，本想用中文，发现文字很难对齐（下图），只好用英文缩写，其实用阿拉伯数字是最漂亮的，对的最整齐

![](/blog/upload/200802181731114031.gif)

修改后的效果看右侧【文章归档】

以zblog 1.8(080201)为例，修改如下，打开 <span style="color: rgb(255,0,0)">function/c_system_event.asp</span>，搜索 **Function BlogReBuild_Archives()**，完整代码如下，需根据自己使用的样式对代码进行修改
<textarea class="vb:firstline[1092]" rows="10" cols="100" name="code">Function BlogReBuild_Archives()
	Dim firstTime,lastTime
	Dim sql,objRS,objStream
	sql = "SELECT [log_PostTime] FROM [blog_Article] WHERE ([log_Level]>1) ORDER BY [log_PostTime]"
	Set objRS = Server.CreateObject("adodb.recordset")
	objRS.open sql,objConn,1,1
	If (Not objRS.bof) And (Not objRS.eof) Then
		firstTime = objRS("log_PostTime")
		objRS.movelast
		lastTime = objRS("log_PostTime")
	End if
	objRS.Close
	Set objRS=Nothing

	Dim i,j,strArchives
	For i = Year(lastTime) To Year(firstTime) step -1
		strArchives = strArchives & "<li><span class=""year"">" & i & "</span>
" & vbCrlf
		For j = 1 To 12
			If i>= Year(Now()) And j > Month(Now()) Then Exit For
			If i = Year(firstTime) And  j < Month(firstTime) Then j = Month(firstTime)

			Set objRS=objConn.Execute("SELECT COUNT([log_ID]) FROM [blog_Article] WHERE ([log_Level]>1) AND year(log_PostTime) = " & i & " and month(log_PostTime) = " & j)

			If (Not objRS.bof) And (Not objRS.eof) Then
				If objRS(0) > 0 then

					If ZC_MOONSOFT_PLUGIN_ENABLE=True Then
						strArchives = strArchives & "[" & Left(ZVA_Month(j),3) & ".]() "
						Call BuildCategory(Empty,Empty,Empty,i & "-" & j,Empty,ZC_DISPLAY_MODE_ALL,ZC_STATIC_DIRECTORY,i & "_" & j& "." & ZC_STATIC_TYPE)
					Else
						strArchives = strArchives & "[" & Left(ZVA_Month(j),3) & ".]() "
					End If
				Else
					strArchives = strArchives & "<span>" & Left(ZVA_Month(j),3) & ".</span> "
				End If
			End If

			objRS.Close
			Set objRS=Nothing
		next
		strArchives = strArchives & "</li>" & vbCrlf
	Next

	strArchives=TransferHTML(strArchives,"[no-asp]")

	Call SaveToFile(BlogPath & "/include/archives.asp",strArchives,"utf-8",True)

	BlogReBuild_Archives=True

End Function</textarea>