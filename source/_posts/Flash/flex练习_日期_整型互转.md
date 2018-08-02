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


<embed width="600" height="384" tplayername="SWF" splayername="SWF" type="application/x-shockwave-flash" src="/images/2009/07/22_12787.swf" mediawrapchecked="true" name="changeTime" allowscriptaccess="sameDomain" bgcolor="#869ca7" quality="high" pluginspage="http://www.macromedia.com/go/getflashplayer" id="changeTime"></embed>

公司的数据库，日期多是用整型（即距离1970-1-1的秒数），查看搜索啥的都要换成一般格式，就做了这么个小东西

日期转换还挺麻烦的，用的是比较笨的方法，例如 2006-2-9 23:35:56 ，用 - 切出日期，再用 : 切出时间，再set到日期对象去

这边有更好的方法，[http://www.digitalpersonae.com/blog/2009/02/01/mysql-actionscript-date-formatter/](http://www.digitalpersonae.com/blog/2009/02/01/mysql-actionscript-date-formatter/)

里面用到Lite3写的一个字符串处理的类，他的博客 [http://www.lite3.cn/](http://www.lite3.cn/)

~~[源文件打包下载](/upload/2009/7/200907222132150351.7z)~~
<!--more-->
```xml
<?xml version=”1.0” encoding=”utf-8”?>
<br>
	<mx:Application xmlns:mx=”<a href="http://www.adobe.com/2006/mxml"" target="_blank" rel="noopener">http://www.adobe.com/2006/mxml"</a> layout=”absolute” width=”600” height=”384”><br>
				<mx:TextArea id=”text1” x=”10” y=”73” width=”224” height=”301” change=”doit()” borderThickness=”2” borderColor=”#00ACAE”>
					<br>
					</mx:TextArea>
					<br>
						<mx:TextArea id=”text2” x=”360” y=”73” width=”224” height=”301” editable=”false”/>
						<br>
							<mx:Button x=”242” y=”155” label=”<== 清空 ==>” width=”110” height=”74” fontFamily=”Arial” fontSize=”12” click=”text1.text=’’;text2.text=’’”/><br>
											<a href="mx:Script" target="_blank" rel="noopener">mx:Script</a>
											<br>
												<![CDATA[<br>            import mx.formatters.DateFormatter;<br>            import cn.StringUtil;<br>            import mx.effects.easing.Exponential;<br>            function doit():void{<br>                text2.text = “”;<br>                var str:Array = text1.text.split(“\r”);<br>                for(var i:int=0;i<str.length;i++){<br>                    str[i] = cn.StringUtil.trim(str[i]);<br>                    var myDate:Date = new Date();<br>                    try{<br>                        if(str[i].indexOf(“-“) != -1){<br>                            str[i] = cn.StringUtil.replace(str[i],” “,”,”);<br>                            str[i] = cn.StringUtil.replace(str[i],”-“,”,”);<br>                            str[i] = cn.StringUtil.replace(str[i],”:”,”,”);<br>                            var strArr:Array = new Array();<br>                            strArr = str[i].split(“,”);<br>                            if(str[i].toString().indexOf(“NaN”) == -1 && strArr.length>=3){<br>                                for(var j:int=0;j<strArr.length;j++){<br>                                    strArr[j] = Number(strArr[j]);<br>                                    if(strArr[j].toString().indexOf(“NaN”) == -1){<br>                                        if(j==0){<br>                                            myDate.setFullYear(strArr[j]);<br>                                        }else if(j==1){<br>                                            myDate.setMonth(strArr[j]-1);<br>                                        }else if(j==2){<br>                                            myDate.setDate(strArr[j]);<br>                                        }else if(j==3){<br>                                            myDate.setHours(strArr[j]);<br>                                        }else if(j==4){<br>                                            myDate.setMinutes(strArr[j]);<br>                                        }else if(j==5){<br>                                            myDate.setSeconds(strArr[j]);<br>                                        }<br>                                    }<br>                                }<br>                                text2.text += int(myDate.getTime()/1000);<br>                            }<br>                        }else if(str[i] !=””){<br>                            str[i] = Number(str[i]);<br>                            if(str[i].toString().indexOf(“NaN”) == -1){<br>                                myDate.setTime(str[i]*1000);<br>                                text2.text += myDate.getFullYear() + “-“ + (myDate.getMonth()+1) + “-“ + myDate.getDate() + “ “ + myDate.getHours() + “:” + myDate.getMinutes() + “:” + myDate.getSeconds()<br>                            }<br>                        }<br>                    }catch(err:Error){<br>                        text2.text += err.toString();<br>                    }<br>                    text2.text += “\n”;<br>                }<br>            }<br>        ]]>
												<br>
												</mx:Script>
												<br>
													<mx:Text x=”37” y=”10” text=”== 日期 <=> 整型 互转 ==

输入日期（格式为 2004-7-20 13:29:4），或者秒数（距离1970年1月1日以来的秒数）” width=”505” height=”55” fontSize=”12” color=”#FFFFFF” textAlign=”left”/><br>
																</mx:Application>
```