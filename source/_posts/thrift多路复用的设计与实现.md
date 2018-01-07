---
title: Thrift多路复用的设计与实现
tags:
  - thrift rpc design
id: 689
categories:
  - C++
  - 设计模式
date: 2013-10-20 17:15:59
---

<div>

# 介绍

Thrift<span style="font-family: 微软雅黑;">作为一个跨语言的</span><span style="font-family: Times New Roman;">rpc</span><span style="font-family: 微软雅黑;">框架，为后端服务间的多语言混合开发提供了高可靠，可扩展，以及高效的实现。但是自从</span><span style="font-family: Times New Roman;">2007</span><span style="font-family: 微软雅黑;">年</span><span style="font-family: Times New Roman;">facebook</span><span style="font-family: 微软雅黑;">开源后的</span><span style="font-family: Times New Roman;">6</span><span style="font-family: 微软雅黑;">年时间内一直缺少一个多路复用的功能</span><span style="font-family: Times New Roman;">(</span>Multiplexing Services)<span style="font-family: 微软雅黑;">，也就是在一个</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">上面实现多个</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">调用。比如要实现一个后端服务，很多时候不仅仅只是实现业务服务接口，往往还会预留一些调试，监控服务接口，以往要实现多个</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">在一个</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">上面调用，要么将多个服务糅合成一个庞大的服务（图</span><span style="font-family: Times New Roman;">1</span><span style="font-family: 微软雅黑;">），要么将多个</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">分摊到不同的</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">上（图</span><span style="font-family: Times New Roman;">2</span><span style="font-family: 微软雅黑;">）。最近的版本</span><span style="font-family: Times New Roman;">0.9.1</span><span style="font-family: 微软雅黑;">终于实现内置的多路复用，本文将就该功能简要分析一下该功能的设计与实现，并提供一个简单的示例。</span>

[![thrift-monolithic-interface](/images/2013/10/thrift-monolithic-interface.png)](/images/2013/10/thrift-monolithic-interface.png)

图1

[![thrift-multi-server](/images/2013/10/thrift-multi-server.png)](/images/2013/10/thrift-multi-server.png)

图2

&nbsp;

## 设计

Thrift<span style="font-family: 微软雅黑;">的架构如下：</span>

[![thrift-arch](/images/2013/10/thrift-arch.png)](/images/2013/10/thrift-arch.png)

Thrift<span style="font-family: 微软雅黑;">采取了很优雅的分层设计，下面简述各层的主要功能：</span>

Ø Transport

负责传输数据的接口，有文件，内存，套接字等实现。

Ø Protocol

负责数据的协议交互接口，有二进制，<span style="font-family: Times New Roman;">Json</span><span style="font-family: 微软雅黑;">等实现。</span>

Ø Processor

负责输入输出数据处理的接口，其实就是对<span style="font-family: Times New Roman;">Protocol</span><span style="font-family: 微软雅黑;">的处理。</span>

Ø Server

包括了上面各层的创建和管理，并提供网络服务的功能，网络服务这块目前有四个实现，分别是最简单的单线程阻塞，多线程阻塞，线程池阻塞和基于<span style="font-family: Times New Roman;">libevent</span><span style="font-family: 微软雅黑;">的非阻塞模式。</span>

实现多路复用有以下几个需要注意的地方：

Ø 不修改现有的<span style="font-family: Times New Roman;">Thrift</span><span style="font-family: 微软雅黑;">代码，主要是指向后兼容，旧代码可以在新版本中编译</span>

Ø 不修改[白皮书](http://thrift.apache.org/static/files/thrift-20070401.pdf)上说的协议

Ø 不依赖任何<span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">层代码，也不需要新的</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">实现</span>

0.9.1<span style="font-family: 微软雅黑;">之前版本不能多个</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">在同一个</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">上面调用，主要是因为在协议层只把</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">的函数名打包，没有将</span><span style="font-family: Times New Roman;">ServiceName</span><span style="font-family: 微软雅黑;">打包进去，所以在</span><span style="font-family: Times New Roman;">Processor</span><span style="font-family: 微软雅黑;">层默认只能处理一个</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">。</span>

新版本中通过新增以下设计完成了<span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">的多路复用：</span>

[![thrift-multiplex-arch](/images/2013/10/thrift-multiplex-arch.png)](/images/2013/10/thrift-multiplex-arch.png)

[![thrift-multiplex](/images/2013/10/thrift-multiplex.png)](/images/2013/10/thrift-multiplex.png)

深色部分为新增部分，需要多个<span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">复用</span><span style="font-family: Times New Roman;">Server</span><span style="font-family: 微软雅黑;">的客户端需要使用新的</span><span style="font-family: Times New Roman;">Protocol</span><span style="font-family: 微软雅黑;">实现，支持</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">复用的服务端需要通过新的</span><span style="font-family: Times New Roman;">Processor</span><span style="font-family: 微软雅黑;">实现注册不同的</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">。</span>

## 实现

从<span style="font-family: Times New Roman;">release-notes</span><span style="font-family: 微软雅黑;">看到目前已经有</span><span style="font-family: Times New Roman;">Java</span><span style="font-family: 微软雅黑;">，</span><span style="font-family: Times New Roman;">Delphi</span><span style="font-family: 微软雅黑;">，</span><span style="font-family: Times New Roman;">C#</span><span style="font-family: 微软雅黑;">，</span><span style="font-family: Times New Roman;">C++</span><span style="font-family: 微软雅黑;">几个语言实现了该功能，具体到</span><span style="font-family: Times New Roman;">C++</span><span style="font-family: 微软雅黑;">实现按照</span>[THRIFT-1902](https://issues.apache.org/jira/browse/THRIFT-1902)的介绍，是直接移植的<span style="font-family: Times New Roman;">java</span><span style="font-family: 微软雅黑;">版本。</span>

### 新增代码

protocol/TMultiplexedProtocol.h

protocol/TMultiplexedProtocol.cpp

protocol/TProtocolDecorator.h

processor/TMultiplexedProcessor.h

### 主要逻辑

1) TMultiplexedProtocol类重写writeMessageBegin_virt方法，在Service<span style="font-family: 微软雅黑;">的函数名前新增了</span><span style="font-family: Times New Roman;">ServiceName</span><span style="font-family: 微软雅黑;">，并附带了一个分隔符；该类采取了</span>decorator模式将绝大部分操作转发到实际的protocol<span style="font-family: 微软雅黑;">对象。</span>

