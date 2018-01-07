---
title: 长连接和Keepalive
id: 123
categories:
  - 系统编程
date: 2010-03-01 23:32:00
tags:
---

    

原文链接:[http://www.cppblog.com/zhangyq/archive/2010/02/28/108615.html](http://www.cppblog.com/zhangyq/archive/2010/02/28/108615.html)

<span style="font-family: arial black,avant garde;"><span style="font-size: medium;"><span style="background-color: #000000;"><span style="color: #ff00ff;"><span style="background-color: #ffffff;">之前一个项目的服务器端好像就是在待机之后，tcp链接已经断开，但是服务器没有检测到，看了这篇文章之后就比较明了了。</span></span></span></span></span>

&nbsp;

TCP协议中有长连接和短连接之分。短连接在数据包发送完成后就会自己断开，长连接在发包完毕后，会在一定的时间内保持连接，即我们通常所说的Keepalive（存活定时器）功能。
默认的Keepalive超时需要7,200,000 milliseconds，即2小时，探测次数为5次。它的功效和用户自己实现的心跳机制是一样的。<span style="font-family: 宋体; font-size: 10.5pt; mso-bidi-font-family: 宋体; mso-ansi-language: EN-US; mso-fareast-language: ZH-CN; mso-bidi-language: AR-SA;">开启<span lang="EN-US">Keepalive</span>功能需要消耗额外的宽带和流量，尽管这微不足道，但在按流量计费的环境下增加了费用，另一方面，<span lang="EN-US">Keepalive</span>设置不合理时可能会因为短暂的网络波动而断开健康的<span lang="EN-US">TCP</span>连接。
</span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">keepalive</span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">并不是<span lang="EN-US">TCP</span>规范的一部分。在<span lang="EN-US">Host Requirements RFC</span>罗列有不使用它的三个理由：（<span lang="EN-US">1</span>）在短暂的故障期间，它们可能引起一个良好连接（<span lang="EN-US">good connection</span>）被释放（<span lang="EN-US">dropped</span>），（<span lang="EN-US">2</span>）它们消费了不必要的宽带，（<span lang="EN-US">3</span>）在以数据包计费的互联网上它们（额外）花费金钱。然而，在许多的实现中提供了存活定时器。</span></span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US"><span style="font-family: 宋体;">
<p class="MsoPlainText" style="margin: 0cm 0cm 0pt;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">一些服务器应用程序可能代表客户端占用资源，它们需要知道客户端主机是否崩溃。存活定时器可以为这些应用程序提供探测服务。<span lang="EN-US">Telnet</span>服务器和<span lang="EN-US">Rlogin</span>服务器的许多版本都默认提供存活选项。
</span>
<p class="MsoPlainText" style="margin: 0cm 0cm 0pt;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">个人计算机用户使用<span lang="EN-US">TCP/IP</span>协议通过<span lang="EN-US">Telnet</span>登录一台主机，这是能够说明需要使用存活定时器的一个常用例子。如果某个用户在使用结束时只是关掉了电源，而没有注销（<span lang="EN-US">log off</span>），那么他就留下了一个半打开（<span lang="EN-US">half-open</span>）的连接。如果客户端消失，留给了服务器端半打开的连接，并且服务器又在等待客户端的数据，那么等待将永远持续下去。存活特征的目的就是在服务器端检测这种半打开连接。
</span>
<p class="MsoPlainText" style="margin: 0cm 0cm 0pt;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">也可以在客户端设置存活器选项，且没有不允许这样做的理由，但通常设置在服务器。如果连接两端都需要探测对方是否消失，那么就可以在两端同时设置（比如<span lang="EN-US">NFS</span>）。<span lang="EN-US"></span></span>

</p>
</p>
</span></span>

<span lang="EN-US"></span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">keepalive</span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">工作原理：</span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">若在一个给定连接上，两小时之内无任何活动，服务器便向客户端发送一个探测段。（我们将在下面的例子中看到探测段的样子。）客户端主机必须是下列四种状态之一：</span></span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">1) </span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">客户端主机依旧活跃（<span lang="EN-US">up</span>）运行，并且从服务器可到达。从客户端<span lang="EN-US">TCP</span>的正常响应，服务器知道对方仍然活跃。服务器的<span lang="EN-US">TCP</span>为接下来的两小时复位存活定时器，如果在这两个小时到期之前，连接上发生应用程序的通信，则定时器重新为往下的两小时复位，并且接着交换数据。<span lang="EN-US"></span></span></span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">2) </span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">客户端已经崩溃，或者已经关闭（<span lang="EN-US">down</span>），或者正在重启过程中。在这两种情况下，它的<span lang="EN-US">TCP</span>都不会响应。服务器没有收到对其发出探测的响应，并且在<span lang="EN-US">75</span>秒之后超时。服务器将总共发送<span lang="EN-US">10</span>个这样的探测，每个探测<span lang="EN-US">75</span>秒。如果没有收到一个响应，它就认为客户端主机已经关闭并终止连接。</span></span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">3) </span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">客户端曾经崩溃，但已经重启。这种情况下，服务器将会收到对其存活探测的响应，但该响应是一个复位，从而引起服务器对连接的终止。</span></span>

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">4) </span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">客户端主机活跃运行，但从服务器不可到达。这与状态<span lang="EN-US">2</span>类似，因为<span lang="EN-US">TCP</span>无法区别它们两个。它所能表明的仅是未收到对其探测的回复。<span lang="EN-US"></span></span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US"><span style="font-family: 宋体;">&nbsp;</span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">服务器不必担心客户端主机被关闭然后重启的情况（这里指的是操作员执行的正常关闭，而不是主机的崩溃）。当系统被操作员关闭时，所有的应用程序进程（也就是客户端进程）都将被终止，客户端<span lang="EN-US">TCP</span>会在连接上发送一个<span lang="EN-US">FIN</span>。收到这个<span lang="EN-US">FIN</span>后，服务器<span lang="EN-US">TCP</span>向服务器进程报告一个文件结束，以允许服务器检测这种状态。</span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">在第一种状态下，服务器应用程序不知道存活探测是否发生。凡事都是由<span lang="EN-US">TCP</span>层处理的，存活探测对应用程序透明，直到后面<span lang="EN-US">2</span>，<span lang="EN-US">3</span>，<span lang="EN-US">4</span>三种状态发生。在这三种状态下，通过服务器的<span lang="EN-US">TCP</span>，返回给服务器应用程序错误信息。（通常服务器向网络发出一个读请求，等待客户端的数据。如果存活特征返回一个错误信息，则将该信息作为读操作的返回值返回给服务器。）在状态<span lang="EN-US">2</span>，错误信息类似于&ldquo;连接超时&rdquo;。状态<span lang="EN-US">3</span>则为&ldquo;连接被对方复位&rdquo;。第四种状态看起来像连接超时，或者根据是否收到与该连接相关的<span lang="EN-US">ICMP</span>错误信息，而可能返回其它的错误信息。

