---
title: C++设计模式之一 工厂模式（简单工厂、工厂和抽象工厂）
id: 143
categories:
  - 设计模式
date: 2009-07-21 23:34:00
tags:
---

    

原文地址：[http://www.cnblogs.com/Seasky/archive/2009/02/06/1385609.html](http://www.cnblogs.com/Seasky/archive/2009/02/06/1385609.html)

[实例代码下载](http://files.cnblogs.com/Seasky/FactoryPattern.7z "工厂模式实例代码")&nbsp;
[结构类图下载](http://files.cnblogs.com/Seasky/FactoryPatternChart.zip "结构图下载")&nbsp;

&nbsp; &nbsp; &nbsp;&nbsp; 今天开始这个系列之前，心里有些恐慌，毕竟园子里的高手关于设计模式的经典文章很多很多，特别是大侠[李会军](http://terrylee.cnblogs.com/)、[吕震宇](http://zhenyulu.cnblogs.com/) 老师的文章更是堪称经典。他们的文笔如行云流水，例子活泼生动，讲解深入浅出。好在他们都是用C#描述，也没有提供必要的源码下载，所以我这里用C++实现。首先我想声明的是我的文笔绝对不如他们的好，例子也没有他们的形象，不过我打算把C++的代码实现和类图提供给大家，就算作为一种补充吧。

&nbsp; &nbsp; &nbsp;&nbsp; 开始设计模式自然而然到提到几个原则：I、开闭法则（OCP）；II、里氏代换法则（LSP）；III、依赖倒置法则(DIP)；IV、接口隔离法则（ISP）；V、合成/聚合复用原则（CARP）；VI、迪米特法则（LoD），这几个法则在[吕震宇](http://zhenyulu.cnblogs.com/) 老师的[设计模式（二）](http://www.cnblogs.com/zhenyulu/articles/36061.html)和[设计模式（三）](http://www.cnblogs.com/zhenyulu/articles/36068.html)中有非常详尽的阐述和深入浅出的举例分析。有兴趣的朋友打开链接看一下就可以了。

&nbsp; &nbsp; &nbsp; 补充说明：

*   我这里所以代码都是用VS2005的C++编译器实现。所以不能保证在其他IDE中能顺利编译，但是我想如果你使用其他编译器，也应该不会有太大问题，主要也应该是stdafx.h文件中包含的头文件问题。*   里面出行的结构图都是用微软的Visio2003 绘制，大家下载后可以直接用Visio打开。*   在以后所有的模式例子中都有客户程序，客户程序这个角色不是模式本身的内容，它是模式之外的部分，但是正是这个客户程序完成了对模式的使用，模式本身的结构是讲解的重点，但是客户程序如何使用模式也是理解模式的一个重要方面，因此在我后续的介绍中都有客户程序这个角色，并会说明究竟调用模式中的哪些角色完成对模式的使用。

#### 简单工厂模式

##### 生活例子

&nbsp; &nbsp; &nbsp; 吃饭是人的基本需求，如果人类不需要吃饭，可能我们就能活得清闲许多，也就不需要像现在一样没日没夜的工作，学习。我们学习是为了找到更好的工作，好工作为了赚更多的钱，最终为了吃饱饭，吃好饭。因此可以说吃饭是与人息息相关，下面就从吃饭的例子来引入工厂模式的学习。

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; 如果你想吃饭了，怎么办自己做吗？自己做就相当于程序中直接使用new。当然是自己下个指令，别人来做更爽。那就把做饭的任务交给你的老婆吧，那么她就是一个做饭的工厂了，你告诉她要要吃红烧肉，等会她就从厨房给你端出来一盘香喷喷的红烧肉了，再来个清蒸鱼吧，大鱼大肉不能太多，那就再来个爆炒空心菜，最后再来个西红柿鸡蛋汤。下图 1) 就是这个问题的模型。
![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-2.JPG)&nbsp;
（图1）
&nbsp; &nbsp; &nbsp; &nbsp;显然到了这里，你是Client，你老婆就是工厂，她拥有做红烧肉的方法，做清蒸鱼的方法，做爆炒空心菜、西红柿鸡蛋汤的方法，这些方法返回值就是食物抽象。红烧肉、清蒸鱼、爆炒空心菜、西红柿鸡蛋汤就是食物的继承类，到这里你就可以大吃二喝了。简单工厂模式也成型了。哈哈，娶一个手艺不错的老婆还真好，吃的好，吃的爽，又清闲。

&nbsp; &nbsp; &nbsp;&nbsp; 下面来看标准的简单工厂模式的分析。&nbsp;

##### 意图

把一系列拥有共同特征的产品的创建封装 

##### 结构图

![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-3.JPG)&nbsp;
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; （图2）

##### 角色分析

<div style="padding-right: 20px; padding-left: 30px; padding-bottom: 10px; padding-top: 0px;">**产品基类**： 工厂创建的所有产品的基类, 它负责描述所有实例所共有的公共接口。它用来作为工厂方法的返回参数。
代码实现：
<pre class="programlisting">        //---这时一个系列的产品基类
class Product
{
protected:
Product(void);
public:
virtual ~Product(void);
public:
virtual void Function() = 0;
};
//cpp
Product::Product(void)
{
}
Product::~Product(void)
{
}
</pre>
**具体产品类**：产品1和产品2，这个角色实现了抽象产品角色所定义的接口。
代码实现：
<pre class="programlisting">        //产品A
class ConcreteProductA:public Product
{
public:
ConcreteProductA(void);
public:
virtual ~ConcreteProductA(void);
public:
virtual void Function();
};
//cpp
ConcreteProductA::ConcreteProductA()
{
cout&lt;&lt;"创建 A 产品"&lt;&lt;endl;
}
ConcreteProductA::~ConcreteProductA()
{
cout&lt;&lt;"释放 A 产品"&lt;&lt;endl;
}
void ConcreteProductA::Function()
{
cout&lt;&lt;"这是产品 A 具有的基本功能"&lt;&lt;endl;
}
//产品B与A类似不这里不再给出，大家可以下载源码
</pre>

**工厂类**：负责具体产品的创建，有两种方式实现产品的创建，I、创建不同的产品用不同的方法；II、创建不同产品用相同的方法，然后通过传递参数实现不同产品的创建。本实例中两种模式都给出了，大家自行分析。 

<pre class="programlisting">//简单工厂，此类不需要继承，直接硬编码实现生成的产品
class SimpleFactory
{
public:
SimpleFactory(){}
public:
~SimpleFactory(){}
public:
Product *CreateProduct(int ProuctType);
Product *CreateProductA();
Product *CreateProductB();
};
//CPP
Product * SimpleFactory::CreateProduct(int ProductType=0)
{
Product *p = 0;
switch(ProductType)
{
case 0:
p= new ConcreteProductA();
break;
case 1:
p= new ConcreteProductB();
break;
default:
p= new ConcreteProductA();
break;
}
return p;
}
Product *SimpleFactory::CreateProductA()
{
return new ConcreteProductA();
}
Product *SimpleFactory::CreateProductB()
{
return new ConcreteProductB();
}
</pre>
**客户端程序**：访问的角色包括产品基类、工厂类。不直接访问具体产品类。通过基类指针的多态实现产品功能的调用。
**访问描述**：客户程序通过调用工厂的方法返回抽象产品，然后执行产品的方法。
<pre class="programlisting">//调用代码
SimpleFactory sf;
Product *p = sf.CreateProductA();
p-&gt;Function();
delete p;
p = sf.CreateProductB();
p-&gt;Function();
delete p;
</pre>
</div>

##### 优缺点说明

**优点**：1) 首先解决了代码中大量New的问题。为何要解决这个问题，好处的说明我想放到结尾总结中。
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;2) 用工厂方法在一个类的内部创建对象通常比直接创建对象更灵活。 
**缺点**：对修改不封闭，新增加产品您要修改工厂。违法了鼎鼎大名的开闭法则（OCP）。

##### 附加说明

*   大家可以参看 吕震宇 老师的[C#设计模式（四）](http://www.cnblogs.com/zhenyulu/articles/36462.html)参看这个模式的分析，里面还给出了这个模式的两个变体，实现比较简单，有兴趣的朋友可以自行用C++实现一下。*   产品基类的代码中构造函数我用了Protected，而没有使用Public，主要是为了体现编码中的一个最小权限原则。说明此类不许用户直接实例化。虽然这里使用了virtual void Function() = 0;编译器也会控制不让用户直接实例化，不过我依然认为使用私有化构造函数来保护类不直接实例化是一个良好的编程风格。

#### 工厂方法模式

##### 生活例子:

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; 人是最贪得无厌的动物，老婆手艺再好，总有不会做的菜，你想吃回锅肉，怎么办，让老婆学呗，于是就给她就新增了做回锅肉的方法，以后你再想吃一个新菜，就要给你老婆新加一个方法，显然用老婆做菜的缺点也就暴露出来了，用程序设计的描述就是对修改永远不能封闭。当然优点也是有的，你有了老婆这个工厂，这些菜不用你自己做了，只要直接调用老婆这个工厂的方法就可以了。 

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;面对上面对修改不能封闭的问题，有没有好的解决方案吗，如果你有钱，问题就迎刃而解了，把老婆抽象变成一个基类，你多娶几个具体的老婆，分别有做鱼的，做青菜的，炖汤的老婆，如果你想吃一个新菜，就再新找个女人，从你的老婆基类继承一下，让她来做这个新菜。显然多多的老婆这是所有男人的梦想，没有办法，法律不允许，那么咱们只是为了做饭，老婆这个抽象类咱们不叫老婆了，叫做厨师吧，她的子类也自然而然的该叫做鱼的厨师、炖汤的厨师了。现在来看这个模式发生了变化，结构中多了一个厨师的抽象，抽象并不具体的加工产品了，至于是炖汤还是炖鱼，是由这个抽象工厂的继承子类来实现，现在的模式也就变成工厂方法模式了，这个上面的结构图1)就变成了下面的图3的结构了。
![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-4.JPG) 
&nbsp; &nbsp; &nbsp;&nbsp; （图3）

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;现在再来分析现在的模式，显然简单工厂的缺陷解决了，新增加一个菜只需要新增加一个厨师就行了，原来的厨师还在做原来的工作，这样你的设计就对修改封闭了。你看把老婆解放出来，招聘大量的厨师到你家里这个方案多么的完美，你老婆也会爱死你了。当然前提就是你要有多多的钱噢，当然这里的钱的多少在软件领域应该看你的客户软件投资方的要求。 
下面来一下标准的工厂模式的实现 

##### 意图

*   定义一个用户创建对象的接口，让子类决定实例化哪一个类。Factory Method使一个类的实例化延迟到其子类。*   上面是GOF关于此模式的意图描述，我想补充的是您可以这样理解：为了改善简单工厂对修改不能关闭的问题。

##### 结构

&nbsp; ![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-5.JPG)
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;图4

##### 角色分析

<div style="padding-right: 20px; padding-left: 30px; padding-bottom: 10px; padding-top: 0px;">**产品基类**：同简单工厂的产品基类，其实就是用和简单工厂中的是同一个类，这里并没有重写。
**具体产品类**：也是用的简单工厂的具体产品类，为了体现对修改的关闭这里为系统新添加了一个具体产品类，就是&ldquo;新产品&rdquo;，代码中叫做&ldquo;ConcreteProductANew&rdquo;
**工厂基类**：定义了工厂创建产品的接口，但是没有实现，具体创建工作由其继承类实现。
代码实例 

<pre class="programlisting">//工厂模式，此模式的工厂只定义加工产品的接口，具体生成交予其继承类实现
//只有具体的继承类才确定要加工何种产品
class Factory
{
public:
Factory(void);
public:
virtual ~Factory(void);
public:
virtual Product* CreateProduct(int ProductType = 0) =0;
};
//CPP
Factory::Factory(void)
{
}
Factory::~Factory(void)
{
}
</pre>
**具体工厂类**：工厂基类的具体实现，由此类决定创建具体产品，这里 **ConcreteFactory1** 对于与图中的 **工厂实现**，**ConcreteFactory2** 对于与图中的**新工厂**。
下面给出实现代码

<pre class="programlisting">//工厂实现
class ConcreteFactory1:public Factory
{
public:
ConcreteFactory1();
public:
virtual ~ConcreteFactory1();
public :
Product* CreateProduct(int ProductType);
};
//新工厂，当要创建新类是实现此新工厂
class ConcreteFactory2:public Factory
{
public:
ConcreteFactory2();
public:
virtual ~ConcreteFactory2();
public :
Product* CreateProduct(int ProductType);
};
//CPP
ConcreteFactory1::ConcreteFactory1()
{
}
ConcreteFactory1::~ConcreteFactory1()
{
}
Product * ConcreteFactory1::CreateProduct(int ProductType = 0)
{
Product *p = 0;
switch(ProductType)
{
case 0:
p= new ConcreteProductA();
break;
case 1:
p= new ConcreteProductB();
break;
default:
p= new ConcreteProductA();
break;
}
return p;
}
ConcreteFactory2::ConcreteFactory2()
{
}
ConcreteFactory2::~ConcreteFactory2()
{
}
Product * ConcreteFactory2::CreateProduct(int ProductType = 0)
{
return new ConcreteProductANew();
}
</pre>
**客户端调用**：访问角色（产品基类、工厂基类、工厂实现类）
**调用描述**：客户程序通过工厂基类的方法调用工厂实现类用来创建所需要的具体产品。从而实现产品功能的访问。
代码实现

<pre class="programlisting">Factory*fct = new ConcreteFactory1();
Product *p = fct-&gt;CreateProduct(0);
p-&gt;Function();
delete p;
p = fct-&gt;CreateProduct(1);
p-&gt;Function();
delete p;
delete fct;
fct = new ConcreteFactory2();
p=fct-&gt;CreateProduct();
delete p;
delete fct;
</pre>
</div>

##### 优缺点分析

**优点 **

*   简单工厂具有的优点*   解决了简单工厂的修改不能关闭的问题。系统新增产品，新增一个产品工厂即可，对抽象工厂不受影响。

**缺点**：对于创建不同系列的产品无能为力 

##### 适用性

*   当一个类不知道它所必须创建的对象的类的时候。*   当一个类希望由它的子类来指定它所创建的对象的时候。*   当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。

##### 其他参考

*   吕震宇的[C#设计模式（5）－Factory Method Pattern ](http://www.cnblogs.com/zhenyulu/articles/36590.html)
*   TerryLee 的[.NET设计模式（5）：工厂方法模式（Factory Method） ](http://terrylee.cnblogs.com/archive/2006/01/04/310716.html)

&nbsp;

#### 抽象工厂模式

##### 生活例子

&nbsp; &nbsp;&nbsp;&nbsp; 世事多变，随着时间的推移，走过的地方越来越多，你天南海北的朋友也越来越多。你发现菜原来还分了许多菜系，鲁菜、粤菜、湘菜等等，它们各有各的风味，同样是红烧肉由不同菜系出来的味道也各不相同， 你招待不同的朋友要用不同的菜系，这下难办了，你的厨师都是鲁菜风味，怎么办，广东的朋友来了吃不惯。现在我们再回到简单工厂模式（就是老婆做菜的模式），我们把红烧肉再向下继承，生成鲁菜红烧肉、粤菜红烧肉、湘菜红烧肉；清蒸鱼向下继承为鲁菜清蒸鱼、粤菜清蒸鱼、湘菜清蒸鱼，其它也以此类推。我们也修改一下老婆的这个类，不让其返回食物基类，而是返回红烧肉、清蒸鱼、爆炒空心菜、西红柿鸡蛋汤这一层次，并把这些方法抽象化，作为菜系工厂基类，然后再从此基类继承出，鲁菜工厂、粤菜工厂、湘菜工厂等等，再由这些具体工厂实现创建具体菜的工作，哈哈你如果招待广东朋友就用粤菜工厂，返回的就是一桌粤菜菜系的红烧肉、清蒸鱼、空心菜和西红柿鸡蛋汤了，你的广东朋友一定会吃的非常合乎胃口了。噢，非常好，你已经实现了抽象工厂模式了。结构模型图也变成了下图 6)的样子了。
![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-6.JPG)
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; （图6）
&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; 现在可以看到，想新来做一个菜系，只需新聘请一个厨师就可以了，多么完美，但是你先别高兴太早，如果你想新增加一个菜就变得非常困难了。

##### 意图

&nbsp; &nbsp; &nbsp; &nbsp;提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。 
**结构**
![](http://images.cnblogs.com/cnblogs_com/Seasky/167561/r_1-7.JPG)

##### 角色分析

<div style="padding-right: 20px; padding-left: 30px; padding-bottom: 10px; padding-top: 0px;">**产品基类**：这里包含产品基类A和产品基类B，实际上在我的示例代码中，这两个产品都从共同的基类继承而来，但是这个继承关系却是在这个模式之外的部分，而本身这个模式关心的是这两个产品基类的差异部分。 
代码实现：这里的代码就是借用的简单工厂模式中具体产品类的代码实现部分，为了大家阅读方便，下面重新给出一下。

<pre class="programlisting">        //产品A
class ConcreteProductA:public Product
{
public:
ConcreteProductA(void);
public:
virtual ~ConcreteProductA(void);
public:
virtual void Function();
};
//cpp
ConcreteProductA::ConcreteProductA()
{
cout&lt;&lt;"创建 A 产品"&lt;&lt;endl;
}
ConcreteProductA::~ConcreteProductA()
{
cout&lt;&lt;"释放 A 产品"&lt;&lt;endl;
}
void ConcreteProductA::Function()
{
cout&lt;&lt;"这是产品 A 具有的基本功能"&lt;&lt;endl;
}
//产品B与A类似不这里不再给出，大家可以下载源码
</pre>

**
具体产品**类：这里的具体产品类是产品A1，A2，B1、B2等，
代码实现：A1对应的实现就是&ldquo;&rdquo;

<pre class="programlisting">class ConcreteProductA1:public ConcreteProductA
{
public:
ConcreteProductA1(void);
public:
virtual ~ConcreteProductA1(void);
public:
virtual void Function();
};
//CPP
ConcreteProductA1::ConcreteProductA1()
{
cout&lt;&lt;"创建 A1 产品"&lt;&lt;endl;
}
ConcreteProductA1::~ConcreteProductA1()
{
cout&lt;&lt;"释放 A1 产品"&lt;&lt;endl;
}
void ConcreteProductA1::Function()
{
cout&lt;&lt;"这时产品 A1 具有的基本功能"&lt;&lt;endl;
}
</pre>

**工厂抽象**接口：定义了创建产品的接口，这里返回参数是返回的产品A，产品B，而本身产品A和B的共同基类，小弟认为正是这个特征构成了抽象工厂和工厂模式的区别。
代码实现

<pre class="programlisting">//抽象工厂模式
class AbstractFactory
{
public:
AbstractFactory();
public:
virtual ~AbstractFactory();
public:
virtual ConcreteProductA* CreateA() = 0;
virtual ConcreteProductB* CreateB() = 0;
};
//CPP
AbstractFactory::AbstractFactory()
{
}
AbstractFactory::~AbstractFactory()
{
}
</pre>

具体工厂实现类：工厂1和工厂2。新增加系列，只需新实现一个工厂。
代码实现: 工厂1的就是ConcreteAbsFactory1，工厂2的代码类似，这里没有给出，可以在下载代码中看到

<pre class="programlisting">////工厂1-----
class ConcreteAbsFactory1:public AbstractFactory
{
public:
ConcreteAbsFactory1();
public:
virtual ~ConcreteAbsFactory1();
public:
virtual ConcreteProductA* CreateA();
virtual ConcreteProductB* CreateB();
};
//CPP
ConcreteAbsFactory1::ConcreteAbsFactory1()
{
}
ConcreteAbsFactory1::~ConcreteAbsFactory1()
{
}
ConcreteProductA* ConcreteAbsFactory1::CreateA()
{
return new ConcreteProductA1();
}
ConcreteProductB * ConcreteAbsFactory1::CreateB()
{
return new ConcreteProductB1();
}
</pre>

客户端访问： 访问角色（产品基类、抽象工厂、具体工厂实现类）
访问描述： 通过抽象工厂的指针访问具体工厂实现来创建对应系列的产品，然后通过产品基类指针访问产品功能。
调用代码：

<pre class="programlisting">          AbstractFactory *absfct = new ConcreteAbsFactory1();
ConcreteProductA *cpa = absfct-&gt;CreateA();
cpa-&gt;Function();
delete cpa;
ConcreteProductB *cpb = absfct-&gt;CreateB();
cpb-&gt;Function();
delete cpb;
delete absfct;
absfct = new ConcreteAbsFactory2();
cpa = absfct-&gt;CreateA();
cpa-&gt;Function();
delete cpa;
cpb = absfct-&gt;CreateB();
cpb-&gt;Function();
delete cpb;
</pre>
</div>

&nbsp;

##### 和工厂模式的分析比较

&nbsp; &nbsp; &nbsp;&nbsp; 现在可以和工厂模式对比一下，抽象工厂返回的接口不再是产品A和产品B的共同基类Product了，而是产品A、产品B基类（在工厂模式中它们为具体实现类，这里变成了基类）了。此时工厂的抽象和简单工厂中的工厂方法也很类似，就是这些特征区使其别于工厂模式而变成抽象工厂模式了，因此抽象工厂解决的是创建一系列有共同风格的产品（鲁菜还是粤菜），而工厂方法模式解决的创建有共同特征的一系列产品（红烧肉、清蒸鱼它们都是食物）。当然简单工厂的缺陷在抽象工厂中又再次出现了，我要新增加一个产品，工厂抽象接口就要改变了。因此抽象工厂并不比工厂模式完美，只不过是各自的适用领域不同而已。其实，这里如果把抽象工厂模式的接口返回产品A和产品B的共同基类（工厂模式返回的参数），你会发现，奇怪这个模式怎么这么眼熟，它不是恰恰退化成工厂模式了。
&nbsp; &nbsp; &nbsp; &nbsp;类模式与对象模式的区别讨论：先看定义类&ldquo;模式使用继承关系，把对象的创建延迟的子类，对象模式把对象的创建延迟到另一个对象中&rdquo;。 分析：首先它们创建对象都不是在基类中完成，都是在子类中实现，因此都符合类模式的概念；但是工厂模式的创建产品对象是在编译期决定的，要调用某个工厂固定的，而抽象工厂模式对产品的创建是在运行时动态决定的，只有到运行时才确定要调用那个工厂，调用工厂随运行环境而改变。（这里我一直很混乱，欢迎大家讨论）

##### 适用性

*   一个系统要独立于它的产品的创建、组合和表示时*   一个系统要由多个 产品系列中的一个来配置时*   当你要强调一个系列相关的产品对象的设计以便进行联合使用时*   当你提供一个产品类库，而只想显示它们的接口而不是实现时。

##### 参考

*   吕震宇的[C#设计模式（6）](http://www.cnblogs.com/zhenyulu/articles/36885.html)*   TerryLee 的[.NET设计模式（3）：抽象工厂模式（Abstract Factory）](http://terrylee.cnblogs.com/archive/2005/12/13/295965.html)

#### 总结

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;工厂本质就是用工厂方法替代直接New来创建对象。这里不是指的让用户重载一个新操作符号来进行创建对象的操作，而是说把New 操作封装在一个方法中，等用户需要创建对象时调用此方法而避免直接使用New而已。这样做的目的就是之一就是封装，避免代码中大量New的运算符，这当然不是主要目的，因为这样虽然New少了，CreateObject方法却多了，但是如果产品类的构造函数变了，我想常用工厂模式的修改源代码的工作应该简便许多吧，当然这算不上这个模式的好处，它的真正强大的功能其实在于适应变化，这也是整个设计模式最根本的目的；还有一点就是体现了抽象于实现的分离，当然创建型模式都具有这个特点，工厂模式非常明显吧了，把具体创建工作放置到工厂中，使客户端程序更专注与业务逻辑的，这样的代码结构也更进行合理。

</div>