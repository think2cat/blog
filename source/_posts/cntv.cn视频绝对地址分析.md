---
title: cntv.cn视频绝对地址分析
tags:
  - actionscript
  - cctv
  - javascript
  - json
  - spider
id: 1116
categories:
  - Web
abbrlink: 64fe6205
date: 2010-01-29 17:45:26
---
[http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.shtml](http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.shtml)

这是CCTV电影站一个播放页面地址，打开源码，找到播放器一段，flash player是由JS创建的
```as
var fo = createPlayer("v_player_cctv",928,552);
fo.addVariable("videoId", "20091221100120");
fo.addVariable("articleId", "VIDE0020091221100120");
fo.addVariable("scheduleId", "C13671000001");
fo.addVariable("filePath", "/love/lianaiqianguize/classpage/video/");
fo.addVariable("sorts", "其他,,,爱情,C13671,CCTV-9");
fo.addVariable("sysSource", "discovery");
fo.addVariable("url", "http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.shtml");
fo.addVariable("videoCenterId","36b502b4ceeb4a6d950955b4bd9e93f8");//视频生产中心id
fo.addVariable("channelId",channelId);//埋码
fo.addVariable("adCall",adCall);//广告
fo.addVariable("isLogin", "y");
fo.addVariable("userId", "001");
fo.addVariable("hour24DataURL", "");
fo.addVariable("isCycle", "false");
fo.addVariable("wideMode", "normal");
fo.addVariable("defaultRate", "low");
fo.addVariable("isAutoPlay", "true");
writePlayer(fo,"myFlash");
```
** createPlayer() ** 函数在 ** film_standard2.js ** 文件，找到swf播放器地址 ** http://player.cntv.cn/standard/cntvplayer.swf ** ，下载下来用 Action Script Viewer 6 打开
<!--more-->

发现里面有这么个类 ** com.cntv.common.model.proxy.GetVideoInfoProxy ** ，通过用 videoId、articleId、filePath 等关键字搜索，找到这么一段AS代码

```as
_local2 = (((((this._localtor.configVO.videoInfoURL + this._localtor.paramVO.filePath) + this._localtor.paramVO.videoId.substr(0, 8)) + "/") + this._localtor.paramVO.videoId.substr(8, (this._localtor.paramVO.videoId.length - 1))) + ".txt");
```

就是说把videoId截前面8位，作为目录，后面是文件名，用 videoId = "20091221100120" 代入，得到这么一个地址

[http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.txt](http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.txt)，跟播放页地址一样，后缀名改为txt而已，把地址放到IE打开，发现是一个JSON格式的文本，格式化之后如下
```json
{
    "videoid": "20091221100120",
    "duration": "21:56",
    "isLive": "false",
    "title": "恋爱前规则1",
    "imagePath": "http://p1.img.cctvpic.com/image/2009/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439_2.jpg",
    "isRtmp": "false",
    "ack": "yes",
    "relation": "public",
    "refHtml": "&lt;embed id=’v_player_cctv’ width=’960′ height=’540′ flashvars=’videoId=20091221100120&amp;isLogin=y&amp;userId=001&amp; articleId=VIDE0020091221100120&amp;scheduleId=C13671000001&amp; filePath=/love/lianaiqianguize/classpage/video/&amp;sorts=其他,,,爱情,C13671000001&amp;url=http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.shtml&amp;configPath=http://dianshiju.cntv.cn/nettv/Library/film/player/config.xml&amp;widgetsConfig=http://dianshiju.cntv.cn/nettv/Library/film/player/widgetsConfig.xml&amp;languageConfig=http://dianshiju.cntv.cn/nettv/Library/film/player/zh_cn.xml&amp;sysSource=film’ allowscriptaccess=’always’ allowfullscreen=’true’ menu=’false’ quality=’best’ bgcolor=’#000000′ name=’v_player_cctv’ src=’http://dianshiju.cntv.cn/nettv/Library/tongyong/player/OutSidePlayer.swf’ type=’application/x-shockwave-flash’ lk_mediaid=’lk_juiceapp_mediaPopup_1257416656250′ lk_media=’yes’/&gt;",
    "refURL": "http://dianying.cntv.cn/love/lianaiqianguize/classpage/video/20091221/100120.shtml",
    "chapters": [
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439-1.mp4",
            "duration": "296"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439-2.mp4",
            "duration": "297"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439-3.mp4",
            "duration": "299"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439-4.mp4",
            "duration": "299"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264418000nero_aac32_20091221_1261357606439-5.mp4",
            "duration": "126"
        }
    ],
    "chapters2": [
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-1.mp4",
            "duration": "178"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-2.mp4",
            "duration": "178"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-3.mp4",
            "duration": "178"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-4.mp4",
            "duration": "179"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-5.mp4",
            "duration": "179"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-6.mp4",
            "duration": "176"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-7.mp4",
            "duration": "176"
        },
        {
            "url": "http://sportsipr.v.cctv.com/200911/qgds/2009/12/21/qgds_h264818000nero_aac32_20091221_1261357611665-8.mp4",
            "duration": "72"
        }
    ]
}
```
实际是个播放列表，直接用迅雷就可以下载了

