---
title: NPAPI使用指南
tags:
  - npapi
  - javascript
categories:
  - Web
abbrlink: d9859d53
date: 2011-09-05 15:47:30
---
## 1.什么是NPAPI

NPAPI是基于OWB的扩展插件，相当于IE的ActiveX控件

目前可分为2类
1). 功能性插件，如播放器、e视通、bulletin
2). 数据传输类，如爱音乐、我的好友，这类插件用于http网络数据传输，类似AJAX，区别在于不会暴露请求地址，以及可对数据进行加密处理
<!--more-->
## 2.NPAPI组成

### A. 插件的组成
插件一般为后缀名 so 的库文件，一个插件有一个主so文件，同时可能附带其它文件（库或者配置文件），这跟插件本身所调用的库有关，类似java的import

### B. 插件的存放目录

1). 系统插件的
基本存放于 /vendor/lib
比如播放器、bulletin等，这类跟系统结合紧密、版本更新相对少

2). 动态加载插件
存放于widget下面的 npapi 目录，如下图
![npapi](/images/2011/09/npapi.png)

这类插件是跟widget打包在一起的，安装时也随widget一起解压，当运行widget时OWB会自动映射虚拟目录，这样npapi目录下的so就可以为系统所识别，这一过程是OWB自动实现的

so的命名规范，`{lib库名}-{widget id}-plugin.so`，例如 `libimusic-20301-plugin.so`
库名是自定义的，推荐使用模块名或跟业务相关名称，方便识别，id为申请widget时的id，必须匹配，不然无法上传

## 3.NPAPI使用

首先需要在html页面加入相应库的html代码，例如爱音乐
```html
<embed id="bboImusic" type="application/bq-imusic-plugin" width="0" height="0">
```
其中 `type` 属性为插件指定，每个插件均不同，id可自定义，调用插件接口时用到

在页面加载的时候，会自动载入库文件，然后可以用js来调用接口，例如
```js
var r = bboImusic.getChannelList(-1, 0, 50);
```
`getChannelList()` 为插件自定义方法，该方法返回爱音乐频道列表，类型为字符串，然后JS可以自行对返回进行处理

需要强调的是，因为调用的是外部接口，这些插件跟底层target版本、关联的库等使用环境都有很大关系，所以有可能会出现不兼容，甚至coredump等问题，也就是说随时可能出错
所以调用插件接口时，必须加上 `try{}catch(){}`，上面的调用必须这么写
```js
try{
  var r = bboImusic.getChannelList(-1, 0, 50);
}catch(e){
  alert(“bboImusic.getChannelList() err:" + e);
}
```
这样可以保证插件出错的情况下，不会导致JS出错

## 4.技巧

- 动态加载插件
  如果把 `<embed>` 标签放到html页面，这样在载入页面的时候，就会初始化这个插件，必将延长加载时间，而且大部分插件仍是单线程，会导致页面假死。
  所以可以利用JS来动态记载插件，比如在页面载入完成之后，先显示LOADING，然后动态将 `<embed>` 标签插入到页面，此时插件才进行初始化，或者是在需要使用到该插件时再载入，也可以起到优化的目的

- 隐藏插件不可用display
  因为插件会在页面留下一块黑色方块，1是影响美观，2是大部分时候需要隐掉，如果用CSS样式 `display = none`，会导致插件初始化失败，也就无法调用插件方法
  而用 `visibility = hidden` 则可以正常加载，也达到隐藏的目的，需显示可以用 `visibility = visible`

## 5.附录

目前用到的NPAPI

* 八宝好友
```html
<embed id="npUserManage" type="application/bq-usermanage-plugin" width="0" height="0">
```

* 播放器
```html
<embed style="visibility:hidden; position:absolute; left:0; top:0;z-index:-1" id="MediaPlayer" type="application/bq_music-plugin" width="1360" height="768"></embed>
```

* 无线传屏
```html
<embed id="dlna_plugin" style="visibility:hidden" type="application/babao-dlna-plugin" width="0" height="0">
```

* VOIP
```html
<embed id="VoipClient" type="application/vvoip-plugin" width="454" height="340" style="margin-top:-340px;">
```

* Bulletin
```html
<embed style="visibility:hidden;" id="bboBulletin" type="application/bq-bulletin-plugin"></embed>
```

* CAE
```html
<embed style="visibility:hidden;"  id="bboCAE" type="application/bq-cae-plugin"></embed>
```

* 优朋电影
```html
<embed id="npVoole" type="application/bq-voole-plugin" width="1" height="1"></embed>
```
