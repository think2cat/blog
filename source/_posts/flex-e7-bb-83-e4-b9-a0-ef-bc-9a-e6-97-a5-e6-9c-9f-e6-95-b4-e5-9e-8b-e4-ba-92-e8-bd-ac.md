---
title: Flex练习：日期  整型 互转
tags:
  - actionscript
  - flex
id: 1088
categories:
  - Flash
abbrlink: d680d32d
date: 2009-07-22 21:22:00
---

&nbsp;

<embed width="600" height="384" tplayername="SWF" splayername="SWF" type="application/x-shockwave-flash" src="/images/2009/07/22_12787.swf" mediawrapchecked="true" name="changeTime" allowscriptaccess="sameDomain" bgcolor="#869ca7" quality="high" pluginspage="http://www.macromedia.com/go/getflashplayer" id="changeTime"></embed>

公司的数据库，日期多是用整型（即距离1970-1-1的秒数），查看搜索啥的都要换成一般格式，就做了这么个小东西

日期转换还挺麻烦的，用的是比较笨的方法，例如 2006-2-9 23:35:56 ，用 - 切出日期，再用 : 切出时间，再set到日期对象去

这边有更好的方法，[http://www.digitalpersonae.com/blog/2009/02/01/mysql-actionscript-date-formatter/](http://www.digitalpersonae.com/blog/2009/02/01/mysql-actionscript-date-formatter/)

里面用到Lite3写的一个字符串处理的类，他的博客 [http://www.lite3.cn/](http://www.lite3.cn/)

[源文件打包下载](/blog/upload/2009/7/200907222132150351.7z)

<textarea name="code" cols="100" rows="10" class="xml"><?xml version="1.0" encoding="utf-8"?>
<mx:Application xmlns:mx="http://www.adobe.com/2006/mxml" layout="absolute" width="600" height="384">
	<mx:TextArea id="text1" x="10" y="73" width="224" height="301" change="doit()" borderThickness="2" borderColor="#00ACAE">
	</mx:TextArea>
	<mx:TextArea id="text2" x="360" y="73" width="224" height="301" editable="false"/>
	<mx:Button x="242" y="155" label="&lt;== 清空 ==&gt;" width="110" height="74" fontFamily="Arial" fontSize="12" click="text1.text='';text2.text=''"/>
	<mx:Script>
		<![CDATA[
			import mx.formatters.DateFormatter;
			import cn.StringUtil;
			import mx.effects.easing.Exponential;
			function doit():void{
				text2.text = "";
				var str:Array = text1.text.split("\r");
				for(var i:int=0;i<str.length;i++){
					str[i] = cn.StringUtil.trim(str[i]);
					var myDate:Date = new Date();
					try{
						if(str[i].indexOf("-") != -1){
							str[i] = cn.StringUtil.replace(str[i]," ",",");
							str[i] = cn.StringUtil.replace(str[i],"-",",");
							str[i] = cn.StringUtil.replace(str[i],":",",");
							var strArr:Array = new Array();
							strArr = str[i].split(",");
							if(str[i].toString().indexOf("NaN") == -1 && strArr.length>=3){
								for(var j:int=0;j<strArr.length;j++){
									strArr[j] = Number(strArr[j]);
									if(strArr[j].toString().indexOf("NaN") == -1){
										if(j==0){
											myDate.setFullYear(strArr[j]);
										}else if(j==1){
											myDate.setMonth(strArr[j]-1);
										}else if(j==2){
											myDate.setDate(strArr[j]);
										}else if(j==3){
											myDate.setHours(strArr[j]);
										}else if(j==4){
											myDate.setMinutes(strArr[j]);
										}else if(j==5){
											myDate.setSeconds(strArr[j]);
										}
									}
								}
								text2.text += int(myDate.getTime()/1000);
							}
						}else if(str[i] !=""){
							str[i] = Number(str[i]);
							if(str[i].toString().indexOf("NaN") == -1){
								myDate.setTime(str[i]*1000);
								text2.text += myDate.getFullYear() + "-" + (myDate.getMonth()+1) + "-" + myDate.getDate() + " " + myDate.getHours() + ":" + myDate.getMinutes() + ":" + myDate.getSeconds()
							}
						}
					}catch(err:Error){
						text2.text += err.toString();
					}
					text2.text += "\n";
				}
			}
		]]>
	</mx:Script>
	<mx:Text x="37" y="10" text="== 日期 &lt;=&gt; 整型 互转 ==&#xa;&#xa;输入日期（格式为 2004-7-20 13:29:4），或者秒数（距离1970年1月1日以来的秒数）" width="505" height="55" fontSize="12" color="#FFFFFF" textAlign="left"/>
</mx:Application>
</textarea>