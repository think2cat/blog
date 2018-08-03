---
title: mFancy手机动画学习笔记
tags:
  - mFancy
  - mobile
  - 笔记
id: 868
categories:
  - Flash
abbrlink: a4c7a9f0
date: 2006-03-08 01:51:36
---

这是去年在公司的学习笔记，现在看来这个格式的手机动画似乎没啥用了，[Flash Lite 2](http://www.macromedia.com/devnet/devices/?promoid=CABH)已经出来很久，支持[Flash Lite Player 2](http://www.macromedia.com/cfusion/store/index.cfm?view=ols_prod&amp;store=OLS-US&amp;category=/Software/Development/StandAlones/FlashLite2)的手机也逐渐增多


最后修改：2005-8-5

#### 一、手机动画简介

mFancy是北京数码超智推出的手机上Flash播放软件，基于J2ME架构

mFlash是紫移通推出的手机Flash动画服务，其产品包括VIS播放器(Vector Image Solution)、VIS内容制作工具、VIS平台解决方案

CimPublisher是北京数码超智信息技术有限公司推出的，一款基于WIVG(Wireless Interactive Vector Graphics)技术的手机动画制作软件

数码超智 [http://www.chaotex.com/](http://www.chaotex.com/)

紫移通 [http://www.ziyitong.com/](http://www.ziyitong.com/)

#### 二、mFancy对Flash的要求


手机|Flash AS对应
-- | --
数字键0-9|数字键0-9
*号键|*号键
#号键|#号键
OK键|Enter回车键
&rarr;&larr;&uarr;&darr;方向键|&rarr;&larr;&uarr;&darr;方向键

#### 三、各类型文件说明

支持Wave、Midi、GSM、AMR格式，

其中WAV可以直接加在Flash里面，Midi、GSM、AMR需要通过&ldquo;手机动画发布工具Publisher&rdquo;与Flash进行合成

*   Midi最大支持16和弦
*   Wav采样率为44200Hz，位深16Bit，双声道或单声道
*   GSM、AMR采样率为800Hz，位深16Bit，单声道
*   SWF，Flash播放文件
*   TXT文本，卡拉OK和MTV的歌词文件
*   CIM为WIVG动画文件，即在测试机上进行测试的文件
*   CPK为产品完整打包文件，包含WIVG动画文件及作品信息（标题、分类、2张GIF截图、文字说明、作者信息、公司信息等），是最终上传到数据库的文件

#### 四、MFancy技术参数

图形技术|二维矢量图形技术，兼容光栅图象技术
--|--
实现方式|C/C++
声音支持|设备音效：MIDI/AMR/GSM/MSF&hellip;软件音效：WAV/MP3/ADPCM，效果：震动/声音与图象/文字完美同步
交互支持|完全兼容Macromedia Flash 5 Action Script及 Flahs Lite 1.1，并扩展电话拨出、短信外发等功能
反走样支持|高质量渲染，支持4&times;4反走样，充分消除边缘锯齿
文件后缀|Cim
内容文件|声音长度：30秒50KB（AMR），1分钟5K（MIDI）
动画长度|取决于内容复杂度（矢量图形技术下，文件尺寸取决于对象数目，通常，总文件尺寸控制在100K以内，必要时，可以完全控制在50K以内）
播放器尺寸|Symbian/Smartphone/Linux平台安装包220K(提供完整网络支持和脚本支持)其他平台预装，核心模块50K(提供裁减的网络支持和脚本支持
运行时最低内存需求|150K以上
CPU要求|CPU要求 20MHz以上
内容下载|1、支持MMS下载/转发（通过内嵌对象方法实现）<br>2、支持WAP下载<br>3、支持WAP Push转发<br>4、支持断点续传（OMA Download）
播放器安装|1、支持WAP下载安装，可两阶段下载及端点续传<br>2、PC端直接安装

#### 五、MFlash技术参数

项目|需求
--|--
CPU|ARM7 TDMI(26 MHZ) , 推荐 30MHz 以上
色彩|最低4,096色(12 bit)，推荐 65,000 色 (16bit) 以上
LCD 尺寸|推荐大于或等于128 X 128 像素
LCD 类型|TFT
代码尺寸|250 KB （包含组件：Raster, Vector, VOD）
内存需求|推荐大于等于350 KB
Vocoder|AMR 或 FR
MIDI|Yamaha MA 系列，General MIDI 等
存储空间|推荐至少1MB，建议大于2MB
内存分配类型|动态内存调用
WAP 相关特征|需求
传输协议|WTP-SAR
终端支持特性|建议支持 UAProf (User Agent Profile)
新MIME 类型注册|推荐支持在WAP 浏览器中注册新的MIME类型
WAP Push|建议支持WAP Push 功能
其他特征|需求
SMS + callback URL|SMS + callback URL
文件系统|EFS (Embedded File System)
TCP 连接|推荐支持TCP，用于基于TCP协议的流媒体应用

#### 六、J2ME vs. Flash Lite

内容|J2ME|Flash Lite
--|--|--
规范|基础规范（MIDP/CLDC）上相对统一，但是大量的可选包使得程序的兼容性下降。更何况各个厂商的KVM实现还有众多Bug|统一的Flash Lite Player规范
编译运行|需要用专门的Publisher进行编译|任何安装了Flash Lite Player的设备都可以播放Flash Lite文件而不需要加以编译修改
图像格式|需要人为解决分辨率问题，因此为了适应各种平台而进行的工作就变得十分繁琐了|支持矢量及SVG等矢量格式，可以适用各类分辨率的屏幕
服务器|可以与任何一种服务器技术整|需要.FlashCom或Flash Cast等服务器技术支持，目前该类服务器应用较少
客服|目前手机普遍支持K-Java程序|目前仍以native方式存于手机，即以应用程序扩展的方式，需安装一个sis文件
综合应用|比较强，特别是众多API的支持，使|不适合作复杂的应用，包括商务和娱乐方面，从安全机制，存储能力，网络连接等层面，都是比较薄弱
推广运营|并未正式运营(或刚刚开始)，收费性质使得普及速度满|基本免费，以及MM发布了SDK并与三星、诺基亚签订软件捆绑协议
