---
title: thrift之Hello
id: 104
categories:
  - 系统编程
date: 2011-04-17 23:35:00
tags:
---

Thrift是一个开发跨语言服务的软件框架。编写thrift文件，通过自带的代码生成引擎即可生成各种语言(C++，Java，Python，PHP，Ruby，Erlang，C#等)的对应代码，下面以最经典的hello为例讲述，如何通过thrift编写跨语言的RPC程序：
1编写thrift文件，保存为hello.thrift：
<pre class="brush:cpp">service Hello
{
    void Hello()
}</pre>
2生成cpp和py框架文件
在hello.thrift文件所在目录执行：
thrift -r --gen cpp hello.thrift
thrift -r --gen py hello.thrift
会在当前目录下面产生两个文件夹，分别为gen-cpp和gen-py，
3编写cpp服务器端代码，拷贝gen-cpp目录中的Hello_server.skeleton.cpp到当前目录，重命名为CppServer.cpp，修改如下：
<pre class="brush:cpp">#include "Hello.h"
#include &lt;protocol/TBinaryProtocol.h&gt; 
#include &lt;server/TSimpleServer.h&gt;
#include &lt;transport/TServerSocket.h&gt; 
#include &lt;transport/TBufferTransports.h&gt; 

using namespace ::apache::thrift;
using namespace ::apache::thrift::protocol;
using namespace ::apache::thrift::transport;
using namespace ::apache::thrift::server;
using boost::shared_ptr;
class HelloHandler : virtual public HelloIf
{
public:
HelloHandler()
{
// Your initialization goes here
}
void Hello()
{
   // Your implementation goes here
    printf("Hello,Thrift/n");
}
};
int main(int argc, char **argv)
{
    int port = 9090;
    shared_ptr handler(new HelloHandler());
    shared_ptr processor(new HelloProcessor(handler));
    shared_ptr serverTransport(new TServerSocket(port));
    shared_ptr transportFactory(new TBufferedTransportFactory());
    shared_ptr protocolFactory(new TBinaryProtocolFactory());
    TSimpleServer server(processor, serverTransport, transportFactory, protocolFactory);

    server.serve();

    return 0;
}</pre>
4编写python客户端代码，PythonClient.py如下：
<pre class="brush:python">#!/usr/bin/env python
import sys
sys.path.append('./gen-py')
from hello import Hello
from hello.ttypes import *
from thrift import Thrift
from thrift.transport import TSocket
from thrift.transport import TTransport
from thrift.protocol import TBinaryProtocol

# Make socket
transport = TSocket.TSocket('localhost', 9090)
# Buffering is critical. Raw sockets are very slow
transport = TTransport.TBufferedTransport(transport)
# Wrap in a protocol
protocol = TBinaryProtocol.TBinaryProtocol(transport)
# Create a client to use the protocol encoder
client = Hello.Client(protocol)
# Connect!
transport.open()
# Call Server services
client.Hello()</pre>
5编写cpp端Makefile如下：
<pre class="brush:bash">BOOST_DIR = /usr/local/include/boost/
THRIFT_DIR = /usr/local/include/thrift
LIB_DIR = /usr/local/lib
GEN_SRC = ./gen-cpp/hello_types.cpp ./gen-cpp/Hello.cpp
default: server
server: CppServer.cpp
g++ -o CppServer -I${THRIFT_DIR} -I${BOOST_DIR} -I./gen-cpp -L${LIB_DIR} -lthrift CppServer.cpp ${GEN_SRC}
clean:
$(RM) -r CppServer</pre>
6编译cpp端，直接make即可生成相应的可执行文件，python端可以直接运行。

至此一个最简单的跨语言PRC程序即完成了，很简单吧。