---
title: MFC透明控件的超级简单实现
id: 183
categories:
  - VC
date: 2008-05-14 00:44:00
tags:
---

    

今天在翻出以前写的一个程序时，想改下对话框的背景，结果导致Static控件的背景留下灰色的默认背景色，感觉很不爽，想着也学了这么久MFC了，这个问题今天一定要解决了，大致思路还是知道的，但是由于一直都是在学习，看别人的代码所以真真要自己写还是得仔细琢磨下，经过几番测试发现下面的方法最容易，贴上代码和截图吧：

// CQTTransparentStatic

class CQTTransparentStatic : public CStatic
{
DECLARE_DYNAMIC(CQTTransparentStatic)
public:
CQTTransparentStatic();
virtual ~CQTTransparentStatic();
//响应父类的反射消息
afx_msg HBRUSH CtlColor(CDC* /*pDC*/, UINT /*nCtlColor*/);
protected:
DECLARE_MESSAGE_MAP()
};

IMPLEMENT_DYNAMIC(CQTTransparentStatic, CStatic)

CQTTransparentStatic::CQTTransparentStatic()
{

}

CQTTransparentStatic::~CQTTransparentStatic()
{
}

BEGIN_MESSAGE_MAP(CQTTransparentStatic, CStatic)
ON_WM_CTLCOLOR_REFLECT()
END_MESSAGE_MAP()

// CQTTransparentStatic 消息处理程序

HBRUSH CQTTransparentStatic::CtlColor(CDC* pDC, UINT nCtlColor)
{
// TODO: 在此更改 DC 的任何属性
// TODO: 如果不应调用父级的处理程序，则返回非空画笔
pDC-&gt;SetBkMode(TRANSPARENT);
return (HBRUSH)::GetStockObject(HOLLOW_BRUSH);
}

代码真的是很简单吧，哈哈，

<DIV forimg="1">![](http://hiphotos.baidu.com/qiutao%5F2008/pic/item/2742ef012d51c71d7aec2c49.jpg)</DIV>

<SPAN style="FONT-SIZE: 10.5pt; mso-bidi-font-size: 12.0pt; mso-fareast-: 1.0pt">怎么样效果也还不错呢。</SPAN>

</div>