事实上，cntv还有另一种json格式，比如[《火星没事》http://dianying.cntv.cn/lcomedy/C15956/classpage/video/20100128/100696.shtml](http://dianying.cntv.cn/lcomedy/C15956/classpage/video/20100128/100696.shtml)，JSON文本如下
```json
{
    "videoid": "20100128100696",
    "duration": "01:32:36",
    "isLive": "false",
    "title": "火星宝贝之火星没事",
    "imagePath": "http://p3.img.cctvpic.com/image/2009/qgds/2010/01/28/qgds_h26441800\r\n\r\n0nero_aac32_20100128_1264647498894_2.jpg",
    "isRtmp": "true",
    "ack": "yes",
    "relation": "public",
    "refHtml": "&lt;embed id=’v_player_cctv’ width=’960′ height=’540′ flashvars=’videoId=20100128100696&amp;isLogin=y&amp;userId=001&amp;articleId=VIDE0020100128100696&amp;scheduleId=C15956000001&amp;filePath=/lcomedy/C15956/classpage/video/&amp;sorts=其他,,,喜剧,C15956000001&amp;url=http://dianying.cntv.cn/lcomedy/C15956/classpage/video/20100128/100696.shtml&amp;configPath=http://dianshiju.cntv.cn/nettv/Library/film/player/config.xml&amp;widgetsConfig=http://dianshiju.cntv.cn/nettv/Library/film/player/widgetsConfig.xml&amp;languageConfig=http://dianshiju.cntv.cn/nettv/Library/film/player/zh_cn.xml&amp;sysSource=film’ allowscriptaccess=’always’ allowfullscreen=’true’ menu=’false’ quality=’best’ bgcolor=’#000000′ name=’v_player_cctv’ src=’http://dianshiju.cntv.cn/nettv/Library/tongyong/player/OutSidePlayer.swf’ type=’application/x-shockwave-flash’ lk_mediaid=’lk_juiceapp_mediaPopup_1257416656250′ lk_media=’yes’/&gt;",
    "refURL": "http://dianying.cntv.cn/lcomedy/C15956/classpage/video/20100128/100696.shtml",
    "streams": [
        {
            "rtmpHost": "rtmp://streaming.cctvpic.com/vod/",
            "streamName": "mp4:v/2010/01/28/65f94038a6f14759bf061da589feba26_h264218000nero_aac32.mp4",
            "bitRate": "250"
        },
        {
            "rtmpHost": "rtmp://streaming.cctvpic.com/vod/",
            "streamName": "mp4:v/2010/01/28/65f94038a6f14759bf061da589feba26_h264418000nero_aac32.mp4",
            "bitRate": "450"
        },
        {
            "rtmpHost": "rtmp://streaming.cctvpic.com/vod/",
            "streamName": "mp4:v/2010/01/28/65f94038a6f14759bf061da589feba26_h264818000nero_aac32.mp4",
            "bitRate": "850"
        },
        {
            "rtmpHost": "rtmp://streaming.cctvpic.com/vod/",
            "streamName": "mp4:v/2010/01/28/65f94038a6f14759bf061da589feba26_h2641436000nero_aac64.mp4",
            "bitRate": "1500"
        }
    ]
}
```
rtmp 是adobe用于flash流媒体的播放协议，详情猛击这里 [http://www.51testing.com/?uid-8776-action-viewspace-itemid-81594](http://www.51testing.com/?uid-8776-action-viewspace-itemid-81594)

关于rtmp的播放，我也不清楚，在反编译的AS中截取这么几段代码，剩下的请自己分析吧
```as
override protected function initNetConnection():void{
		this.nc = new NetConnection();
		this.nc.addEventListener(NetStatusEvent.NET_STATUS, this.ncNetStatuHandler);
		this.nc.addEventListener(SecurityErrorEvent.SECURITY_ERROR, this.ncSecurityHandler);
		this.nc.addEventListener(IOErrorEvent.IO_ERROR, this.ncIOErrorHandler);
		this.nc.addEventListener(AsyncErrorEvent.ASYNC_ERROR, this.ncAsyncErrorHandler);
		this.nc.client = new VideoPlayerClient(this);
		this.nc.call("checkBandwidth", null);
}
override public function play():void{
	if (!this.isStartPlay){
		this.nc.connect(_modelLocator.currentVideoInfo.rtmpHost);
	} else {
		if (this.ns != null){
			this.ns.resume();
			this.isPause = false;
			_dispatcher.dispatchEvent(new ControlBarEvent(ControlBarEvent.EVENT_VIDEO_PLAY));
		};
	};
}

private function createStream():void{
	this.ns = new NetStream(this.nc);
	this.ns.bufferTime = this.BUFFER_TIME;
	this.ns.addEventListener(NetStatusEvent.NET_STATUS, this.nsNetStatusHandler);
	this.ns.addEventListener(IOErrorEvent.IO_ERROR, this.nsIOErrorHandler);
	this.ns.addEventListener(AsyncErrorEvent.ASYNC_ERROR, this.nsAsyncErrorHandler);
	this.ns.client = new VideoPlayerClient(this);
	this.video = new Video();
	this.video.smoothing = true;
	this.video.attachNetStream(this.ns);
	this.psFilter = new PSFilterTools(this.video);
	this.addChild(this.video);
	setMask();
	this.ns.play(_modelLocator.currentVideoInfo.streamName);
}
```