---
title: boost.python
date: 2017-11-19 17:55:37
tags:
---
最近工作中使用到boost.python，感觉网上这方面的资料还是比较少，官方文档写的也不够详尽，算是做个备忘吧

    const char* greet()
    {
        return "hello,world";
    }
    // operator= for index

    typedef std::vector<int> VI;
    struct Foo
    {
        int32_t a;
        std::string b;
        VI vi;
        bool operator==(Foo const& f) const { return f.a == a; }
    };

    // A friendly class.
    typedef std::vector<Foo> VecFoo;
    class hello
    {
    public:
        hello(const std::string& country) { this->country = country; }
        std::string greet() const { return "Hello from " + country; }
        Foo get_foo()const
        {
            Foo foo;
            foo.a = 123;
            foo.b = country;
            return foo;
        }
    public:
        std::string country;
        VecFoo vf;
    };

    template <typename T>
    struct VecConvert
    {
        typedef std::vector<T> VecT;
        static PyObject* convert(const VecT& vt)
        {
            boost::python::list pylist;
            for (auto& t : vt)
            {
                pylist.append(t);
            }
            return boost::python::incref(pylist.ptr());
        }
    };    
* 导出函数及POD结构
        boost::python::def("greet", greet);
        boost::python::class_<Foo>("Foo")
            .def_readwrite("a", &Foo::a)
            .def_readwrite("b", &Foo::b)
            .def_readwrite("vi", &Foo::vi);
* 导出类
        boost::python::class_<hello>("hello", init<std::string>())
            // Add a regular member function.
            .def("greet", &hello::greet)
            // Add invite() as a member of hello!
            .def("invite", invite)
            .def("get_foo", &hello::get_foo)
            .def_readwrite("vf", &hello::vf);  
* 导出容器
   1.  导出一个新的容器类
            boost::python::class_<VI>("VI").def(boost::python::vector_indexing_suite<VI>());
   2.  直接转换为python的list
            boost::python::to_python_converter<VI, VecConvert<int>>();
* python list对象转换为c++ vector
        //c++
        void setFoos(const boost::python::list& pylist)
        {
            std::vector<Foo> fs;
            for (int i =0; i< boost::python::len(pylist); i++)
            {
                fs.push_back(boost::python::extract<Foo>(pylist[i]));
            }
        }
        boost::python::def("setFoos", setFoos);
        //python
        Foo f1,f2
        setFoos([f1, f2])
