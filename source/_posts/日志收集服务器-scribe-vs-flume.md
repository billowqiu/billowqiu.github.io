---
title: 日志收集服务器-scribe vs Flume
id: 108
categories:
  - 系统编程
date: 2011-04-09 00:03:00
tags:
---

    

I read this [ post](http://www.cloudera.com/blog/2010/07/whats-new-in-cdh3b2-flume/)
 about Cloudera&rsquo;s Flume with much interest. Flume sounds 
like a very interesting tool, not to mention that from Cloudera&rsquo;s business 
perspective it makes a lot of sense:

> We&rsquo;ve seen our customers have great success using Hadoop for processing their 
> data, but the question of how to get the data there to process in the first 
> place was often significantly more challenging.

Just in case you didn&rsquo;t have the time to read about Flume yet, here&rsquo;s a short 
description from the [ GitHub project page](http://github.com/cloudera/flume)
:

> Flume is a distributed, reliable, and available service for efficiently 
> collecting, aggregating, and moving large amounts of log data. It has a simple 
> and flexible architecture based on streaming data flows. It is robust and fault 
> tolerant with tunable reliability mechanisms and many failover and recovery 
> mechanisms. The system is centrally managed and allows for intelligent dynamic 
> management. It uses a simple extensible data model that allows for online 
> analytic applications.

In a way this sounded a bit familiar. I thought I&rsquo;ve seen something kind of 
similar before: [ Scribe](http://wiki.github.com/facebook/scribe/)
:

> Scribe is a server for aggregating streaming log data. It is designed to 
> scale to a very large number of nodes and be robust to network and node 
> failures. There is a scribe server running on every node in the system, 
> configured to aggregate messages and send them to a central scribe server (or 
> servers) in larger groups. If the central scribe server isn&rsquo;t available the 
> local scribe server writes the messages to a file on local disk and sends them 
> when the central server recovers. The central scribe server(s) can write the 
> messages to the files that are their final destination, typically on an nfs 
> filer or a distributed filesystem, or send them to another layer of scribe 
> servers.

So my question is: **how does Flume and Scribe compare**
? What 
are the major differences and what scenarios are good for one or the other?

If you have the answer to any of these questions, please drop a comment or [send me an email](mailto:nosql%5B@%5Dmypopescu%5B.%5Dcom)
.

_Update_
: Looks like 
I&rsquo;ve failed to find this [ useful thread](https://groups.google.com/a/cloudera.org/group/flume-user/msg/22676a7e38028fbb)
, but thanks to [this 
comment](http://nosql.mypopescu.com/post/820711193/how-does-flume-and-scribe-compare#comment-62595276)
 mistake is corrected:

> 1\. Flume allows you to configure your Flume installation from a central 
> point, without having to ssh into every machine, update a configuration variable 
> and restart a daemon or two. You can start, stop, create, delete and reconfigure 
> logical nodes on any machine running Flume from any command line in your network 
> with the Flume jar available.
> 
> 2\. Flume also has centralised liveness monitoring. We&rsquo;ve heard a couple of 
> stories of Scribe processes silently failing, but lying undiscovered for days 
> until the rest of the Scribe installation starts creaking under the increased 
> load. Flume allows you to see the health of all your logical nodes in one place 
> (note that this is different from machine liveness monitoring; often the machine 
> stays up while the process might fail). 
> 
> 3\. Flume supports three distinct types of reliability guarantees, allowing 
> you to make tradeoffs between resource usage and reliability. In particular, 
> Flume supports fully ACKed reliability, with the guarantee that all events will 
> eventually make their way through the event flow. 
> 
> 4\. Flume&rsquo;s also really extensible - it&rsquo;s really easy to write your own source 
> or sink and integrate most any system with Flume. If rolling your own is 
> impractical, it&rsquo;s often very straightforward to have your applications output 
> events in a form that Flume can understand (Flume can run Unix processes, for 
> example, so if you can use shell script to get at your data, you&rsquo;re golden). 
> 
> &mdash; Henry Robinson

In the same thread, I&rsquo;m reading about another tool [ 
Chukwa](http://wiki.apache.org/hadoop/Chukwa)
:

> Chukwa is a Hadoop subproject devoted to large-scale log collection and 
> analysis. Chukwa is built on top of the Hadoop distributed filesystem (HDFS) and 
> MapReduce framework and inherits Hadoop&rsquo;s scalability and robustness. Chukwa 
> also includes a exible and powerful toolkit for displaying monitoring and 
> analyzing results, in order to make the best use of this collected data.
</div>