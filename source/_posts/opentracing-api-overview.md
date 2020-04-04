---
title: opentracing-api-overview
date: 2018-11-11 21:27:00
categories:
  - microservice
tags: tracing
---
### 简介
opentracing是一个API规范，各类框架和库负责实现该规范，从而允许开发人员通过手动埋点实现平台无关的tracing跟踪。目前主流的语言都有对应的接口提供，具体可以参考[链接](https://opentracing.io/docs/supported-languages)。下面以opentracing-go为例对其做一个大致介绍。

### Span
#### SpanContext
表示一个Span的状态，通常会传递给相关联的Span，也包括跨进程传递，例如<trace_id, span_id, sampled>。主要接口有：
ForeachBaggageItem(handler func(k, v string) bool)，handler函数会针对每个BaggageItem进行调用。

#### Baggage
跟spancontext一样也是跨进程传递的，为一系列key：value对

#### Tag
设置用于过滤span的tags，其中key必须是string类型，value可以是任意类型，opentracing有一些预定义的[tags](https://github.com/opentracing/specification/blob/master/semantic_conventions.md)
```
SetTag(key string, value interface{}) Span
span.SetTag()
```

#### Log

```
LogFields(fields ...log.Field)
//    span.LogFields(
//        log.String("event", "soft error"),
//        log.String("type", "cache timeout"),
//        log.Int("waited.millis", 1500))
```

#### Finish

```
FinishWithOptions(opts FinishOptions)
通常这是对于span的最后一个操作，目前看到的实现基本上都是在这个动作做span的上报。

type FinishOptions struct {
    FinishTime time.Time    // 重写span的结束时间
    LogRecords []LogRecord  // 可以在这个时候一次性写入所有的Log,效果同手动调用LogFields
}
```



一个span的列子:

    t=0            operation name: db_query               t=x 

     +-----------------------------------------------------+
     | · · · · · · · · · ·    Span     · · · · · · · · · · |
     +-----------------------------------------------------+

Tags:
- db.instance:"jdbc:mysql://127.0.0.1:3306/customers
- db.statement: "SELECT * FROM mytable WHERE foo='bar';"

Logs:
- message:"Can't connect to mysql server on '127.0.0.1'(10061)"

SpanContext:
- trace_id:"abc123"
- span_id:"xyz789"
- Baggage Items:
  - special_id:"vsid1738"

---

### Tracer
负责创建Span和SpanContext的传播，属于一个Manager角色。


```
type SpanReference struct {
    // 与ReferencedContext的关联关系，有ChildOfRef和FollowsFromRef，具体可见http://opentracing.io/spec/
	Type              SpanReferenceType 
	// 需要建立关联关系的spancontext
	ReferencedContext SpanContext
}
type StartSpanOptions struct {
	// 一次可以设置0到多个span之间的关联，如果不设置此字段，表明当前创建的span为root span
	References []SpanReference  

	// StartTime overrides the Span's start time, or implicitly becomes
	// time.Now() if StartTime.IsZero().
	// 指定span的开始时间
	StartTime time.Time

    // 类似于Span的SetTag()
	Tags map[string]interface{}
}

// 创建一个新的span
StartSpan(operationName string, opts ...StartSpanOption) Span

// 将sm按照format的格式序列化到carrier，通常是在client端调用
Inject(sm SpanContext, format interface{}, carrier interface{}) error
// 从carrier中按照format的格式取出sm，通常是在server端收到client调用后，取出spancontext
Extract(format interface{}, carrier interface{}) (SpanContext, error)

// opentracing有定义一些内置的BuiltinFormat：
Binary：二进制方式
TextMap：key:value 的string pairs，对应的carrier类型为map[string]string
HTTPHeaders：作为HTTP header的string pairs，对应的carrier类型为http.Header
```
