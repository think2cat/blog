---
title: Bower学习笔记
tags:
  - bower
  - nodejs
id: 1825
categories:
  - Web
abbrlink: cf44a887
date: 2017-08-21 15:17:19
---
![bower](/images/2017/08/bower.png)

#### 1. bower是什么
bower是基于node.js的包管理工具，可以方便的安装和卸载web开发资源，诸如jQuery、bootstrap之类
而像yeoman和grunt则是在bower基础上的开发工具

#### 2. bower和npm的区别
都是包管理工具，但是bower是for web，npm的包依赖是一种独特的嵌套依赖关系树，依赖包层层嵌套树杈非常坑长，打开node_modules可以看到里面密密麻麻的包，但对于前端库来说，这些基本是多余的，往往需要的只是一个未压缩或者压缩后的js和css而已，而bower正是管理js/css/模板的一个工具
[Stack上有相关评论点这里](https://stackoverflow.com/questions/18641899/what-is-the-difference-between-bower-and-npm)
<!-- more -->

#### 3. 如何安装
安装完node.js后输入
npm install -g bower
安装完后输入 bower -v 如果能打印版本号说明安装成功

#### 4.  如何使用

* 初始化bower.json
```cmd
bower init
```
* 安装bower本身支持的包，支持列表见 https://bower.io/search/
```cmd
bower install jQuery --save-dev
```
* 安装github上的包，则可用短连接形式安装
```cmd
bower install jQuery/jQuery
```
* 安装其它网上资源，则直接跟上URL
```cmd
bower install https://github.com/jquery/jquery
```
* 安装特定版本库
```cmd
bower install bootstrap#3.3.7
```
* 卸载安装包
```cmd
bower uninstall jquery
```
* 搜索
```cmd
bower search bootstrap
```
* 查看库信息，包括各版本和依赖
```cmd
bower info bootstrap
```
* 查看本地库的依赖关系
```cmd
bower list
```
* 打开包的官网
```cmd
bower home bootstrap
```
#### 5. 配置文件.bowerrc
```json
{
    "directory": "bower_components" //包目录，我一般localhost会指到build目录，所以配成build/bower_components
    "json" : "bower.json", //包管理文件
    "endpoint" : "https://Bower.herokuapp.com", //用来搜索库的索引地址
    "searchpath" : "", //备用索引地址，一般是用于非公开库的搜索
    "proxy": "http://proxy.local", //网络代理设置
    "https-proxy": "https://proxy.local",
    "timeout": 60000
}
```
其它参数请看[官方文档](https://bower.io/docs/config/)，或者[GitHub](https://github.com/bower/spec/blob/master/config.md)

#### 6. 补充：npm v3的依赖管理
上面说的嵌套依赖管理是npm v2的，在v3已经做了优化，在库版本兼容前提下不会再重复下载，详情可看[这里](https://docs.npmjs.com/how-npm-works/npm3) |  [中文](http://coloration.cc/npmjs-documentation/2016/03/29/npmV3.html)

![](/images/2017/08/npm3deps4.png)    

#### 7. 补充：关于语义化版本
[http://semver.org/lang/zh-CN/](http://semver.org/lang/zh-CN/)