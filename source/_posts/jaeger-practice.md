---
title: jaeger实战
date: 2019-04-11 20:12:56
categories:
  - microservice
tags: tracing
---
做tracing选型时发现除了Zipkin(这个其实很久之前就有所闻，但是一直没实操过)，还有Uber开源的[Jaeger](https://www.jaegertracing.io)，看到官网就感觉这是一个用心的产品，这个应该离不开Uber有专门的团队维护。

#### 特性
官网列了几个特性：
- Opentracing兼容，提供了Go, Java, Node, Python, C++的官方客户端
- 服务之间使用一致的采样逻辑
- 多存储支持：Cassandra, Elasticsearch, Kafka, memory
- 动态采样(吐槽一下这个，一年多了，还没合入开源版本)
- 支持收集的数据做后续分析
- 现代化的UI
- 云原生支持
- 可视化(每个组建都支持将metric导出到Prometheus)
- 兼容Zipkin

由于调研的时候没法对细节做深入了解，当时选择Jaeger最重要的一点其实是其实是：**jaeger-agent是一个通过UDP方式接收本机发出的spans。**
为什么是这一点呢？因为对于业务来说tracing其实是一个相对无关紧要的东西，如果给一个TCP的通道让它上报，其实是不太友好的，性能和维护性方面都没有UDP好。

#### 架构

![Jaeger架构图](https://www.jaegertracing.io/img/architecture-v1.png)

- jaeger-client：嵌入在应用程序里面，负责span的创建以及上报
- jaeger-agent：每个物理机部署一个，负责收集client上报的span，然后转发给collector
- jaeger-collector：接收agent发来的span，写入后端存储
- jaeger-query：提供rest接口，负责从存储中拉取trace信息供UI查询
- jaeger-ui：展示trace和服务依赖图

#### 实战
如果想快速体验，可以按照官网的[Docker](https://www.jaegertracing.io/docs/1.11/deployment/)方式部署，由于当时的线上环境还没有这方面的支持，下面以最新的Release二进制版本简述一下部署的流程。

##### 准备工作
- Jaeger安装包: 
    https://github.com/jaegertracing/jaeger/releases/download/v1.11.0/jaeger-1.11.0-darwin-amd64.tar.gz
- elasticsearch-6.4.0: 
    https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.tar.gz

##### 启动
1. bin/elasticsearch
2. SPAN_STORAGE_TYPE=elasticsearch ./jaeger-collector
3. ./jaeger-agent --collector.host-port 127.0.0.1:14267
4. SPAN_STORAGE_TYPE=elasticsearch ./jaeger-query

打开http://127.0.0.1:16686即可看到Jaeger的查询页面：
![](/images/2019/04/jaeger-ui.png "jaeger-ui")

- Search，按照 服务:操作，tag，时间等维度进行trace的筛选
- Compare，比较两个trace
- Dependencies，查看服务依赖图，这个需要一个单独的Spark任务才能生成，下文会讲到

##### 例子
Jaeger的安装包里面自带了一个测试程序:example-hotrod，模拟Uber的打车服务，通过如下方式启动：

    jaeger-1.11.0-darwin-amd64 billowqiu$ ./example-hotrod all

这样会一次性启动所有的服务，包括：Customer service，Driver service，Route service，Frontend service，同时内部还会模拟调用MySQL和Redis。
启动该服务后，打开http://127.0.0.1:8080/ 随便点一个乘客姓名，这时在Jaeger的ui里面可以看到如下trace：
![](/images/2019/04/hotrod_trace.png "hotrod_trace")

点进去可以看到详细的trace：
![](/images/2019/04/hotrod_trace_detail.png "hotrod_trace_detail")

##### Dependencies生成
上面通过例子程序可以看到6个服务的调用流程及耗时等trace数据，但是http://127.0.0.1:16686/dependencies 里面依然是空的，这是因为服务依赖图需要单独的任务生成：
    [jaegertracing/spark-dependencies](https://github.com/jaegertracing/spark-dependencies)， 当时为了解决一个生产环境的问题，还为这个项目提了个[pr](https://github.com/jaegertracing/spark-dependencies/pull/39)，最后官方合入了master，也算是一个小惊喜吧。
代码clone下来之后执行：

        spark-dependencies billowqiu$ mvn clean package -Dmaven.test.skip=true

这样会build一个jar包，然后执行：

    spark-dependencies billowqiu$ STORAGE=elasticsearch ES_NODES=http://localhost:9200 java -jar jaeger-spark-dependencies/target/jaeger-spark-dependencies-0.0.1-SNAPSHOT.jar

在Mac下面可能会出现：Can't assign requested address: Service 'sparkDriver' failed after 16 retries (starting from 0) 这个错误，可以参照[这个](https://stackoverflow.com/questions/34601554/mac-spark-shell-error-initializing-sparkcontext)解决。
这时再打开http://127.0.0.1:16686/dependencies 选择DAG，服务依赖如图所示：

![](/images/2019/04/hotrod_trace_dependencies.png "hotrod_trace_dependencies")
