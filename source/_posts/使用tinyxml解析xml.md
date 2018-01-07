---
title: 使用TinyXML解析XML
id: 152
categories:
  - C++
date: 2009-06-08 22:59:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 由于工作需要，以前都是直接硬写在程序中的堆砌代码，现在决定都改为XML描述了，一来程序看起来清爽多了，二来以后扩展也方便。项目中使用的是TinyXML，今天去其官方网站逛了一圈，其中有个Tutorial写的很好，最后有个例子代码，个人感觉稍作修改完全可以拿到实际项目中使用，管他有多少个Node一个递归即搞定：<pre style="border: 1px dotted #785;background: #f5f5f5;">// tutorial demo program
#include "stdafx.h"
#include "tinyxml.h"

// ----------------------------------------------------------------------
// STDOUT dump and indenting utility functions
// ----------------------------------------------------------------------
const unsigned int NUM_INDENTS_PER_SPACE=2;

const char * getIndent( unsigned int numIndents )
{
static const char * pINDENT="                                      + ";
static const unsigned int LENGTH=strlen( pINDENT );
unsigned int n=numIndents*NUM_INDENTS_PER_SPACE;
if ( n &gt; LENGTH ) n = LENGTH;

return &amp;pINDENT[ LENGTH-n ];
}

// same as getIndent but no "+" at the end
const char * getIndentAlt( unsigned int numIndents )
{
static const char * pINDENT="                                        ";
static const unsigned int LENGTH=strlen( pINDENT );
unsigned int n=numIndents*NUM_INDENTS_PER_SPACE;
if ( n &gt; LENGTH ) n = LENGTH;

return &amp;pINDENT[ LENGTH-n ];
}

int dump_attribs_to_stdout(TiXmlElement* pElement, unsigned int indent)
{
if ( !pElement ) return 0;

TiXmlAttribute* pAttrib=pElement-&gt;FirstAttribute();
int i=0;
int ival;
double dval;
const char* pIndent=getIndent(indent);
printf("/n");
while (pAttrib)
{
printf( "%s%s: value=[%s]", pIndent, pAttrib-&gt;Name(), pAttrib-&gt;Value());

if (pAttrib-&gt;QueryIntValue(&amp;ival)==TIXML_SUCCESS)    printf( " int=%d", ival);
if (pAttrib-&gt;QueryDoubleValue(&amp;dval)==TIXML_SUCCESS) printf( " d=%1.1f", dval);
printf( "/n" );
i++;
pAttrib=pAttrib-&gt;Next();
}
return i;
}

void dump_to_stdout( TiXmlNode* pParent, unsigned int indent = 0 )
{
if ( !pParent ) return;

TiXmlNode* pChild;
TiXmlText* pText;
int t = pParent-&gt;Type();
printf( "%s", getIndent(indent));
int num;

switch ( t )
{
case TiXmlNode::DOCUMENT:
printf( "Document" );
break;

case TiXmlNode::ELEMENT:
printf( "Element [%s]", pParent-&gt;Value() );
num=dump_attribs_to_stdout(pParent-&gt;ToElement(), indent+1);
switch(num)
{
case 0:  printf( " (No attributes)"); break;
case 1:  printf( "%s1 attribute", getIndentAlt(indent)); break;
default: printf( "%s%d attributes", getIndentAlt(indent), num); break;
}
break;

case TiXmlNode::COMMENT:
printf( "Comment: [%s]", pParent-&gt;Value());
break;

case TiXmlNode::UNKNOWN:
printf( "Unknown" );
break;

case TiXmlNode::TEXT:
pText = pParent-&gt;ToText();
printf( "Text: [%s]", pText-&gt;Value() );
break;

case TiXmlNode::DECLARATION:
printf( "Declaration" );
break;
default:
break;
}
printf( "/n" );
for ( pChild = pParent-&gt;FirstChild(); pChild != 0; pChild = pChild-&gt;NextSibling()) 
{
dump_to_stdout( pChild, indent+1 );
}
}

// load the named file and dump its structure to STDOUT
void dump_to_stdout(const char* pFilename)
{
TiXmlDocument doc(pFilename);
bool loadOkay = doc.LoadFile();
if (loadOkay)
{
printf("/n%s:/n", pFilename);
dump_to_stdout( &amp;doc ); // defined later in the tutorial
}
else
{
printf("Failed to load file /"%s/"/n", pFilename);
}
}

// ----------------------------------------------------------------------
// main() for printing files named on the command line
// ----------------------------------------------------------------------
int main(int argc, char* argv[])
{
for (int i=1; i&lt;argc; i++)
{
dump_to_stdout(argv[i]);
}
return 0;
}
</pre> 

递归和树简直就是绝配，前阵子做等了部分加载联系人到树控件时也是用递归搞定的，有时间再把递归好好研究下。

</div>