---
title: linux设置DNS客户端
id: 598
categories:
  - 网络通讯
date: 2013-01-24 14:47:57
tags:
---

1修改/etc/resolv.conf，添加对应的nameserver和domain

2修改/etc/nsswitch.conf，找到hosts，在后面添加dns，即hosts: files dns，表示先从/etc/hosts文件查找，再通过dns查找

3重启dns cache进程nscd，否则可能会出现域名可以解析，但是ping不通