<span style="font-family: 宋体; font-size: 10.5pt; mso-bidi-font-family: 宋体; mso-ansi-language: EN-US; mso-fareast-language: ZH-CN; mso-bidi-language: AR-SA;" lang="EN-US">linux</span>
<p class="MsoPlainText" style="margin: 0cm 0cm 0pt;"><span style="font-family: 宋体; font-size: 10.5pt; mso-bidi-font-family: 宋体; mso-ansi-language: EN-US; mso-fareast-language: ZH-CN; mso-bidi-language: AR-SA;">内核包含对<span lang="EN-US">keepalive</span>的支持。其中使用了三个参数：<span lang="EN-US">tcp_keepalive_time</span>（开启<span lang="EN-US">keepalive</span>的闲置时 长）<span lang="EN-US">tcp_keepalive_intvl</span>（<span lang="EN-US">keepalive</span>探测包的发送间隔）和<span lang="EN-US">tcp_keepalive_probes </span>（如果对方不予应答，探测包的发送次数）；在liunx中，<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">keepalive</span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">是一个开关选项，可以通过函数来使能。具体地说，可以使用以下代码：
<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">setsockopt(rs, SOL_SOCKET, SO_KEEPALIVE, (void *)&amp;keepAlive, sizeof(keepAlive));</span>
</span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US"></span></span>

</span></span></p>

&nbsp;

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">当<span lang="EN-US">tcp</span>检测到对端<span lang="EN-US">socket</span>不再可用时<span lang="EN-US">(</span>不能发出探测包<span lang="EN-US">,</span>或探测包没有收到<span lang="EN-US">ACK</span>的响应包<span lang="EN-US">),select</span>会返回<span lang="EN-US">socket</span>可读<span lang="EN-US">,</span>并且在<span lang="EN-US">recv</span>时返回<span lang="EN-US">-1,</span>同时置上<span lang="EN-US">errno</span>为<span lang="EN-US">ETIMEDOUT。此时TCP的状态是断开的。</span></span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">keepalive</span>参数设置代码如下：

<div style="padding-bottom: 4px; background-color: #eeeeee; padding-left: 4px; width: 98%; padding-right: 5px; font-size: 13px; word-break: break-all; padding-top: 4px; border: #cccccc 1px solid;"><span style="color: #008000;">//</span><span style="color: #008000;">&nbsp;开启KeepAlive</span><span style="color: #008000;">
</span><span style="color: #000000;">BOOL&nbsp;bKeepAlive&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;TRUE;
</span><span style="color: #0000ff;">int</span><span style="color: #000000;">&nbsp;nRet&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;::setsockopt(socket_handle,&nbsp;SOL_SOCKET,&nbsp;SO_KEEPALIVE,&nbsp;(</span><span style="color: #0000ff;">char</span><span style="color: #000000;">*</span><span style="color: #000000;">)</span><span style="color: #000000;">&amp;</span><span style="color: #000000;">bKeepAlive,&nbsp;</span><span style="color: #0000ff;">sizeof</span><span style="color: #000000;">(bKeepAlive));
</span><span style="color: #0000ff;">if</span><span style="color: #000000;">&nbsp;(nRet&nbsp;</span><span style="color: #000000;">==</span><span style="color: #000000;">&nbsp;SOCKET_ERROR)
{
</span><span style="color: #0000ff;">return</span><span style="color: #000000;">&nbsp;FALSE;
}

