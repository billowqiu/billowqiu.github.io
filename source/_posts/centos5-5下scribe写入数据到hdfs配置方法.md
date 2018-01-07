---
title: CentOS5.5下scribe写入数据到HDFS配置方法
id: 105
categories:
  - 系统编程
date: 2011-04-16 01:55:00
tags:
---

1按照[CentOS 5.5 下配置Hadoop 0.21单节点](http://blog.csdn.net/ToCpp/archive/2011/04/10/6314374.aspx)
一文中的方法配置Hadoop

2编译scribe，支持hdfs
2.1下载thrift,libevent,boost等库，可以都下载最新版本，基本上都是make &amp; make install
2.2下载最新版scribe-2.2，之前在网上看到说scribe有很多bug，必须在当前开发版本才能写入HDFS，试了好久没成功，也以为确实是代码的问题，现在发现不是这个问题，直接[下载](https://github.com/facebook/scribe)该版本
2.3在scribe源码包里面bootstrap.sh，按照文档上的说法是可以一步到位的即

./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H -I/data/billowqiu/jdk1.6.0_25/include -I/data/billowqiu/jdk1.6.0_25/include/linux -I/data/billowqiu/hadoop-0.20.1+169.89/src/c++/libhdfs" LDFLAGS="-L/data/billowqiu/hadoop-0.20.1+169.89/src/c++/libhdfs/lib -L/data/billowqiu/jdk1.6.0_25/jre/lib/amd64/server" --with-hadooppath=/data/billowqiu/hadoop-0.20.1+169.89 --enable-hdfs，实在不行直接修改Makefile也可以，先./configure --enable-hdfs，然后修改相应的src目录下面的Makefile，将以下字段修改如下：CPPFLAGS = -I/usr/local/lib/jdk1.6.0_23/include -I/usr/local/lib/jdk1.6.0_23/include/linux

上面的JDK路径以机器上面的实际路径为准。

貌似在静态链接boost会出现“undefined reference to `boost::system::generic_category()”，通过将将-lboost_system -lboost_filesystem放到链接库最后可以解决。

$ cd src $ g++ -Wall -O3 -L/usr/lib -o scribed store.o store_queue.o conf.o file.o conn_pool.o scribe_server.o network_dynamic_config.o dynamic_bucket_updater.o env_default.o -L/usr/local/lib -L/usr/local/lib -L/usr/local/lib -lfb303 -lthrift -lthriftnb -levent -lpthread libscribe.a libdynamicbucketupdater.a -lboost_system -lboost_filesystem

&nbsp;

2.4拷贝hadoop-0.21.0/hdfs/src/c++/libhdfs/hdfs.h文件到scribe/src目录下，libhdfs其实就是通过JNI让C/C++调用HDFS接口，在hadoop-0.21.0/hdfs/src/c++/libhdfs目录下面执行如下操作
./configure --enable-shared JVM_ARCH=tune=k8 --prefix=`pwd`/nativelib
./make install
这时会在nativelib/lib下面生成5个文件，将其都拷贝到/usr/local/lib下面，执行ldconfig
2.5编译scribe，在scribe/src目录下面执行./make，即可生成scribed文件，
2.6按照scribe/examples目录下面的配置文件写个简单的支持HDFS的配置文件simple_hdfs_example.conf：
port=1463
max_msg_per_second=2000000
check_interval=1
max_queue_size=100000000
num_thrift_server_threads=2
# DEFAULT - write all messages to hadoop
&lt;store&gt;
category=default
target_write_size=20480
type=file
fs_type=hdfs
file_path=hdfs://localhost:9000/scribedata
create_symlink=no
use_hostname_sub_directory=yes
base_filename=thisisoverwritten
max_size=1000000000
rotate_period=100s
add_newlines=1
&lt;/store&gt;
&lt;store&gt;
category=qt
target_write_size=20480
type=file
fs_type=hdfs
file_path=hdfs://localhost:9000/scribedata
create_symlink=no
use_hostname_sub_directory=yes
base_filename=thisisoverwritten
max_size=1000000000
rotate_period=100s
add_newlines=1
&lt;/store&gt;
2.7到处libhdfs库需要使用的jar路径即CLASSPATH，具体需要哪些不太清楚，
官方文档上建议将hadoop/lib目录下所有的库的加入，下面是我导出的：
export CLASSPATH=$CLASSPATH:/xxx/hadoop-0.21.0/hadoop-common-0.21.0.jar:/xxx/hadoop-0.21.0/hadoop-hdfs-0.21.0.jar:/xxx/hadoop-0.20.1+152/contrib/scribe-log4j/hadoop-0.20.1+152-scribe-log4j.jar:/xxx/hadoop-0.21.0/lib/commons-logging-1.1.1.jar:/xxx/hadoop-0.21.0/lib/commons-logging-api-1.1.jar:/xxx/hadoop-0.21.0/lib/core-3.1.1.jar:/xxx/hadoop-0.21.0/lib/log4j-1.2.15.jar
XXX你懂的。

2.8可以执行scribe了， ./scribed ../examples/simple_hdfs_example.conf
2.9通过发送工具发送日志到scribe，echo "Successful write data to HDFS,I am qiutao" | ./scribe_cat qt
3.10在hadoop目录下面执行bin/hadoop dfs -lsr /scribedata即可看到相应的数据生成了。
整个过程确实比较繁琐，尤其要注意2.4，一定要在自己机器上面编译libhdfs库，否则会出现一些莫名其妙的问题，
基本上都是抛出的java异常，在这个上面吃了不少亏，终于在自己机器上面写入HDFS了，一步步的也学到了不少东西，
留作以后备用。