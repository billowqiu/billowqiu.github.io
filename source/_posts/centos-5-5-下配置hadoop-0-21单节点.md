---
title: CentOS 5.5 下配置Hadoop 0.21单节点
id: 106
categories:
  - 系统编程
date: 2011-04-10 23:47:00
tags:
---

主要参考Apache官方文档http://hadoop.apache.org/common/docs/r0.21.0/single_node_setup.html
唯一注意的是对于CentOS默认安装后的主机名问题，默认主机名为bogon，需要在/etc/hosts中加入如下一行：
<span style="font-family: mceinline;">**127.0.0.1 bogon.localdomain bogon
**</span>

运行bin/hadoop namenode -format后进行文件系统的格式化，
运行bin/start-all.sh启动所有节点，
可以通过jps查看进程：
[root@bogon hadoop-0.21.0]# jps
20532 JobTracker
20437 SecondaryNameNode
21589 Jps
20678 TaskTracker
20140 NameNode
20289 DataNode
[root@bogon hadoop-0.21.0]#
&nbsp;