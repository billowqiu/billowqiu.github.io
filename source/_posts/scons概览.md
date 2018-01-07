---
title: SCons基本概念
tags:
  - SCons
  - 工具
  - 翻译
id: 491
categories:
  - 工具
date: 2012-07-08 22:30:03
---

**SCons环境**当SCons开始构建工程，它会创建一个自己的环境，这个环境包括工程依赖关系树，帮助函数，创建者和一些其他的工具。这个环境一部分在内存中创建，另外一些存储到磁盘以便下次构建时加速。这个是SCons的环境，不同于使用Makefile的用户理解的系统环境。

**系统环境**是一个类似操作系统的环境容器，包括PATH,HOME等变量。通常可以通过os.environ在Python中获取，因此在SCons也是如此。SCons没有隐式导入任何系统环境设置(例如编译器选项，工具路径等)，因为它被设计为行为可预见的跨平台工具。这也是为什么如果依赖一些系统环境变量，你必须自己通过SCons脚本隐式获取。

**SConsturc**是SCons执行的主脚本名，处理即从它开始。

**SConscript**也是SCons执行的脚本，只不过是放到工程的子目录。这些文件主要是为了进行分层级的编译，通过放在工程根目录的SConstruct包含。

**Builder**是一个SCons对象，你可以显式调用它在一系列源文件中指定编译依赖目标。SCons的威力是依赖关系会自动的被跟踪，当源文件更改后，系统会自动探测哪些目标需要重新构建。

**SCanner**另外一个SCons对象。它可以扫描额外依赖的源文件，这些依赖的文件不会被作为Builder的源文件编译。

**Tool**是任何扩展Builders，Scanners，和其他有助于SCons环境的外部组件，SCons通过扫描一些目标位置来寻找工具，这些工具可以是工程特定，也可以通过自己安装来扩展核心功能的。

原文为[http://www.scons.org/wiki/BasicConcepts](http://www.scons.org/wiki/BasicConcepts)