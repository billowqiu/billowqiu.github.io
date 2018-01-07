---
title: Bjarne Stroustrup 讲述语言的演变（转载）
id: 196
categories:
  - C++
date: 2008-10-02 18:28:00
tags:
---

    <DIV class=FeatureSmallHead>采访++</DIV>
<DIV class=FeatureHeadline>Bjarne Stroustrup 讲述语言的演变</DIV>
<DIV class=FeatureByLine>Howard Dierking</DIV>
<DIV>

<DIV class=ContentSeparator>
<DIV class=MTPS_CollapsibleRegion>
<DIV class=CollapseRegionLink>![](http://i.msdn.microsoft.com/Platform/Controls/CollapsibleArea/resources/plus.gif) 目录</DIV>
<DIV class=MTPS_CollapsibleSection style="DISPLAY: none">[<FONT color=#0033cc><U>对语言的看法</U></FONT>](http://msdn.microsoft.com/zh-cn/magazine/cc500572.aspx#S1) 
[<FONT color=#0033cc><U>语言发展趋势</U></FONT>](http://msdn.microsoft.com/zh-cn/magazine/cc500572.aspx#S2) 
[<FONT color=#0033cc><U>方法和最佳实践</U></FONT>](http://msdn.microsoft.com/zh-cn/magazine/cc500572.aspx#S3) 
[<FONT color=#0033cc><U>展望未来</U></FONT>](http://msdn.microsoft.com/zh-cn/magazine/cc500572.aspx#S4) 
[<FONT color=#0033cc><U>书籍和电话</U></FONT>](http://msdn.microsoft.com/zh-cn/magazine/cc500572.aspx#S5)</DIV></DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>偶尔， </FONT>**</SPAN>进化中的飞跃会迅速改善并重塑整个工程领域。在软件开发领域，C++ 编程语言的诞生就引发了此类飞跃。这种飞跃并非此语言本身所固有的。在 C++ 之前即已存在面向对象的语言（如 Simula67 和 Smalltalk）。但由于 C++ 是在 C 编程语言的基础上构建的（并且可编译现有的 C 程序），因此它可将面向对象思维的精髓带入主流。</DIV>
<DIV class=ArticleNormalPara>C++ 在软件设计和开发方面提供了大量的灵感，从设计模式到元编程莫不如此。并且由于其可在硬件平台间移植并具备更底层的表示，随着硬件变得更快更小，C++ 肯定会成中流砥柱。</DIV>
<DIV class=ArticleNormalPara>我最近有幸采访了 C++ 的发明者 Bjarne Stroustrup，其间谈及了多个话题：从他对语言的看法、行业的普遍发展趋势到他个人阅读的书目。其中许多问题都是读者们通过我的博客所提出的，在此特向那些提出问题的人员表示感谢。当然，更感谢 Bjarne。</DIV>

<DIV class=ArticleTypeTitle>对语言的看法</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>Howard Dierking </FONT>**</SPAN>为什么编程语言与人们之间有如此深的关联—以至各种语言都有了自己的“追星族”？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>Bjarne Stroustrup </FONT>**</SPAN>这个问题最好是问问心理学家、社会学家，甚至是经济学家，而不是一个计算机科学家！我猜想由于我们用来表达思想的语言已成为自身的一部分，因此，如果您仅了解一门语言，则很难与其他语言的支持者进行沟通。如果是这样，看起来必须要精通更多的语言。我认为，如果您仅了解一门编程语言，就不可能成为软件领域的专家。可能还有经济方面的原因：尽管理解基本原理可不受编程语言种类的限制，但许多实际技巧则不一样。因此，如果我仅了解语言 X 以及其工具集，而您赞成语言 Y 及其工具集，那么我们之间就会存在障碍。要想跨越这一障碍同样需要掌握多种语言以及工具集（并且牢固掌握基本原理）。遗憾地是，我所建议的解决方案并未考虑到大多数人没有多少空闲时间。那些狂热的粉丝是特例。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>IDE 在软件开发中应扮演何种角色？IDE 应如何对语言进行支持？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我并不经常使用 IDE。我赞赏那些可理解我的语言且反应迅速的 IDE 编辑器，但是也希望正常工作时不必用到 IDE。如果性能良好的 IDE 能得以普及，我可能会改变观点—即 IDE 实际成为语言的一部分，反之亦然（请参见**图 1**）。我所希望的代码可移植性在此会有用武之地。通过使用 C++，我希望通过源文件中的源代码就能理解我的系统。有些 IDE 机制包含的转换或生成代码非常不便于理解，我对此很不欣赏。</DIV>
<DIV class=ArticleImageSpacer>![](http://msdn.microsoft.com/zh-cn/magazine/cc500572.fig01.gif) 
<DIV class=ArticleImageCaptionText>Figure 1** The IDE Designer as a Language **(单击该图像获得较大视图)</DIV></DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您是否认为无用信息或可读性是当今通用语言中所存在的一个问题？如果是这样，应该怎样解决呢？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>语法越简单越好，但我猜想当大多数人在谈到可读性时，对于实际文字的抱怨并不多，表达内容的复杂性往往招来的抱怨更多。太多人都希望能了解以任意语言编写的任何程序，并希望在线支持工具只提供少量帮助就可以理解用于表达程序的所有结构和程序本身的所有逻辑。我们可以将其与查看并使用自然语言的方式做个对比。您是否希望无需背景知识即可理解莎士比亚的十四行诗？如果遇到原始古英语写的贝奥武夫又该怎么办呢？也许，我们对于编程语言的期望过高了。对于任意特定的应用程序而言，如果任何语言所表达的内容适用范围很大，往往会被视为过于复杂，但是实际上语言必须要适应多种应用程序。某一领域专用的语言只能适用于特例，但是现在我们必须面对多种语言及其交互所带来的复杂问题。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>通用语言应如何支持应用程序设计理念（如组件编程和服务编程）？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>通用语言应支持编写可表达常规和特定于应用程序概念的库，支持工具构建以及提供连接应用程序的不同部分所需的“粘合剂”。为此，语言需具有灵活性、富有表现力的系统、良好的基础性能以及长期稳定性。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>多重分派是否是件好事？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>是的。传统的单一分派面向对象编程语言（如 Simula、C++、Smalltalk、Java 和 C#）无法完美地表达具体类型要到运行时才知道的简单操作（如将数字相乘或查找两个形状的交集）。生成代码（由双重分派、访问者模式等因素确定）很慢并且不易维护。在运行时支持多重分派的语言（如 Dylan 和 CLOS）要好些，而在编译时支持多重分派的语言（如 C++）有时效果更好。去年，我和我的一些学生一起发布了一篇有关如何向 C++ 清晰添加多重分派的研究论文。与我们看过的所有解决方法相比，使用多重分派时生成的代码更短、更简单、占用的内存更少，并且运行速度更快（请参见**图 2**）。虽然对于 C++0x 而言，该工作太晚了。您可在 [<FONT color=#800080><U>research.att.com/~bs/multimethods.pdf</U></FONT>](http://research.att.com/~bs/multimethods.pdf) 中找到相关的文章。</DIV>
<DIV class=ArticleImageSpacer>![](http://msdn.microsoft.com/zh-cn/magazine/cc500572.fig02.gif) 
<DIV class=ArticleImageCaptionText>Figure 2** Multiple Dispatch via Multiple Inheritance **(单击该图像获得较大视图)</DIV></DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您对现在 C++ 中的协变/逆变是什么看法？您是否希望在未来的语言版本中改变当前的行为？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>没有这种想法。在该领域，我认为 C++ 充分发挥了作用。您有没有考虑过将 vector&lt;Apple&gt; 转换成 vector&lt;Fruit&gt; 这类示例？这样做非常不安全，除非 vector&lt;Fruit&gt; 是固定不变的（或者可向其添加 Orange）。我不认为隐式运行时类型检查是用于解决此类问题的好方法。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>相对于方法调用，您对使用消息来传递关键语言功能是什么看法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我喜欢显式消息传递，但我仅在很久以前在分布式系统环境中用过它。实际上，要普及它的使用，我们必须针对语言和工具做一些细致的工作，这样才能支持消息传递。我认为这方面的工作尚未完成，当然，也许是我弄错了。如果依赖消息和消息队列，则可以避开许多与共享和锁定相关的问题。我非常希望在几年内能看到用于此类功能的标准 C++ 库，并且我会亲自体验一下。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>如果既要支持千差万别的诸多受众，又要提倡改进代码表现力和设计精致度，这对通用语言而言是否存在固有冲突？语言在对后者的支持方面扮演着何种角色？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我想从某种意义上讲，可通过特殊化来实现精致。如果您的受众（用户社区）很小，可投其所好，要是您还是用户社区的灵魂，即可撇开传统的表示法和概念（宣称“你需要研读类型理论才能使用它”）。对通用语言的要求不仅包括需支持各种用途，还包括需要让设想和教育背景各异的一大群人能学会（基本知识必须可供学业不精的高中生使用）。</DIV>
<DIV class=ArticleNormalPara>因此，我认为只要能通过通用语言来表达完善的代码，该语言就可提倡精益求精。对于 C++，可编写非常完善的代码。使其成为通用语言的一部分并且广泛可用，此类示例可供大众和读者数量巨大的文章和课本使用。没有哪种特殊化语言（虽然也具备完善的代码）可做到这些。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>我们是否太过强调体系结构了？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>没有，至少我定义体系结构的方式并非如此。恰好相反，对于体系结构的强调太少，而不理解结构化原理的糟糕编码太多了。我认为体系结构方面的主要问题是：许多程序员对于什么能让好编码发挥正常功效只有一个非常模糊的概念。看到并意识到还不够。制定规则来确定禁止执行的操作也不够。我们需要连贯的法定规则。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>开源社区对于软件质量、设计和职业水准是有所帮助、有所损害还是没有影响？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>这真是个很难回答的问题。我看到过能有所帮助的情况（提高了代码质量以及相关人员的职业水准），也遇到过有损害的情况（教授真的很糟糕的习惯和态度），还有许多情况我也说不清。</DIV>
<DIV class=ArticleNormalPara>我无法猜想对社区产生的普遍影响，或者如果开源工作的力度更大一些或更小一些，会发生什么情况。对我来说，社区太大了，猜不透。</DIV>

<DIV class=ArticleTypeTitle>语言发展趋势</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您是否认为在不久的将来动态语言会成为焦点？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>应该不会。我想人们对苹果和桔子比较得太多了。我认为一般意义的选择不适合静态和动态语言，并且语言也不能严格地分为这两种：绝大部分动态语言都有需要用静态方式确定的内容，所有主要静态语言也都可完成需在运行时确定值含义的操作。当然，一定会有一些我猜不出的流行趋势；但是我认为在现实中，仍是根据应用程序的需求、应用领域和/或开发人员现有的技能来理性选择语言的。例如，我不会尝试使用 Ruby（例如）来实现 Java 运行时，也不会用 C++ 表达交互性很强的模拟语言（这一点与它的实现相反）。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您是否认为最终删除 void*/variant/object 这类项目是件好事？（**图 3** 显示了一个不错的示例。）</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>删除所有内容包罗万象的选项从理论上是行得通的，但在实际中，前提条件是我们必须对要表达的所有内容进行完整分类，以便确保针对性。例如，我从未看到过实际有函数可对每个对象执行操作；想要执行操作，始终需要对所操作的内容进行一些假定（我认为纯粹的转发函数是最接近计数器示例的函数）。当前在 C++ 概念方面的工作（有关泛型算法的约束/需求）可以提供帮助，但是如果缺少一些实际的常规机制来表达“是我确实不知道的内容”，就不能处理意外需求，我们也就无所作为。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>随着松散类型语言又成为了流行趋势，我们是否应该重新考虑匈牙利表示法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我不肯定有这种趋势，尽管在总体工作中，适合松散类型语言的份额可能有所增加。换句话说，虽然松散类型语言的使用率增加得更快，静态类型语言的使用率也在增加（我认为是这样）。但是绝不要使用匈牙利表示法。那是个很糟糕的主意。源代码应该反映程序的内涵，而不是模拟类型系统。如果确实觉得需要使用匈牙利表示法，可能是使用的语言与应用程序不搭配。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>在 2000 年，您做了一场讲座，题目是“C++：用于新千年的新语言”，并且介绍了 Wrap&lt;T&gt; 这一概念。这基本上就是面向方面的编程 (AOP)。您对 AOP 及其 Wrap&lt;T&gt; 模式（Pointcut、Advice 等等）的形式化有何看法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我对于 AOP 的研究不多，因此不能给出一个可靠的答案。我喜欢在语言中支持组合（尤其是非入侵式），而不是“不支持”和“通过某个工具支持”。我担心的是超语言工具和非标准工具链。C++ 模板在非入侵式组合方面极其成功 — 只需看看标准模板库 (STL) 和一些嵌入式系统编程中的使用情况就能了解这一点。要是进行组合时不必将概念强行放入刚性或预先设计的层次中，那当然不错。然而，对于某些开发人员和维护人员而言，太难于管理了。并且太过繁重。C++0x 将尝试在不影响灵活性和性能的情况下解决这一问题。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>C++ 语言的某些特征可能会产生一些令人讨厌的意外结果（比如宏）。您希望 C++ 或常见的流行语言能消除其他哪些意外结果？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>实际上，当我开始使用 C 时，我就知道宏非常令人头疼；但是与大多数人一样，我对它造成的麻烦和普遍的负面影响还是估计不足。在 C 中普遍使用宏可能是十年前 C++ 开发环境不尽人意的主要原因。并且 C++ 中初始化对象方法还太多太混乱；我希望在 C++0x 中使用统一的机制来解决该问题。我回答上一问题时提到过模板。它们使后来的 C++（1985 年发布）取得了巨大的成功，但对语言也有负面影响—同样，C++0x 中有一部分内容会专门解决这一问题。</DIV>
<DIV class=ArticleNormalPara>由于兼容性原因，C++ 无法解决我们在过去所发现的许多小问题和不大不小的问题。例如，声明符语法没必要这么复杂，能说明任意线性表示法就足够了。此外，许多默认值也存在错误：构造函数不应默认为转换，名称不应默认为可从其他源文件访问等等。不能控制链接程序一直是问题根源；特别是实现者似乎喜欢以不兼容的形式提供类似的功能。</DIV>
<DIV class=ArticleNormalPara>当然也有意外的收获。其中最出色的是，在与资源管理和错误处理（使用异常）相关的技术中普遍使用了析构函数。我知道析构函数是个不错的主意，因为您总会需要用到构造函数的逆向功能—但是，对于它们在合理使用 C++ 方面的重要性，我还不太了解。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您曾在自己的网站上说道：“我认为我们应该力求完善所构建的应用程序，而不是完善语言本身。”在域特定语言 (DSL) 方面的发展是否就集中体现了您的这一说法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>是的，这是很明确的。它经常向这个方向做尝试。有时甚至会达到预期的效果。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您对于 DSL 的主要看法是什么？您设想 DSL 和通用语言之间是怎样的一种关系？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我担心的是许多语言在设计、实现和引进时声势浩大，然后就销声匿迹，没有任何显著的实效。在此期间—通常是多年的长期开发—新语言耗费了大量资源，却基本没有回报。我曾写过一篇有关此现象的文章，名为“语义增强库语言的基本原理”([<FONT color=#0033cc><U>research.att.com/~bs/SELLrationale.pdf</U></FONT>](http://research.att.com/~bs/SELLrationale.pdf))。我赞成使用库（可能通过工具来支持）和通用语言。</DIV>
<DIV class=ArticleNormalPara>我认为 DSL 应是不得已而为之的办法，而非首选方法。如果可能的话，DSL 应牢牢地根植于通用语言和标准工具链之中。DSL 需要通用语言（或者至少是系统编程语言）完成自身实现并实现其运行时基元。我认为最好是 DSL 有意识地与至少一门通用语言稳定配合使用，这样即可通过使用以通用语言编写的库来轻松地添加新功能。专业人员自然应掌握多门语言，但是我担心各种 DSL 的复杂性汇集起来可能产生问题。并且，许多 DSL 似乎都“想”成为通用语言。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您提到过由于不同硬件存在定义的多样性，所以 C++ 中的许多构造有意模糊不清。您有没有发现互操作性方面已经做出了改进，让一些这样的构造变得明确了？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我认为“模糊不清”这个词并不准确。太多东西都没有定义或者要在实现时才定义。我想如果能从头重新定义 C++ 的话，它可能会没有未定义的行为，要在实现时定义的行为也会少许多。但是，我没有逆转时光的机器，也不能用现在的一组决议毁掉亿万行代码。</DIV>

<DIV class=ArticleTypeTitle>方法和最佳实践</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您比较喜欢使用和传授哪种过程方法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>确定关键应用程序概念，确定有用的库，构建新库以支持应用程序概念、尽早试验想法、尽早集成、尽早且经常进行测试，将文档和教程资料用作设计工具，并且从小程序生成大程序（反复）。显然我刚才侧重的是相对较小的项目。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您有没有发现语言和开发方法之间存在交叉或密切关系？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>库设计被视为设计/开发技术时，我认为是这样的。通过库构建来关注越来越高级（更接近应用程序）的功能对语言提出了要求。我不想过分夸大这一点，但是我认为您不能拥有用于（例如）COBOL、C、Java、C++ 以及 Python 的单个开发方法，而又期望从每种语言获得更多的支持。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>在创建软件时，您个人有哪些经验法则？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>注重关键概念、它们的接口、资源（内存、文件、锁定等等）的管理和错误处理。为类设计良好的不变体和“资源获取即初始化”(RAII) 是关键技术。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>对于敏捷有各种说法。“敏捷”对您意味着什么？C++ 是否支持敏捷？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我不使用这个词，它太含糊不清。C++ 当然支持敏捷—无论它表示什么意思。</DIV>

<DIV class=ArticleTypeTitle>展望未来</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>一种语言应如何发展才能既支持高级功能（如模板、动态事件以及自我编写的代码），同时又能让新手理解这种语言？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我不知道。我想并不存在一个通用的答案。如果新功能支持更有效的技术，那么它们就在语言中占有重要的地位。但是，稳定性至关重要：C 和 C++ 保持效力的一个原因是标准委员会要求旧代码（通常有十年的历史）仍然有效且可顺畅地集成新功能。至少可以说这并不太容易，并且引入新功能并不总是成功。标准委员会成员经常忽视新手因素。为 C++0x 计划的一些关键功能（如统一初始化、自动关键字（从其初始化表达式推断出变量类型）以及如检查模板参数要求等概念）应能使语言更易于被普通人员使用。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您是否将语言元数据看作未来编程语言的重要基础？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>不，我个人非常不喜欢大量使用元数据。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>如果 CPU 开始上升为多核，您是否认为并发性未来会发生重大转变？C++0x 会如何解决并发性挑战？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>C++0x 具备基础：适用于多线程的机器模型，用于构建库的一组基础基元以及线程和锁定库 API。我希望看到（可能在今后的几年里）更多这类坚实的基础，尤其是更为简单且级别更高的并发模型，它们以线程池、功能和消息队列为基础。我们需要找到自动或近似自动的方法，将计算分布到多个处理器并在这些处理器上进行局部处理。在这一领域中已有许多模型（许多是使用的 C++），但都还不是主要模型。示例包括德克萨斯 A&amp;M 大学的 STAPL 和英特尔的 TBB。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您对图形处理单元 (GPGPU) 方面的通用计算有什么看法？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>非常着急，但除了利用特定用途的处理器需要多种技能这一明显现象外，我并没有实际经验可做出更多评论。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您是否预见高性能计算 (HPC) 最终会对编程人员透明？此外，它是否会成为语言、库或框架的目标？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>略微有些透明；我不认为并发性可以或应该完全透明。对于初学者而言，错误处理可能会非常困难，具体取决于处理器的可用性、有无共享内存、分布与否以及延迟。在适当的地方接近透明现在是并将一直是新语言、新语言功能以及（我最喜欢的）新库的目标。仅当底层语言提供了作为机器模型所需的基本保证以及一组非常底层的基元，后者才可行。C++0x 将会做到这一点。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>C++0x 的设计进展到什么程度了？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>马上要最后完工了！至少我希望如此—在出现最终结果前，谁都预料不到会出什么变化，越接近完成，情绪越紧张激动。当前的计划是在六月选出完整的新标准以供公众评论，12 到 18 个月后出台一个正式的新标准。我完全可以围绕这个主题写一本书：如何制定标准，指导方针应该是什么以及其中具体有些什么？这些内容我基本都写到了，请参阅我的 HOPL 文章“在现实世界中不断改进语言：C++ 1991-2006”（位置为 [<FONT color=#0033cc><U>research.att.com/~bs/hopl-almost-final.pdf</U></FONT>](http://research.att.com/~bs/hopl-almost-final.pdf)）以及我主页文章中有关 C++0x 的所有内容。如果您的要求很高，可在 Web 上查找“WG21”并搜索所有 ISO C++ 标准委员会的文章（包括所有建议）。它至少可以让您确信其中包括大量工作。改进一门使用广泛的编程语言非常困难，如果语言处在许多工具、语言和应用程序的实施层就更困难。您还可以在网上和我的 C++ 页面上找到许多我和其他人提供的视频资料。</DIV>

<DIV class=ArticleTypeTitle>书籍和电话</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您目前在看什么书？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>技术资料方面，我重读了 Hennessy 和 Patterson 撰写的计算机体系结构方面的书，还在努力收集可以帮助人们编写出色代码的文章和书籍（我正在教的一门课也能用到）。查找此类文章的难度超出了我的想象；学术文章太过专业化，而非学术文章往往说得好听，实际作用又不大（欢迎提供建议）。当然，我还读些 C++ 标准化方面的文章。我原准备重读 Knuth，但由于有人偷偷拿走了我的第 III 卷，所以只好再等等。我还将再读一遍 O'Brian 的 Aubrey 和 Maturin 系列，就当是个消遣。为了加深对科学的理解，目前打算拜读 Richard Dawkins。我最近还读完了 Rodger 的《Command of the Ocean:A Naval History of Britain, 1649-1815》（因此准备读 O'Brian 的书）。</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>HD </FONT>**</SPAN>您曾说过：“我过去常常希望我的计算机能像电话一样容易使用；现在这个愿望终于实现了，我不用再费心研究电话用法了。”您有智能电话吗？它的操作更简单了吗？</DIV>
<DIV class=ArticleNormalPara><SPAN class=ArticleInlineTitle>**<FONT color=#003399>BS </FONT>**</SPAN>我并不太热衷使用电话。我更喜欢面对面的交流，如果无法面对面，用书面文字也行。即便最炫的电话也比不上一封措辞完美的电子邮件。我使用是一款很薄并且小巧玲珑的手机，但是它的大部功能我都用不上。声音质量和可靠性也很不尽人意。公平地讲，现在的用户界面比我说出那番言论时要好得多了。</DIV>

<DIV class=ContentSeparator>
<DIV class=AuthorBio>**Howard Dierking** 是《MSDN 杂志》_的主编。_</DIV></DIV></DIV></DIV>
</div>