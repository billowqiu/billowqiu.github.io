---
title: boost中各种各样的timer
id: 122
categories:
  - C++
date: 2010-03-31 23:17:00
tags:
---

    

最近由于工作需要，大量使用boost，某天闲来无事整了个hello world版的timer实现。<pre style="border: 1px dotted #785;background: #f5f5f5;">#include &lt;boost/thread.hpp&gt;
#include &lt;boost/date_time.hpp&gt;
#include &lt;iostream&gt;
#include &lt;boost/function.hpp&gt;
void threadTimer( boost::posix_time::time_duration duration ,boost::function&lt;void (void)&gt; funCallback)
{
    //使用localtime
    boost::posix_time::ptime ptimeCur( boost::posix_time::second_clock::local_time() );
    while ( true )
    {
        boost::posix_time::ptime ptimeNow( boost::posix_time::second_clock::local_time() );
        //超过了预定的时间,由于下面有sleep因此这里算时间长度时减去相应的长度
        //纯属学习,没啥实际用途,O(&cap;_&cap;)O~
        if ( boost::posix_time::time_period( ptimeCur, ptimeNow ).length() &gt; (duration -boost::posix_time::seconds(1)) )
        {
            funCallback();
            ptimeCur = ptimeNow;
        }
        //为了不让cpu被占用,这里sleep下
        boost::xtime xt;
        boost::xtime_get(&amp;xt, boost::TIME_UTC);
        xt.sec += 1;
        boost::thread::sleep( xt );
    }
}
void timeFun()
{
    std::cout &lt;&lt; "time out..." &lt;&lt; std::endl;
}
int _tmain(int argc, _TCHAR* argv[])
{
    boost::thread thread(threadTimer,boost::posix_time::time_duration( boost::posix_time::seconds(3) ), timeFun );
    thread.join();

return 0;
}</pre>&nbsp;

虽然代码不长，但是涉及到boost好几个库:thread,data_time,function，呵呵，纯属学习了。在查找thread::sleep使用过程中，

顺带发现thread里面自带的一个例子(稍加修改)即可作为timer使用：

<pre style="border: 1px dotted #785;background: #f5f5f5;">#include &lt;boost/thread/thread.hpp&gt;
#include &lt;boost/thread/xtime.hpp&gt;
#include &lt;iostream&gt;
struct thread_alarm
{
    thread_alarm(int secs) : m_secs(secs) { }
    void operator()()
    {
        while (true)
        {
            boost::xtime xt;
            boost::xtime_get(&amp;xt, boost::TIME_UTC);
            xt.sec += m_secs;
            boost::thread::sleep(xt);
            std::cout &lt;&lt; "alarm sounded..." &lt;&lt; std::endl;
        }
    }
    int m_secs;
};
int main(int argc, char* argv[])
{
    int secs = 5;
    std::cout &lt;&lt; "setting alarm for 5 seconds..." &lt;&lt; std::endl;
    thread_alarm alarm(secs);
    boost::thread thrd(alarm);
    thrd.join();
    return 0;
}</pre>&nbsp;

这里巧妙利用sleep实现timer，值得学习。

ps:今天研究asio过程中，发现deadline_timer比较好用，另外加上还有个单独的timer库，看来timer真是无处不在呀。

</div>