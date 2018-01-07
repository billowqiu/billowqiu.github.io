---
title: Linux core文件生成及设置
id: 109
categories:
  - 系统编程
date: 2011-04-02 00:35:00
tags:
---

    

首先是生成core文件，可以通过ulimit命令设置，但是要想在整个系统中生效光在shell里面设置是不行的，可以通过如下方法：

1编辑/root/.bash_profile文件，在其中加入：ulimit -S -c unlimited

需要注意的是不是每个版本的系统都有这个文件(Suse下面就是)，如果没有可以手工创建

2重启系统或者执行:soruce /root/.bash_profile

&nbsp;

core文件的设置：

1）/proc/sys/kernel/core_uses_pid可以控制core文件的文件名中是否添加pid作为扩展。文件内容为1，表示添加pid作为扩展名，生成的core文件格式为core.xxxx；为0则表示生成的core文件同一命名为core。

可通过以下命令修改此文件：

echo &quot;1&quot; &gt; /proc/sys/kernel/core_uses_pid

2）proc/sys/kernel/core_pattern可以控制core文件保存位置和文件名格式。

可通过以下命令修改此文件：

echo &quot;/corefile/core-%e-%p-%t&quot; &gt;

core_pattern，可以将core文件统一生成到/corefile目录下，产生的文件名为core-命令名-pid-时间戳

以下是参数列表:&nbsp;&nbsp; 

%p - insert pid into filename #添加pid&nbsp;&nbsp; 

%u - insert current uid into filename #添加当前uid&nbsp;&nbsp; 

%g - insert current gid into filename #添加当前gid&nbsp;&nbsp; 

%s - insert signal that caused the coredump into the filename #添加导致产生core的信号

%t - insert UNIX time that the coredump occurred into filename #添加core文件生成时的unix时间&nbsp;&nbsp;&nbsp; 

%h - insert hostname where the coredump happened into filename&nbsp; #添加主机名

%e - insert coredumping executable name into filename

&nbsp;

上面两个设置core输出的文件，好像只能这样往里面写入内容，我通过vi修改没成功，可能跟此文件在kernal目录下有关吧。

&nbsp;

</div>