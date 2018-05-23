---
title: yeoman入门笔记
tags:
  - yeoman
id: 1786
categories:
  - Web
abbrlink: b3a22ac5
date: 2017-04-19 16:23:26
---
yeoman官网 [http://yeoman.io/](http://yeoman.io/)

yeoman是一个脚手架工具，用来初始化项目目录结构，便于部署项目
早期的web项目基本目录接口大概是这样子

![](/images/2017/04/yeoman_1.png)

随着web技术发展，各种前端框架的广泛应用，现在的目录结构大概是这样子

![](/images/2017/04/yeoman_2.png)

根据不同框架、不同编译环境、不同测试环境，以及发布部署，结构更是千变万化，而这些都是比较耗时间去考虑的，现在有了yeoman，可以自动化生成这些结构，只需选好模板，一条命令几个回车即可搞定
<!--more-->
首先要安装node.js，然后再安装yeoman
```sh
npm install -g yo
```
然后执行
```sh
yo
```

![yeoman](/images/2017/04/yeoman_3.png)

使用默认自带的模板进行初始化，也可以在官网找其它模板来初始化
官网目前有6000+模板可使用，安装可用yo的向导式菜单进行安装，也可直接命令行安装，比如
```sh
npm install --global generator-jquery
```
然后初始化
```sh
yo jquery
```
目录结构到依赖的库什么的一次搞定，那酸爽！