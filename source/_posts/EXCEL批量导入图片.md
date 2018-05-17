---
title: EXCEL批量导入图片
tags:
  - EXCEL
  - VBA
  - vbs
  - 宏
  - 批量
id: 918
categories:
  - Web
abbrlink: 8db6a251
date: 2007-02-26 14:15:28
---

之前在[小桥流水](http://www.521000.com/bbs/dispbbs.asp?boardID=11&amp;ID=349215)看到有人问怎样在Excel批量导入图片，随手写了个宏，没想到今天又有人问我，中午把VBA小改一下

图片用1.jpg 2.jpg 3.jpg ... 10.jpg 12.jpg依次命名

图片间隔是2张相邻图片左上角的间隔，例如图片尺寸100像素，间隔写100就刚好紧挨着

默认开始位置是以选择框所在位置，例如下图，选择框在B2，图片就从B2开始排列了

![](/images/2007/02/26_200702261437022173_12748.jpg)
<!--more-->
~~[演示下载](/upload/200702261437022175.rar)~~，解压到D盘就可以直接执行了，如果打开弹出提示窗口，是因为你Excel安全性设置高，没事，一样可以执行

VBA代码如下
```vbs
Sub Macro1()
' 宏由 CAT 录制，时间: 2007-2-7
' 批量导入图片

    Dim picPath, picWidth, picHeight, fileExt
    picPath = "D:\"     '图片存放路径
    picN = 4            '图片数量
    fileExt = ".jpg"    '图片后缀名
    picScale = 30       '图片缩放百分比，不带 %
    perPic = 2          '每行图片数量
    xWidth = 202        '图片水平间隔，即水平相邻的图片左上角间隔
    xHeight = 152       '图片垂直间隔

    Dim x, y
    x = 0
    y = 0
    For i = 1 To picN
         ActiveSheet.Pictures.Insert(picPath & i & fileExt).Select
         Selection.ShapeRange.ScaleWidth picScale / 100, msoFalse, msoScaleFromTopLeft
         Selection.ShapeRange.ScaleHeight picScale / 100, msoFalse, msoScaleFromTopLeft
         Selection.ShapeRange.IncrementLeft xWidth * x
         Selection.ShapeRange.IncrementTop xHeight * y
         If i Mod perPic = 0 Then
            x = 0
            y = y + 1
        Else
            x = x + 1
        End If
    Next
End Sub
```