---
title: vim小技巧
id: 624
categories:
  - 工具
date: 2013-04-03 01:04:09
tags:
---

由于最近工作代码量不会太多，打算用vim搞定，刚好弥补下之前vim囫囵吞枣式的使用习惯。

*   ctags -R --languages=c,c++ .会生成一个tags文件，打开vim通过set tags=tags;命令设置tags文件路径，开始的tags表示会从当前目录找tags文件，分号后面可以再加其他可选的tags文件，ctrl+]在实现和定义之间跳转，ctrl+t回到上一个动作。
*   Texplore这个命令可以在打开tab页时浏览文件夹，很方便的打开多个文件，剩下的就可以通过tab*命令切换tab页了
*   %进行括号匹配跳转
*   可视化模型下，&lt;,&gt;分别进行减少，增加缩进
*   set encoding设置文件在终端上面显示的编码
*   set fileencoding设置文件写入磁盘的编码