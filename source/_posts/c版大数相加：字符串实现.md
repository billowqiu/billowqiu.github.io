---
title: C++版大数相加：字符串实现
id: 102
categories:
  - C++
date: 2011-06-01 23:15:00
tags:
---

所以大数即，一般的整数类型无法直接表示，通常即通过字符串表示，下面是用c++字符串实现相加，比较粗糙，但大致原理应该是表述清楚了：
<pre class="brush:cpp">std::string stringAdd( const std::string&amp; strLeft, const std::string&amp; strRight )
{
    //结果最长为较长数字加1
    int nLeftLength = strLeft.length();
    int nRigthLength = strRight.length();
    int nLargeLength = nLeftLength &gt;= nRigthLength ? nLeftLength : nRigthLength;
    std::string strResult( nLargeLength+1, &#39;0&#39; );
    int nCount = 0;
    while( nLargeLength )
    {
        char nTempLeft;
        if( nLeftLength )
        {
            nTempLeft = strLeft[--nLeftLength];
        }
        else
        {
            nTempLeft = &#39;0&#39;;
        }
        cout &lt;&lt; &quot;tempLeft:&quot; &lt;&lt; nTempLeft &lt;&lt; std::endl;
        char nTempRigth;
        if( nRigthLength )
        {
            nTempRigth = strRight[--nRigthLength];
        }
        else
        {
            nTempRigth = &#39;0&#39;;
        }
        cout &lt;&lt; &quot;tempRight:&quot; &lt;&lt; nTempRigth &lt;&lt; std::endl;
        //计算当前位的值
        int nTemp = nTempLeft-&#39;0&#39; + nTempRigth-&#39;0&#39; + strResult[nLargeLength]-&#39;0&#39;;
        cout &lt;&lt; &quot;temp:&quot; &lt;&lt; nTemp &lt;&lt; std::endl;
        strResult[nLargeLength] = (nTemp%10)+&#39;0&#39;;
        cout &lt;&lt; &quot;nLargeLength:&quot; &lt;&lt; nLargeLength &lt;&lt; std::endl;
        //如果超过了10则应进1
        if( nTemp &gt;= 10 )
        {
            strResult[nLargeLength-1] = 1+&#39;0&#39;;
        }
        nLargeLength--;
    }
    return strResult;
}</pre>

&nbsp;