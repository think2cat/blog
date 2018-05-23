---
title: 群晖NAS上的SVN和GIT服务
tags:
  - GIT
  - svn
id: 1784
categories:
  - Web
abbrlink: 9f28229e
date: 2017-04-15 23:51:54
---
下午终于把SVN搬到NAS上，然后又把SVN迁移到GIT
群晖的套件有SVN和GIT，直接安装即可
SVN安装完后，磁盘会多出svn文件夹，如果有完整的版本库，直接拷进去然后在群晖管理页面的SVN套件刷新就可以看到，登录用户名什么都照原来的，根本不用设置
如果需要新建版本库，则在SVN套件点新建即可

群晖的GIT套件安装完后，建议新建一个git用户专门来管理GIT目录，然后在GIT套件设置该用户的权限即可
GIT不像SVN可以直接在面板新建库，需要打开SSH然后登录，登录账户必须是administrator群组里的账户，登录后新建目录，然后执行git初始化
```sh
git init --bare
```
<!--more-->
接着本地再进行clone
```sh
git clone ssh://git@192.168.1.2/volume1/git/test
```
因为是空库，可能会提示错误什么，不用管，反正搞完push上去就OK

如果跟我一样是从svn迁移过来的，git有个svn命令
```sh
git svn svn://192.168.1.2/test/
```
可以做整库迁移，保留commit信息
迁移后的git库再push到remote即可