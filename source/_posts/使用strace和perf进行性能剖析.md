---
title: 使用strace和perf进行性能剖析
id: 566
categories:
  - Linux
  - 系统编程
  - 网络通讯
date: 2012-11-23 20:38:04
tags:
---

写了一个小型的udp代理程序，刚刚测试发现性能没有达到预期。通过以下步骤进行了简单分析：

# strace -p  $pid -c -o strace.log

# cat starce.log

System call usage summary for 32 bit mode:
% time     seconds         usecs/call                  calls             errors syscall
------ ----------- ----------- ---------              ---------           ----------------
47.16    0.018374                        0               2397064           sendmsg
26.21    0.010212                        0               2397065           recvmsg
12.93    0.005036                       0               2397076           gettimeofday
8.42    0.003282                        0              1198532            epoll_wait
5.28    0.002057                        0               1198538           time
0.00    0.000000                      0               6                        write
0.00    0.000000                      0               6                        epoll_ctl
------ ----------- ----------- ---------           ---------             ----------------
100.00    0.038961                                    9588287           total

可以看到除网络相关的系统调用外，gettimeofday和time调用比较频繁，一时也没有找到程序中哪里有该调用，再通过perf top -p $pid获得如下输出

samples          pcnt     function                                           DSO

2304.00         22.3%     __random_r                               /lib/libc-2.12.so
539.00          5.2%     __srandom_r                                  /lib/libc-2.12.so
439.00          4.3%     copy_user_generic_string            [kernel.kallsyms]
356.00          3.5%     _spin_lock                                        [kernel.kallsyms]
261.00          2.5%     ia32_sysenter_target                      [kernel.kallsyms]
259.00          2.5%     ipt_do_table                                     [kernel.kallsyms]

突然想起有个生成seq的函数里面有rand和gettimeofday，经过简单优化，重新测试性能翻倍。

**PS:strace果然是神器啊**