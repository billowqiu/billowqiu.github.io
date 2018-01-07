---
title: 行为模式之Command
id: 132
categories:
  - 设计模式
date: 2009-10-19 22:40:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &ldquo;本来是打算看ThinkInC++上面的设计模式学习的，看到第二个模式也就是Command发现上面讲得实在是太少了，只好看那本经典的EBook了，幸好还算清晰，第一章看完一半就决定买一本纸质书了，犹豫了两天昨天晚上终于下单了，Head First 设计模式，明天估计就到货了，希望选择是对的。&rdquo;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 刚刚把EBook上面的Commad一节看完，对这个模式有一点点体会了，但是由于没有在实际项目中用到也只能是纸上谈兵了，主要点就是将调用封装为对象，类似于函数对象吧，在调用和执行之间有个中介Command。涉及的主要参与者有Command-抽象类；CocreateCommand-具体的命令类；Client-应用程序，创建一个具体的命令并设置其接受者；Invoker-调用者，调用Command的接口；Receiver-接受者，执行具体命令类的请求，KoF说任何类都可以是Receiver，下面是KoF 的结构图：

![序列图](http://p.blog.csdn.net/images/p_blog_csdn_net/ToCpp/EntryImages/20091019/commad-action.gif)

&nbsp;

下面是类图，Invoker和Command是聚合关系(包含但是不是必须)，ConcreteCommand继承Command，Client和ConcreteCommand单向关联与Receiver，Client依赖于ConcreteCommand，即Client必须得ConcreteCommand才能完成工作

![结构图](http://p.blog.csdn.net/images/p_blog_csdn_net/ToCpp/EntryImages/20091019/command.gif)

下面是KoF的简短实现：

<pre style="border: 1px dotted #785;background: #f5f5f5;">class Command {
    public:
        virtual ~Command();

        virtual void Execute() = 0;
    protected:
        Command();
    };
class OpenCommand : public Command {
    public:
        OpenCommand(Application*);

        virtual void Execute();
    protected:
        virtual const char* AskUser();
    private:
        Application* _application;
        char* _response;
    };
OpenCommand::OpenCommand (Application* a) {
        _application = a;
    }

    void OpenCommand::Execute () {
        const char* name = AskUser();

        if (name != 0) {
            Document* document = new Document(name);
            _application-&gt;Add(document);
            document-&gt;Open();
        }
    }
class PasteCommand : public Command {
    public:
        PasteCommand(Document*);

        virtual void Execute();
    private:
        Document* _document;
    };

    PasteCommand::PasteCommand (Document* doc) {
        _document = doc;
    }

    void PasteCommand::Execute () {
        _document-&gt;Paste();
    }</pre> 

对于简单的不需要参数和取消操作的Template版本：

<pre style="border: 1px dotted #785;background: #f5f5f5;">template &lt;class Receiver&gt;
    class SimpleCommand : public Command {
    public:
        typedef void (Receiver::* Action)();

        SimpleCommand(Receiver* r, Action a) :
            _receiver(r), _action(a) { }

        virtual void Execute();
    private:
        Action _action;
        Receiver* _receiver;
    };
template &lt;class Receiver&gt;
    void SimpleCommand&lt;Receiver&gt;::Execute () {
        (_receiver-&gt;*_action)();
    }
MyClass* receiver = new MyClass;
    // ...
    Command* aCommand =
        new SimpleCommand&lt;MyClass&gt;(receiver, &amp;MyClass::Action);
    // ...
    aCommand-&gt;Execute();
</pre> 

先贴上这么多，不知道哪一天会在项目中应用到，慢慢体会&hellip;&hellip;

</div>