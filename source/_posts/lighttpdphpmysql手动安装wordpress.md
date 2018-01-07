---
title: 'lighttpd,php,mysql手动安装wordpress'
id: 529
categories:
  - Web
date: 2012-09-08 15:48:30
tags:
---

本文介绍由lighttpd,php,mysql搭建windows上的Wordpress站点的步骤，linux上面操作类似。

**准备工作**，附上各个组件的下载地址：

lighttpd:[http://en.wlmp-project.net/downloads.php?cat=lighty](http://en.wlmp-project.net/downloads.php?cat=lighty)

php:[http://windows.php.net/download/](http://windows.php.net/download/)

mysql:[http://dev.mysql.com/downloads/mysql/](http://dev.mysql.com/downloads/mysql/)

wordpress:[http://cn.wordpress.org/](http://cn.wordpress.org/)

下载之后解压为如下目录：

![](/images/2012/09/wordpress-install.png)

**组件配置**

**lighttpd:**

修改LightTPDconf目录下面的lighttpd.conf为如下配置
<pre class="brush:other">server.document-root = "K:softwarewebwordpress"

index-file.names            = ( "index.php", "index.pl", "index.cgi", "index.cml",
                                "index.html", "index.htm", "default.htm" )

server.modules += ("mod_fastcgi")			   
#### fastcgi module
## read fastcgi.txt for more info
## for PHP don't forget to set cgi.fix_pathinfo = 1 in the php.ini
## ... and PHP_FCGI_MAX_REQUESTS = 0 environment variable in system properties
fastcgi.server             = ( ".php" =&gt;
                               ( "localhost" =&gt;
                                 (
                                   "host" =&gt; "127.0.0.1",
                                   "port" =&gt; 9000
                                 )
                               )
                             )</pre>
 上面的host:port为php-cgi的监听地址。

**php：**

复制php.ini-production为php.ini，打开以下字段即可，

extension_dir = "ext"

cgi.fix_pathinfo=1

extension=php_mysql.dll

**mysql:**

运行binmysqld.exe --defaults-file=my-small.ini，启动mysqld，创建一个名为wordpress的数据库，推荐使用MySQL Workbench这个工具，很方便。

**wordpress：**

不用特殊配置

**运行**

1启动lighttpd，LightTPD.exe -f conflighttpd.conf -m modules

2启动php-cgi，php-cgi.exe -b 127.0.0.1:9000 -f php.ini

3打开浏览器，输入[http://localhost](http://localhost)即可看到wordpress安装页面。