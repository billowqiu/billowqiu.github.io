---
title: thrift安装记录
id: 501
categories:
  - C++
  - 系统编程
date: 2011-04-15 12:30:05
tags:
---

thrift依赖boost，libevent，openssl等

前两者都用默认安装即可，openssl安装时需要注意使用如下配置选项：

./config --prefix=/usr/local/ shared，否则会导致thrift在configure时出现依赖ssl或者crypto错误，折腾了半天，记录下。

另外在thrift配置时主要使用以下选项，以包含相应文件：

./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H"