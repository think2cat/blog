---
title: Flash ActionScript 1.0 学习笔记(2)
tags:
  - actionscript
  - flash
id: 841
categories:
  - Flash
date: 2005-05-26 01:19:00
---

19.鼠标双击拖动

<DIV style="BORDER-RIGHT: #ccf 1px dotted; PADDING-RIGHT: 5px; BORDER-TOP: #ccf 1px dotted; PADDING-LEFT: 5px; PADDING-BOTTOM: 5px; MARGIN: 5px; BORDER-LEFT: #ccf 1px dotted; WIDTH: 90%; PADDING-TOP: 5px; BORDER-BOTTOM: #ccf 1px dotted">
Mouse.doubleClick = function(clickTime){
if(!clickTime){
clickTime = 500;
}
if(Mouse.lastClickTime-(Mouse.lastClickTime=getTimer()+clickTime&gt;0)){
return ture;
}
}
</DIV>

20.鼠标移动速度

<DIV style="BORDER-RIGHT: #ccf 1px dotted; PADDING-RIGHT: 5px; BORDER-TOP: #ccf 1px dotted; PADDING-LEFT: 5px; PADDING-BOTTOM: 5px; MARGIN: 5px; BORDER-LEFT: #ccf 1px dotted; WIDTH: 90%; PADDING-TOP: 5px; BORDER-BOTTOM: #ccf 1px dotted">
function mouseSpeedObj(fps) {
this.fps = fps;
this.distArray = [];
for (var i = 0; i<FPS; i++) {
 this.distArray[i] = 0;
}
this.distSum = 0;
this.distArrayId = 0;
}
mouseSpeedObj.prototype.speed = function() {
var xdiff = xold-(xold=_root._xmouse);
var ydiff = yold-(yold=_root._ymouse);
var dist = Math.sqrt(xdiff*xdiff+ydiff*ydiff);
if (dist == 0) {
return 0;
}
this.distSum -= this.distArray[this.distArrayId];
this.distArray[this.distArrayId] = dist;
this.distSum += dist;
this.distArrayId = ++this.distArrayId%this.fps;
return Math.round(this.distSum*0.127044)/100;
};

//调用
onClipEvent (load) {
foo = new _parent.mouseSpeedObj(12);
}
onClipEvent (enterFrame) {
ms = foo.speed();
}
</DIV>

21.mouseDown事件只能检查鼠标左键，要检查右键、中键应用enterFrame

<DIV style="BORDER-RIGHT: #ccf 1px dotted; PADDING-RIGHT: 5px; BORDER-TOP: #ccf 1px dotted; PADDING-LEFT: 5px; PADDING-BOTTOM: 5px; MARGIN: 5px; BORDER-LEFT: #ccf 1px dotted; WIDTH: 90%; PADDING-TOP: 5px; BORDER-BOTTOM: #ccf 1px dotted">
if (Mouse.checkBtnClicked(1)) {
// 按下左键
if (!hitTest(_root._xmouse, _root._ymouse, true)) {
_visible = 0;
}
trace("左");
} else if (Mouse.checkBtnClicked(2)) {
// 按下右键
_visible = 1;
_x = _root._xmouse;
_y = _root._ymouse;
trace("右");
} else if (Mouse.checkBtnClicked(4)) {
// 按下中键
_visible = 1;
_x = _root._xmouse;
_y = _root._ymouse;
trace("中");
}
</DIV>

22.keyPress有一个特点，当键盘被按下不释放时，里面的程序连续执行。Flash最多支持同时按下三个键

23.让网页中的Flash得到焦点，(只支持WinIE)

24.麦克风对象
myMic = Microphone.get(); //创建Mic对象
createEmptyMovieClip("micMC",100); //创建空剪辑
micMC.attachAudio(myMic); //将Mic对象附加到剪辑上
activityLevel属性，返回声音音量，int类型0～100
onActivity事件，开始说话或停止说话时触发一次，默认值是10和2s，表示连续2秒音量都低于10，可以利用setSilenceLevel修改
setSilenceLevel()方法，setSilenceLevel(level,timeout)，level在0～100间，timeout以ms为单位

25.摄像头同样有activityLevel属性，setMotionLevel()跟setSilenceLevel()相同

26.TextField的_alpha属性只有嵌入了字符的动态文本才支持，获取颜色时得到的是十进制格式，修改颜色相关属性时，仍需用十六进制的格式，如0xRRGGBB

27.多种字段格式，TextField.setTextFormat(起始索引,完结索引,TextFormat)
索引由0记起，在字符与字符间计算，第一个字符前为0，第一个字符与第二个字符间为1

28.getTimer()函数返回的是Flash自播放起累计时间，单位ms
Date对象返回1970年1月1日午夜以来时间，单位ms
var myTimer = new Date().getTimer();