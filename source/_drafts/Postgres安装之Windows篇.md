---
title: Postgres安装之Windows篇
id: 509
categories:
  - 未分类
tags:
---

1添加用户，postgreSQL不允许以系统管理员身份安装，参见其wiki说明，[http://wiki.postgresql.org/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E4%B8%8E%E8%A7%A3%E7%AD%94](http://wiki.postgresql.org/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E4%B8%8E%E8%A7%A3%E7%AD%94)，执行以下命令添加用户：

net user postgres postgres /add /expires:never /passwordchg:no

用户名和密码都为postgres，密码永不过期，不允许更改密码，如果已存在该账户可以

net localgroup users postgres /delete删除之。

2给用户设置目录权限：

cacls F:DBMSpgsql /E /T /G postgres:R

cacls F:DBMSpgsql /E /T /G postgres:C

3注册服务并启动之：

cd F:DBMSpgsqlbin
pg_ctl register -N postgres -D F:DBMSpgsqldata -w -U postgres -P postgres

4

&nbsp;