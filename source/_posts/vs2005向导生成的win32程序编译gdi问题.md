---
title: VS2005向导生成的win32程序编译GDI+问题
id: 139
categories:
  - VC
date: 2009-09-26 13:59:00
tags:
---

    

昨天用VS2005向导生成的Win32程序，当向其中添加GDI+相关文件引用时会提示如下错误

&nbsp;

1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(67) : error C4430: 缺少类型说明符 - 假定为 int。注意: C++ 不支持默认 int
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(67) : error C2440: &ldquo;初始化&rdquo;: 无法从&ldquo;const char [37]&rdquo;转换为&ldquo;int&rdquo;
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 没有使该转换得以执行的上下文
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(67) : error C2146: 语法错误 : 缺少&ldquo;;&rdquo;(在标识符&ldquo;IImageBytes&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(67) : error C2470: &ldquo;IImageBytes&rdquo;: 看起来像函数定义，但没有参数列表；跳过明显的函数体
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(67) : error C2059: 语法错误 : &ldquo;public&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(246) : error C2146: 语法错误 : 缺少&ldquo;;&rdquo;(在标识符&ldquo;id&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(246) : error C4430: 缺少类型说明符 - 假定为 int。注意: C++ 不支持默认 int
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusimaging.h(246) : error C4430: 缺少类型说明符 - 假定为 int。注意: C++ 不支持默认 int
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(384) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(395) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(405) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(443) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(444) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(445) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(453) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(465) : error C2535: &ldquo;Gdiplus::Image::Image(void)&rdquo;: 已经定义或声明成员函数
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(384) : 参见&ldquo;Gdiplus::Image::Image&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(499) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusheaders.h(510) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : error C2065: &ldquo;IStream&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : error C2065: &ldquo;stream&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : error C2065: &ldquo;image&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : error C2275: &ldquo;Gdiplus::GpImage&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusgpstubs.h(61) : 参见&ldquo;Gdiplus::GpImage&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1133) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1139) : error C2275: &ldquo;Gdiplus::GpImage&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusgpstubs.h(61) : 参见&ldquo;Gdiplus::GpImage&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1139) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1139) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1156) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1244) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1247) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1250) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1262) : error C2061: 语法错误 : 标识符&ldquo;PROPID&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1275) : error C2065: &ldquo;bitmap&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1275) : error C2275: &ldquo;Gdiplus::GpBitmap&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusgpstubs.h(62) : 参见&ldquo;Gdiplus::GpBitmap&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1275) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1275) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1281) : error C2275: &ldquo;Gdiplus::GpBitmap&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusgpstubs.h(62) : 参见&ldquo;Gdiplus::GpBitmap&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1281) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(1281) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2128) : error C2065: &ldquo;header&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2128) : error C2275: &ldquo;Gdiplus::MetafileHeader&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetaheader.h(112) : 参见&ldquo;Gdiplus::MetafileHeader&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2128) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2128) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2146) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2166) : error C2065: &ldquo;metafile&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2166) : error C2275: &ldquo;Gdiplus::GpMetafile&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusgpstubs.h(63) : 参见&ldquo;Gdiplus::GpMetafile&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2166) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2166) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2213) : error C2275: &ldquo;HDC&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(260) : 参见&ldquo;HDC&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2213) : error C2146: 语法错误 : 缺少&ldquo;)&rdquo;(在标识符&ldquo;referenceHdc&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2213) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2213) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2213) : error C2275: &ldquo;HDC&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(260) : 参见&ldquo;HDC&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2219) : error C2059: 语法错误 : &ldquo;)&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2224) : error C2275: &ldquo;HDC&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(260) : 参见&ldquo;HDC&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2224) : error C2146: 语法错误 : 缺少&ldquo;)&rdquo;(在标识符&ldquo;referenceHdc&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2224) : warning C4229: 使用了记时错误 : 忽略数据上的修饰符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2224) : error C2078: 初始值设定项太多
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2224) : error C2275: &ldquo;HDC&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(260) : 参见&ldquo;HDC&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusflat.h(2230) : error C2059: 语法错误 : &ldquo;)&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdipluspath.h(133) : error C2061: 语法错误 : 标识符&ldquo;byte&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(80) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(197) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(197) : error C2535: &ldquo;Gdiplus::Metafile::Metafile(void)&rdquo;: 已经定义或声明成员函数
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(80) : 参见&ldquo;Gdiplus::Metafile::Metafile&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(213) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(213) : error C2535: &ldquo;Gdiplus::Metafile::Metafile(void)&rdquo;: 已经定义或声明成员函数
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(80) : 参见&ldquo;Gdiplus::Metafile::Metafile&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(231) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(231) : error C2535: &ldquo;Gdiplus::Metafile::Metafile(void)&rdquo;: 已经定义或声明成员函数
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(80) : 参见&ldquo;Gdiplus::Metafile::Metafile&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(277) : error C2061: 语法错误 : 标识符&ldquo;IStream&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(370) : error C2535: &ldquo;Gdiplus::Metafile::Metafile(void)&rdquo;: 已经定义或声明成员函数
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(80) : 参见&ldquo;Gdiplus::Metafile::Metafile&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(207) : error C2065: &ldquo;referenceHdc&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(207) : error C2065: &ldquo;type&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(208) : error C2065: &ldquo;description&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(225) : error C2065: &ldquo;frameRect&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusmetafile.h(225) : error C2065: &ldquo;frameUnit&rdquo;: 未声明的标识符
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(45) : error C2275: &ldquo;BOOL&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(152) : 参见&ldquo;BOOL&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(45) : error C2146: 语法错误 : 缺少&ldquo;)&rdquo;(在标识符&ldquo;useEmbeddedColorManagement&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(45) : error C2761: &ldquo;{ctor}&rdquo;: 不允许成员函数重新声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(45) : error C2059: 语法错误 : &ldquo;)&rdquo;
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(46) : error C2143: 语法错误 : 缺少&ldquo;;&rdquo;(在&ldquo;{&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(46) : error C2447: &ldquo;{&rdquo;: 缺少函数标题(是否是老式的形式表?)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(80) : error C2275: &ldquo;BOOL&rdquo;: 将此类型用作表达式非法
1&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; f:/microsoft visual studio 8/vc/platformsdk/include/windef.h(152) : 参见&ldquo;BOOL&rdquo;的声明
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(80) : error C2146: 语法错误 : 缺少&ldquo;)&rdquo;(在标识符&ldquo;useEmbeddedColorManagement&rdquo;的前面)
1&gt;f:/microsoft visual studio 8/vc/platformsdk/include/gdiplusbitmap.h(80) : error C2761: &ldquo;Gdiplus::Image *Gdiplus::Image::FromStream(void)&rdquo;: 不允许成员函数重新声明

&nbsp;

但是自己新建一个空工程然后添加相关的文件就没问题，找了半天猜猜可能是StdAfx里面的宏有问题，试了几下发现然来是

#define WIN32_LEAN_AND_MEAN&nbsp;&nbsp;// 从 Windows 头中排除极少使用的资料

这个定义导致的，去掉就OK了，没想到M$自己的东西都这样。

</div>