---
title: C++对象内存结构之虚指针
id: 101
categories:
  - C++
date: 2011-07-20 22:28:00
tags:
---

虚函数对于C++进行OO的作用毋庸置疑，下面通过一个简单的实例对虚指针进行深入讲解
<pre class="brush:cpp">#include &lt;cstring&gt;
class VirtualClass
{
public:
   virtual void foo()
   {
   }
};

class NoVirtualClass
{
public:
   void foo()
  {
  }
};

int main(int, char *[])
{
    NoVirtualClass*pObjA = new NoVirtualClass;
    VirtualClassobj;
    VirtualClass*pObjB = new VirtualClass;

    size_t nNoVirtual = sizeof(NoVirtualClass);
    size_t nVirtual = sizeof(VirtualClass);

    std::memset( pObjA, 0, nNoVirtual );
    std::memset( &amp;obj, 0, nVirtual );
    std::memset( pObjB, 0, nVirtual );

    pObjA-&gt;foo();
    obj.foo();
    pObjB-&gt;foo();

    return 0;
}</pre>
类NoVirtualClass的大小为1，《深度搜索C++对象模型》有解释，类VirtualClass大小为4，因为对于有virtual函数的C++类，每个对象都有个叫做vfptr的指针，在VS2008中调试上述代码可以看到如下信息：

-__vfptr0x00415740 const VirtualClass::`vftable'*
[0]0x00411104 VirtualClass::foo(void)*

上面的三个memset分别进行如下操作：

30行那个将pObjA所指的内存区域置0，也就是将其所指的那一个字节清零。

31和32行都是将__vfptr置空。

下面三个函数调用，就很有意思了，尤其是第三个调用，直接出现内存违例。

大家可以把上述代码放到VS中调试跟踪一把，肯定会让自己对C++内存对象模型以及虚函数指针有一个更深的理解。

&nbsp;