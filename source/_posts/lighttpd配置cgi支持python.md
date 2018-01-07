---
title: lighttpd配置cgi支持python
id: 551
categories:
  - Python
  - Web
date: 2012-11-06 19:03:57
tags:
---

1：static-file.exclude-extensions += (".py")

2：server.modules += ( "mod_cgi" )

3：cgi.assign = (".py" =&gt; "/usr/bin/python")

&nbsp;