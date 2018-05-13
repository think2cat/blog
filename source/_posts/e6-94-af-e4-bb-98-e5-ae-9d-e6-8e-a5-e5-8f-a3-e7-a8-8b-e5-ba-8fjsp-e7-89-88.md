---
title: 支付宝接口程序JSP版
tags:
  - java
  - jsp
  - 接口
  - 淘宝
  - 网银
id: 927
categories:
  - Web
abbrlink: 8a017fb4
date: 2007-03-20 21:51:20
---

支付宝论坛提供的：[支付宝接口程序论坛地址](http://club.alipay.com/show_thread-80---5699059-.htm) | [ASP](http://union.alipay.com/alipay/dev_download/xuni/new_asp_xuni.rar) | [ASP.net(GBK)](http://union.alipay.com/alipay/dev_download/xuni/xuniaspx.rar) | [ASP.net(UTF-8)](http://union.alipay.com/alipay/dev_download/xuni/WebSite1utf.rar) | [JSP(GBK)](http://union.alipay.com/alipay/dev_download/xuni/jsp_xuni_gbk.rar) | [JSP(UTF-8)](http://union.alipay.com/alipay/dev_download/xuni/jsp_xuni.rar)

支付宝提供的接口，似乎有点问题，处理完返回的时候总是出现false，经对比发现是MD5加密后字符串不一样了

    Map params = new HashMap();
    //获得POST 过来参数设置到新的params中
    Map requestParams = request.getParameterMap();
    for (Iterator iter = requestParams.keySet().iterator(); iter.hasNext();) {
    	String name = (String) iter.next();
    	String[] values = (String[]) requestParams.get(name);
    	String valueStr = "";
    	for (int i = 0; i < values.length; i++) {
    		valueStr = (i == values.length - 1) ? valueStr + values[i]: valueStr + values[i] + ",";
    	}
    	params.put(name, valueStr);
    }`</pre>

    解决办法是做字符转换，正确代码如下

    <pre>`Map params = new HashMap();
    //获得POST 过来参数设置到新的params中
    Map requestParams = request.getParameterMap();
    for (Iterator iter = requestParams.keySet().iterator(); iter.hasNext();) {
    	String name = (String) iter.next();
    	String[] values = (String[]) requestParams.get(name);
    	String valueStr = "";
    	for (int i = 0; i < values.length; i++) {
    		valueStr = (i == values.length - 1) ? valueStr + values[i]: valueStr + values[i] + ",";
    	}
    	valueStr = new String(valueStr.getBytes("iso8859_1"),"gb2312");		//转换字符集
    	params.put(name, valueStr);
    }

附上**<span style="color: #ff0000;">service</span>**参数类型，接口文档没有的

1.  1.  <span style="color: #ff0000;"> create_digital_goods_trade_p</span> 通知返回地址等即可实现实物交易
&nbsp;

1.  <span style="color: #ff0000;"> trade_create_by_buyer</span> 修改物流方式以及费用等即可实现实物交易
2.  <span style="color: #ff0000;"> create_donate_trade_p</span> 可实捐赠项目
3.  <span style="color: #ff0000;"> create_direct_pay_by_user</span> 支付宝论坛没说，自己试了，应该即时到帐
另外Payment.java似乎有2个版本，一个用于虚拟物品交易（无收货地址），另一个用于现实物品交易，源码是一样的，只是有2个变量名称不一样，致使MD5加密的时候出错，如果你提交支付的时候出现支付类型错误，可以试着检查一下