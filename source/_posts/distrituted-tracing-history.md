---
title: 分布式跟踪系统历史
date: 2018-10-21 23:23:27
categories:
  - microservice
tags: tracing
---
#### 介绍
从五月份到现在一直都在和tracing打交道，也算对这个领域有了一定的了解，打算做点记录，第一篇就从tracing系统的历史说起。

#### tracing系统历史
Google在互联网领域基础设施方面的理论贡献真的是当之无愧的No.1，从早期的三驾马车，到tracing相关开源系统的鼻祖[Dapper](https://ai.google/research/pubs/pub36356)，再到近年的spanner，F1等等，几乎都是发个paper科普一下自家已经运营N年的东西，然后开源领域就各种开种"山寨"。
调用链跟踪这个话题，其实很早就有了，具体可以去翻看一下dapper的引用文献，但是最近两年因为微服务的火热，各大公司才开始真正对这个东西重视，毕竟一个接口的调用可能引起几十个甚至上百个下游服务的调用，如果没有一个跟踪系统，对于问题的定位难度可想而知。

- Dapper论文发表于2010年，[翻译版](http://bigbully.github.io/Dapper-translation/)
- Twitter在2012年开源了[zipkin](https://blog.twitter.com/engineering/en_us/a/2012/distributed-systems-tracing-with-zipkin.html)
- zipkin开源后
    - 估计Twitter维护不力，而且是相对小众的scale写的，后面由开源社区用java重写了并改名为[OpenZipkin](https://zipkin.io)继续维护维护
    - 7月份完成了2.0的数据模型切换[Zipkin 2.10 completes our v2 migration](https://github.com/openzipkin/zipkin/releases/tag/2.10.1)
    - 8.30号捐给了[Apache](http://incubator.apache.org/projects/zipkin.html)。
- Jaeger的诞生
    - Uber在集成到内部系统之后各种坑出来了，具体可见[Evolving Distributed Tracing at Uber Engineering](https://eng.uber.com/distributed-tracing/)，于是另起炉灶于2017年初开源了[Jaeger](https://www.jaegertracing.io)


#### tracing标准

- [opentracing](https://opentracing.io/)

    2016年就开始启动了，为了解决tracing系统各自为政，搞了个统一的标准，并提供了各种语言的api。目前的主要参与者为Uber，LightStep(Dapper第一作者的公司)，Redhat(Jaeger开源后，直接放弃自家研发的类似产品，转而到Jaeger上面来了，哈)等公司。

- [opencensus](https://opencensus.io/)
    
    Google年初提出的一个集metrics和tracing的新标准，估计一方面是看到Opentracing想竞争一把，就像之前的容器编排一样，另外也可以推广自己家的[Stackdriver](https://cloud.google.com/stackdriver/)，目前[微软](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/)也加入了对该标准的支持，准备搞到自己的Auze里面。

- [w3c/distributed-tracing](https://github.com/w3c/distributed-tracing)
    
    2017年提出的一个为了统一各种tracer系统中Context格式标准，目前还是Draft阶段，主要参与者为Google和微软。

#### tracer实现

上面提到了三个tracing标准，w3c的就不说了，opencensus由于是谷歌主导的，参与者不多，相应的tracer实现也只有自家的Stackdriver。
opentracing实现则是[遍地开花](https://opentracing.io/docs/supported-tracers/)：
- Jaeger
- Appdash
- LightStep
- Instana
- SkyWalking
- inspectIT
- stagemonitor
- Datadog

其中大部分都是商业产品，顺带支持一下opentracing，而且都是属于APM级别的，Zipkin竟然不在名列！！！
开源方面的，Jaeger和Appdash都是go语言开发的，部署起来相对比较轻便；而且Jaeger背后有Uber支持，提供了个各种语言的API，这个也是当时技术选型时选择的一个点，同时其基于UDP方式的Agent数据采集也比较符合个人的胃口。

#### 总结
随着微服务的流行，传统的单体应用监控已经不能满足大规模分布式服务之间的问题定位，分布式跟踪刚好能解决这个问题，而Opentracing的出现统一了各个Tracer系统的接入标准，让开发者在选择的时候不用绑定在某一个厂商。由于tracing和monitoring在实现和用途上有一定的重叠，集tracing和monitoring的APM应该是该领域的一个趋势，这也是opencensus的一大优势。