2) TMultiplexedProcessor类增加了一个存放service<span style="font-family: 微软雅黑;">的</span><span style="font-family: Times New Roman;">map</span><span style="font-family: 微软雅黑;">，</span>

typedef std::map&lt; std::string, shared_ptr&lt;TProcessor&gt; &gt; services_t;

key为ServiceName，这样在process方法中先解出TMultiplexedProtocol传递过来的ServiceName，通过该key查找到对应的Processor对象，再调用该Processor的process方法，这样就完美的实现了多个serivice的共存。

## 使用示例

IDL:
<pre class="brush:other">namespace cpp thrift.multiplex.demo

service FirstService
{
    void blahBlah()
}

service SecondService
{
    void blahBlah()
}</pre>
Server:
<pre class="brush:cpp">int port = 9090;
shared_ptr&lt;TProcessor&gt; processor1(new FirstServiceProcessor
         (shared_ptr&lt;FirstServiceHandler&gt;(new FirstServiceHandler())));
shared_ptr&lt;TProcessor&gt; processor2(new SecondServiceProcessor
         (shared_ptr&lt;SecondServiceHandler&gt;(new SecondServiceHandler())));

//使用MultiplexedProcessor
shared_ptr&lt;TMultiplexedProcessor&gt; processor(new TMultiplexedProcessor());

//注册各自的Service
processor-&gt;registerProcessor("FirstService", processor1);
processor-&gt;registerProcessor("SecondService", processor2);

shared_ptr&lt;TServerTransport&gt; serverTransport(new TServerSocket(port));
shared_ptr&lt;TTransportFactory&gt; transportFactory(new TBufferedTransportFactory());
shared_ptr&lt;TProtocolFactory&gt; protocolFactory(new TBinaryProtocolFactory());
TSimpleServer server(processor, serverTransport, transportFactory, protocolFactory);
server.serve();</pre>
Client:
<pre class="brush:cpp">shared_ptr&lt;TSocket&gt; transport(new TSocket("localhost", 9090));
transport-&gt;open();

shared_ptr&lt;TBinaryProtocol&gt; protocol(new TBinaryProtocol(transport));
shared_ptr&lt;TMultiplexedProtocol&gt; mp1(new TMultiplexedProtocol(protocol, "FirstService"));
shared_ptr&lt;FirstServiceClient&gt; service1(new FirstServiceClient(mp1));
shared_ptr&lt;TMultiplexedProtocol&gt; mp2(new TMultiplexedProtocol(protocol, "SecondService"));
shared_ptr&lt;SecondServiceClient&gt; service2(new SecondServiceClient(mp2));

service1-&gt;blahBlah();
service2-&gt;blahBlah();</pre>

## 总结

Thrift<span style="font-family: 微软雅黑;">通过在客户端采取的</span>decorator模式巧妙的在协议层将ServiceName<span style="font-family: 微软雅黑;">传递到服务端，服务端通过常见的手段将</span><span style="font-family: Times New Roman;">ServiceName</span><span style="font-family: 微软雅黑;">和具体的</span><span style="font-family: Times New Roman;">Processor</span><span style="font-family: 微软雅黑;">进行映射，完美的解决了</span><span style="font-family: Times New Roman;">Service</span><span style="font-family: 微软雅黑;">的多路复用问题。</span>

## 参考资料

1. [http://thrift.apache.org/docs/concepts/](http://thrift.apache.org/docs/concepts/)

2. [https://issues.apache.org/jira/browse/THRIFT-1902](https://issues.apache.org/jira/browse/THRIFT-1902)

3. [https://issues.apache.org/jira/browse/THRIFT-563](https://issues.apache.org/jira/browse/THRIFT-563)

4. [http://bigdata.impetus.com/whitepaper](http://bigdata.impetus.com/whitepaper)

</div>