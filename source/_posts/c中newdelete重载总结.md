---
title: c++中new/delete重载总结
id: 576
categories:
  - C++
date: 2015-05-31 22:18:55
tags:
---

这个问题看了很多次，忘了很多次，最近在重温内存池时又看了下，做个笔记吧。

首先是两个名词：new表达式，new操作符，

默认情况下当你写下如下语句时:

Foo* foo = new Foo;(这里是new-expression)，实际执行了三个步骤：

1：new表达式调用一个operator new的标准库函数，分配一块足够大、原始的、未命名的内存空间

2：编译器运行相应的构造函数，并传入初始值

3：对象分配空间并构造完成，返回一个指向该对象的指针

delete foo，实际执行了两个相反的步骤：

1：调用该对象的析构函数

2：调用operator delete标准库函数，回收空间

c++中new/delete有如下级别：

*   global new/delete
*   class specific new/delete
通过代码来说明问题比较直观

*   重载全局操作符
<pre class="brush:cpp">#include &lt;iostream&gt;
#include &lt;stdlib.h&gt;

class Foo
{
public:
	Foo()
	{
		std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
	}
	~Foo()
	{
		std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
	}
};

//重载全局new操作符，和一般的操作符重载类似
void* operator new(size_t sz) 
{
	std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
	void* m = malloc(sz);
	if(!m) 
	{
		std::cerr &lt;&lt; "out of memory" &lt;&lt; std::endl;
	}

	return m;
}

//重载全局delete操作符，和一般的操作符重载类似
void operator delete(void* m) 
{
	std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
	free(m);
}

int main(int argc, const char* argv[])
{
	//new-expression(表达式)
	//首先调用全局的operator(操作符) new分配内存,再调用构造函数
	Foo* foo = new Foo;
	//delete-expression
	//首先调用析构函数,再调用全局的operator(操作符) delete释放内存
	delete foo;

	return 0;
}</pre>

*   类操作符
<pre class="brush:cpp">#include &lt;cstddef&gt;
#include &lt;iostream&gt;
#include &lt;new&gt;

class Framis 
{
	enum { sz = 10 };
	char c[sz]; // To take up space, not used
	static unsigned char pool[];
	static bool alloc_map[];
public:
	enum { psize = 100 };  // frami allowed
	Framis() { std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl; }
	~Framis() { std::cout &lt;&lt; __FUNCTION__ &lt;&lt; std::endl; }
	void* operator new(size_t) throw(std::bad_alloc);
	void operator delete(void*);
};

unsigned char Framis::pool[psize * sizeof(Framis)];
bool Framis::alloc_map[psize] = {false};

// Size is ignored -- assume a Framis object
void* Framis::operator new(size_t) throw(std::bad_alloc) 
{
	for(int i = 0; i &lt; psize; i++)
	{
		if(!alloc_map[i]) 
		{
			std::cout &lt;&lt; "using block " &lt;&lt; i &lt;&lt; " ... ";
			alloc_map[i] = true; // Mark it used
			return pool + (i * sizeof(Framis));
		}
	}
	std::cout &lt;&lt; "out of memory" &lt;&lt; std::endl;
	throw std::bad_alloc();
}

void Framis::operator delete(void* m) 
{
	if(!m) return; // Check for null pointer
	// Assume it was created in the pool
	// Calculate which block number it is:
	unsigned long block = (unsigned long)m - (unsigned long)pool;
	block /= sizeof(Framis);
	std::cout &lt;&lt; "freeing block " &lt;&lt; block &lt;&lt; std::endl;
	// Mark it free:
	alloc_map[block] = false;
}

int main() 
{
	Framis* f[Framis::psize];
	try 
	{
		for(int i = 0; i &lt; Framis::psize; i++)
		{
			f[i] = new Framis;
		}
		new Framis; // std::cout of memory
	} 
	catch(std::bad_alloc) 
	{
		std::cerr &lt;&lt; "out of memory!" &lt;&lt; std::endl;
	}
	delete f[10];
	f[10] = 0;
	// Use released memory:
	Framis* x = new Framis;
	delete x;
	for(int j = 0; j &lt; Framis::psize; j++)
	{
		delete f[j]; // Delete f[10] OK
	}

	return 0;
}</pre>
上面的例子是从&lt;&lt;think in c++&gt;&gt;中copy过来的，类的new/delete操作符函数，尽管你可以不写static关键字，但是编译器其实把其当做static函数的。

另外还有个叫做placement new/delete表达式的重载，可以在指定的地址空间上构造/析构对象。