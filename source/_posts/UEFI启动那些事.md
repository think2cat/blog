---
title: UEFI启动那些事
tags:
  - BIOS
  - ESP
  - MSR
  - UEFI
  - WIN10
id: 1858
categories:
  - IT
abbrlink: 7a24af62
date: 2017-12-11 17:35:58
---
UEFI早在大概05年就推出1.1版了，但是一直没有推广开，直到win7开始支持
我作为一个守旧派不大愿意接受新硬件，也一直坚守Legacy BIOS启动方式（下面简称BIOS）

但是最近换了新电脑，自带WIn10，格掉后重新分区，Ghost了一版Win10后发现无法启动，主板设置居然连硬盘启动都消失了，折腾半天无果，才不得已看了恶补了一下知识
关于BIOS启动和UEFI启动可以看这篇豆芽文
[Unified Extensible Firmware Interface](https://wiki.archlinux.org/index.php/Unified_Extensible_Firmware_Interface)

简单来说，BIOS启动后从硬盘MBR引导扇区（Master Boot Record）加载引导信息，而UEFI是从\ESP分区加载引导信息，这又涉及到MBR分区表和GPT分区表
<!-- more -->
MBR和GPT分区表在使用和性能上是没有差别的，只是由于MBR规定是硬盘扇区前面512字节，长度的限制，所以只能支持2.2T硬盘和最多4个分区，而GPT则没有这方面限制，理论上支持无限分区
在分区软件DiskGenius可以看到2种分区表类型，GUID则是GPT（GPT是缩写，全称是GUID Partition Table (GPT)）

![DiskGenius](/images/2017/12/DiskGenius-1.png)
	（图片来自网络）

选择GPT分区表下方可以看到ESP和MSR分区
> ESP = EFI system partition
MSR = Microsoft Reserved Partition

其中ESP就是上面提到的引导区，如果硬盘不是系统启动盘，即使用GPT分区表，ESP分区也是 可以不要的，再者MSR分区是为了避免硬盘插到老系统提示硬盘未分区而做的兼容，如果不需要兼容老系统也是可以不要的

接着来说我遇到的主板设置找不到硬盘引导的问题，就是分区时用了MBR形式，而主板并不兼容这种老引导，解决就是重新用EFI分区表，然后重装系统，重装时会自动在ESP分区写入引导文件，如果跟我一样是GHOST的，那GHOST完后，还要手动往ESP分区写入引导文件，win10是自带了修复工具，命令行下敲bcdboot可以看到，如果有的话直接敲
```cmd
bcdboot C:\Windows /s I: /f UEFI /l zh-cn
```
但是我启动的PE居然没有这个命令，好在自带了一个小工具

![](/images/2017/12/uefi修复.png)

709KB 非常小巧，修复完重启，终于可以看到win10的启动画面了，内牛满面！

另外晒下新买的本本

![](/images/2017/12/DiskGenius.png)