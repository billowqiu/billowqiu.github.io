---
title: vmware虚拟机copy或move
id: 657
categories:
  - 未分类
date: 2013-07-29 00:03:46
tags:
---

今天把用了三年多的xp换成了win7，有很多软件是不需要重装的，但是vmware必须得重装，接踵而至的就是vmware虚拟的IP段和网卡地址变化，导致打开原有的虚拟机时会提示类似“I copy it or move it”。

这个应该是因为vmware网卡变化导致的，网上有找到这个copy和move的区别：

*   <span style="line-height: 1.714285714; font-size: 1rem;">选择I copied it的时候，VMware软件检测到物理机改变后会对个虚拟机重新生成新的网卡MAC地址，UUID</span>
*   <span style="line-height: 1.714285714; font-size: 1rem;">选择I moved it，只改变UUID，虚拟机其它配置不变</span>
我一般都习惯性的选择move，看来这个习惯还是正确的，呵呵，但是虚拟机IP还是得修改才能方便使用，例如重装前装的虚拟机IP大致如下：

![suse_ifconfig_old](/images/2013/07/suse_ifconfig_old.png)
而重装后vmware的IP地址发生了变化：
![vmware8_ipconfig](/images/2013/07/vmware8_ipconfig.png)这就导致打开的虚拟机IP段和vmware的不匹配，从而无法与vmware通信，进而与本机实体机通信，最简单的临时方案时先在vmware中登陆，修改虚拟机的IP：
![modify_ip](/images/2013/07/modify_ip.png)

&nbsp;

后面就可以通过secretcrt之类工具在windows使用了，但是每次都这样改也不是长久之计，终极方案是彻底修改掉其IP，在suse下面是如下文件：

/etc/sysconfig/network/ifcfg-eth-id-00:0c:29:c5:67:f8(后面这串数字其实就是虚拟机的mac)

修改后直接重启即可。

如果选择copy，好像是会改变网络接口名，也就是这个eth0,eth1，在/etc/udev/rules.d/30-net_persistent_names.rules文件中有如下记录：

![suse_udev_net](/images/2013/07/suse_udev_net.png)

&nbsp;

&nbsp;

&nbsp;