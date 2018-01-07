---
title: socket基础知识
id: 140
categories:
  - 系统编程
date: 2009-09-05 17:52:00
tags:
---

    

<span style="font-size: x-small;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 如果一个socket成功创建了，它存在于一个地址一个地址家族(socket函数的第一个参数即为地址家族)，为什么要提供这个参数呢，因为Windows Sockets提供了一个与协议无关的编程接口，使得开发人员可以开发直接使用任何一种协议的网络程序。尽管如此，要实现网络通信定位和网络连接，为主机定址是必须得，bind函数就是干这个的。</span><span style="font-size: x-small;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 另外在调用send/sendto时如果这是一个未绑定的socket，那么Windows Sockets会执行一个隐式的bind调用，系统会给其分配一个唯一值，并标记为绑定状态，可以通过getsockname 获取相关scoket name。因此在bind之前没有send/sendto之类操作，那么调用recv/recvfrom会得到10022的错误代码，即&ldquo;提供了一个无效的参数&rdquo;，也就是在socket没有绑定一个ip/port之前是不能接受数据的，联想到我们一般的C/S模式，S端都会先bind到一个ip/port，因为S是服务提供者，他不会主动发送数据，而是等待C来要求服务也就是发送数据，所以我们一般在C端不需要显示调用bind而是当send/sendto之类函数调用时，由系统绑定一个唯一的ip/port。对于S端我们一般都是指定一个固定的ip/port，而C端，MSDN上面的建议是最好让系统自动分配一个，可以在调用bind的时候可以将port设为0以免发生冲突。</span>

&nbsp;

&nbsp;

</div>