</span><span style="color: #008000;">//</span><span style="color: #008000;">&nbsp;设置KeepAlive参数</span><span style="color: #008000;">
</span><span style="color: #000000;">tcp_keepalive&nbsp;alive_in&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;{</span><span style="color: #000000;">0</span><span style="color: #000000;">};
tcp_keepalive&nbsp;alive_out&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;{</span><span style="color: #000000;">0</span><span style="color: #000000;">};
alive_in.keepalivetime&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;</span><span style="color: #000000;">5000</span><span style="color: #000000;">;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #008000;">//</span><span style="color: #008000;">&nbsp;开始首次KeepAlive探测前的TCP空闭时间</span><span style="color: #008000;">
</span><span style="color: #000000;">alive_in.keepaliveinterval&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;</span><span style="color: #000000;">1000</span><span style="color: #000000;">;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #008000;">//</span><span style="color: #008000;">&nbsp;两次KeepAlive探测间的时间间隔</span><span style="color: #008000;">
</span><span style="color: #000000;">alive_in.onoff&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;TRUE;
unsigned&nbsp;</span><span style="color: #0000ff;">long</span><span style="color: #000000;">&nbsp;ulBytesReturn&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;</span><span style="color: #000000;">0</span><span style="color: #000000;">;
nRet&nbsp;</span><span style="color: #000000;">=</span><span style="color: #000000;">&nbsp;WSAIoctl(socket_handle,&nbsp;SIO_KEEPALIVE_VALS,&nbsp;</span><span style="color: #000000;">&amp;</span><span style="color: #000000;">alive_in,&nbsp;</span><span style="color: #0000ff;">sizeof</span><span style="color: #000000;">(alive_in),
</span><span style="color: #000000;">&amp;</span><span style="color: #000000;">alive_out,&nbsp;</span><span style="color: #0000ff;">sizeof</span><span style="color: #000000;">(alive_out),&nbsp;</span><span style="color: #000000;">&amp;</span><span style="color: #000000;">ulBytesReturn,&nbsp;NULL,&nbsp;NULL);
</span><span style="color: #0000ff;">if</span><span style="color: #000000;">&nbsp;(nRet&nbsp;</span><span style="color: #000000;">==</span><span style="color: #000000;">&nbsp;SOCKET_ERROR)
{
</span><span style="color: #0000ff;">return</span><span style="color: #000000;">&nbsp;FALSE;
}
</span></div>

开启Keepalive选项之后，对于使用IOCP模型的服务器端程序来说，一旦检测到连接断开，GetQueuedCompletionStatus函数将立即返回FALSE，使得服务器端能及时清除该连接、释放该连接相关的资源。对于使用select模型的客户端来说，连接断开被探测到时，以recv目的阻塞在socket上的select方法将立即返回SOCKET_ERROR，从而得知连接已失效，客户端程序便有机会及时执行清除工作、提醒用户或重新连接。

<span style="font-family: 宋体;"><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;" lang="EN-US">TCP</span><span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;">连接非正常断开的检测<span lang="EN-US">(KeepAlive</span>探测<span lang="EN-US">)</span></span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">此处的&rdquo;非正常断开&rdquo;指<span lang="EN-US">TCP</span>连接不是以优雅的方式断开<span lang="EN-US">,</span>如网线故障等物理链路的原因<span lang="EN-US">,</span>还有突然主机断电等原因<span lang="EN-US"></span></span></span>

<span style="mso-bidi-font-family: 宋体; mso-hansi-font-family: 宋体;"><span style="font-family: 宋体;">有两种方法可以检测<span lang="EN-US">:1.TCP</span>连接双方定时发握手消息<span lang="EN-US"> 2.</span>利用<span lang="EN-US">TCP</span>协议栈中的<span lang="EN-US">KeepAlive</span>探测<span lang="EN-US"></span></span></span>

<span style="font-family: 宋体; font-size: 10.5pt; mso-bidi-font-family: 宋体; mso-ansi-language: EN-US; mso-fareast-language: ZH-CN; mso-bidi-language: AR-SA;">第二种方法简单可靠<span lang="EN-US">,</span>只需对<span lang="EN-US">TCP</span>连接两个<span lang="EN-US">Socket</span>设定<span lang="EN-US">KeepAlive</span>探测。</span>

</p>
</div>