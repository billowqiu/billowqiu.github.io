---
title: 代理类(Surrogate)
id: 147
categories:
  - C++
date: 2009-07-05 23:44:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 依然是学习&lt;Ruminations On c++&gt;的一点笔记。

问题的提出是：怎样设计一个C++容器，使它有能力包含类型不同而彼此相关的对象呢？

容器通常只能包含一种类型的对象，所以很难在容器中存储对象本身，最容易想到的就是存储指针(我目前在项目中使用的也是这种方法)，这样虽然可以通过继承来处理类型不同的问题，但是也增加了内存分配的额外负担。

书中介绍了一种叫做代理的对象来解决这个问题，代理类是句柄类中最简单的一种，下面是一个简单的代理类实现：

<pre style="border: 1px dotted #785;background: #f5f5f5;">// Surrogate.cpp : 定义控制台应用程序的入口点。
//
#include "stdafx.h"

#define _CRTDBG_MAP_ALLOC
#include &lt;stdlib.h&gt;
#include &lt;crtdbg.h&gt;

#include &lt;iostream&gt;
#include &lt;vector&gt;
using namespace std;

class Vehicle
{
public:
virtual double weight()const =0;
virtual void   start()=0;
virtual Vehicle* copy()const=0;//用来复制编译时类型未知的对象
virtual ~Vehicle()//以便调用正确的派生类析构函数
{

}
//...
};

class RoadVehicle : public Vehicle
{
public:
double weight()const
{
cout&lt;&lt;"RoadVehicle::weight()"&lt;&lt;endl;
return 0;
}
void   start()
{
cout&lt;&lt;"RoadVehicle::start()"&lt;&lt;endl;
}
Vehicle* copy()const
{
return new RoadVehicle(*this);
}
/**/
};

class AutoVehicle : public RoadVehicle
{
public:
double weight()const
{
cout&lt;&lt;"AutoVehicle::weight()"&lt;&lt;endl;
return 0;
}
void   start()
{
cout&lt;&lt;"AutoVehicle::start()"&lt;&lt;endl;
}
Vehicle* copy()const
{
return new AutoVehicle(*this);
}
/**/
};

class Aircraft : public Vehicle
{
public:
double weight()const
{
cout&lt;&lt;"Aircraft::weight()"&lt;&lt;endl;
return 0;
}
void   start()
{
cout&lt;&lt;"Aircraft::start()"&lt;&lt;endl;
}
Vehicle* copy()const
{
return new Aircraft(*this);
}
/**/
};

class Helicopter : public Aircraft
{
public:
double weight()const
{
cout&lt;&lt;"Helicopter::weight()"&lt;&lt;endl;
return 0;
}
void   start()
{
cout&lt;&lt;"Helicopter::start()"&lt;&lt;endl;
}
Vehicle* copy()const
{
return new Helicopter(*this);
}
/**/
};

//代理类
class VehicleSurrogate
{
public:
VehicleSurrogate();
VehicleSurrogate(const Vehicle&amp;);
~VehicleSurrogate();
VehicleSurrogate(const VehicleSurrogate&amp;);
VehicleSurrogate&amp; operator=(const VehicleSurrogate&amp;);

//来自类Vehicle的操作
double weight()const;
void start();
private:
Vehicle *vp;
};

VehicleSurrogate::VehicleSurrogate():vp(0)
{

}

VehicleSurrogate::VehicleSurrogate(const Vehicle&amp; v):vp(v.copy())
{

}

VehicleSurrogate::~VehicleSurrogate()
{
delete vp;
}

VehicleSurrogate::VehicleSurrogate(const VehicleSurrogate&amp; v):vp(v.vp?v.vp-&gt;copy():0)
{

}

VehicleSurrogate&amp; VehicleSurrogate::operator=(const VehicleSurrogate&amp; v)
{
if (this!=&amp;v)
{
delete vp;
vp=(v.vp?v.vp-&gt;copy() : 0);
}
return *this;
}

//直接转发给vp调用
double VehicleSurrogate::weight()const
{
if (vp==0)
{
throw "empty VehicleSurrogate.weight()";
}
return vp-&gt;weight();
}

void VehicleSurrogate::start()
{
if (vp==0)
{
throw "empty VehicleSurrogate.start()";
}
vp-&gt;start();
}

int _tmain(int argc, _TCHAR* argv[])
{
vector&lt;VehicleSurrogate&gt; vecVS;

RoadVehicle rv;
Aircraft ar;
AutoVehicle av;
Helicopter hc;
vecVS.push_back(rv);
vecVS.push_back(ar);
vecVS.push_back(av);
vecVS.push_back(hc);

vector&lt;VehicleSurrogate&gt;::iterator iter=vecVS.begin();
vector&lt;VehicleSurrogate&gt;::iterator iterEnd=vecVS.end();
while(iter!=iterEnd)
{
iter-&gt;weight();
iter-&gt;start();
iter++;
}

_CrtDumpMemoryLeaks();

return 0;
}</pre> 

下面是测试结果，使用VS2005发现有内存泄漏，暂时没找到原因。

![result](http://p.blog.csdn.net/images/p_blog_csdn_net/ToCpp/EntryImages/20090706/Surrogate.png)

</div>