---
title: python之命令行解析和运行外部命令
id: 523
categories:
  - Python
date: 2012-09-06 12:54:25
tags:
---

最近在做一个删除日志的脚步过程中用到了python，涉及到了命令行解析，运行外部程序，xml解析等。

Python中的命令行解析有几种方式，比如较古老的getopt，由于个人不太熟悉，选择了使用更方便的optparse.OptionParser，使用上非常像boost.program_option；运行外部程序也有多种方式，有类似system的调用和popen族，前者方便使用，但是不能得到命令运行的输出，后者则没有这个限制，帮助文档上面也比较推荐使用，以下是一个简单的demo，仅仅作为一个备忘吧。
<pre class="brush:python">import shlex, subprocess
import optparse

def option_parse():
    parser = optparse.OptionParser()
    parser.add_option("-d", "--duration", dest="duration", default="30", help="the duration need to delete from hadoop")
    parser.add_option("-f", "--cfg", dest="cfgfile", default="server_config.xml", help="config file")
    parser.add_option("-q", "--quiet", action="store_false", dest="verbose", default=True, help="don't print status messages to stdout")

    return parser.parse_args()

def run_cmd(cmd):
    try:
        args = shlex.split(str(cmd))
        #args = shlex.split(cmd)
        print args
        p = subprocess.Popen(args, stdout=subprocess.PIPE)
        (output, err) = p.communicate()
        return output
    except Exception,e:
        print e
        return ""

option_parse()</pre>
在run_cmd函数中，如果使用注释部分代码，有可能会出现编码方面的问题，具体为什么没有深究，加上str转换就好了。