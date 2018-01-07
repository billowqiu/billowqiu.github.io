---
title: 主备切换之State模式实现
id: 103
categories:
  - 设计模式
date: 2011-04-30 00:53:00
tags:
---

最近项目中需要实现一个主备切换的功能，通过分析可以得出也就两个状态的切换，使用if/switch之类的语句可以轻松搞定，但是为了学习并实践State模式，这里采取了一个State模式的实现：
<pre class="brush:cpp">// HAState.h: interface for the HAState class.
//
//////////////////////////////////////////////////////////////////////
#if !defined(AFX_HASTATE_H__410262B3_3FEB_44B3_BFA7_04C4BEBCE636__INCLUDED_)
#define AFX_HASTATE_H__410262B3_3FEB_44B3_BFA7_04C4BEBCE636__INCLUDED_
#if _MSC_VER &gt; 1000
#pragma once
#endif // _MSC_VER &gt; 1000
#include &lt;iostream&gt;
class HASwitch;
enum HASTATE
{
    STANDBY,
    PRIMARY,
    FAILURE
};
class HAState
{
public:
    HAState();
    virtual ~HAState();

    virtual HASTATE getState();
    virtual bool standby2primary( HASwitch* pSwitch ) = 0;
    virtual bool primary2standby( HASwitch* pSwitch ) = 0;

protected:
    void changeState( HASwitch* pSwitch, HAState* pNewState );
protected:
    HASTATE m_euState;
};
#endif // !defined(AFX_HASTATE_H__410262B3_3FEB_44B3_BFA7_04C4BEBCE636__INCLUDED_)
// HAState.cpp: implementation of the HAState class.
//
//////////////////////////////////////////////////////////////////////
#include "HAState.h"
#include "HASwitch.h"
//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////
HAState::HAState():m_euState( FAILURE )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
HAState::~HAState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
void HAState::changeState( HASwitch* pSwitch, HAState* pNewState )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    pSwitch-&gt;changeState( pNewState );
}
HASTATE HAState::getState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return m_euState;
}</pre>
&nbsp;
<pre class="brush:cpp">// PrimaryState.h: interface for the PrimaryState class.
//
//////////////////////////////////////////////////////////////////////
#if !defined(AFX_PRIMARYSTATE_H__DFA509B1_9344_4750_A582_5637A65DF2A8__INCLUDED_)
#define AFX_PRIMARYSTATE_H__DFA509B1_9344_4750_A582_5637A65DF2A8__INCLUDED_
#if _MSC_VER &gt; 1000
#pragma once
#endif // _MSC_VER &gt; 1000
#include &lt;boost/noncopyable.hpp&gt;
#include "HAState.h"
class PrimaryState : public HAState, public boost::noncopyable
{
public:
    PrimaryState();
    virtual ~PrimaryState();
    static PrimaryState&amp; instance();
    virtual bool standby2primary( HASwitch* pSwitch ) ;
    virtual bool primary2standby( HASwitch* pSwitch ) ;
private:
    static PrimaryState s_Instance;
};
#endif // !defined(AFX_PRIMARYSTATE_H__DFA509B1_9344_4750_A582_5637A65DF2A8__INCLUDED_)</pre>
&nbsp;
<pre class="brush:cpp">// PrimaryState.cpp: implementation of the PrimaryState class.
//
//////////////////////////////////////////////////////////////////////
#include "PrimaryState.h"
#include "StandbyState.h"
#include "HASwitch.h"
//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////
PrimaryState PrimaryState::s_Instance;
PrimaryState::PrimaryState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    m_euState = PRIMARY;
}
PrimaryState::~PrimaryState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
bool PrimaryState::standby2primary( HASwitch* pSwitch )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    std::cout &lt;&lt; "Current state is already primary" &lt;&lt; std::endl;

    return true;
}
bool PrimaryState::primary2standby( HASwitch* pSwitch )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    std::cout &lt;&lt; "Begin switch to standby..." &lt;&lt; std::endl;

    std::cout &lt;&lt; "Do something" &lt;&lt; std::endl;
    changeState( pSwitch, &amp;StandbyState::instance() );

    std::cout &lt;&lt; "End switch to standby..." &lt;&lt; std::endl;

    return true;
}
PrimaryState&amp; PrimaryState::instance()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return s_Instance;
}
// StandbyState.h: interface for the StandbyState class.
//
//////////////////////////////////////////////////////////////////////
#if !defined(AFX_STANDBYSTATE_H__028C47F8_1A92_4854_A93D_2AECB6A17DF6__INCLUDED_)
#define AFX_STANDBYSTATE_H__028C47F8_1A92_4854_A93D_2AECB6A17DF6__INCLUDED_
#if _MSC_VER &gt; 1000
#pragma once
#endif // _MSC_VER &gt; 1000
#include &lt;boost/noncopyable.hpp&gt;
#include "HAState.h"
class StandbyState : public HAState, public boost::noncopyable
{
public:
    StandbyState();
    virtual ~StandbyState();
    static StandbyState&amp; instance();
    virtual bool standby2primary( HASwitch* pSwitch ) ;
    virtual bool primary2standby( HASwitch* pSwitch ) ;
private:
    static StandbyState s_Instance;
};
#endif // !defined(AFX_STANDBYSTATE_H__028C47F8_1A92_4854_A93D_2AECB6A17DF6__INCLUDED_)
// StandbyState.cpp: implementation of the StandbyState class.
//
//////////////////////////////////////////////////////////////////////
#include "StandbyState.h"
#include "PrimaryState.h"
#include "HASwitch.h"
//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////
StandbyState StandbyState::s_Instance;
StandbyState::StandbyState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    m_euState = STANDBY;
}
StandbyState::~StandbyState()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
bool StandbyState::standby2primary( HASwitch* pSwitch )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    std::cout &lt;&lt; "Begin switch to primary..." &lt;&lt; std::endl;

    std::cout &lt;&lt; "Do something" &lt;&lt; std::endl;
    changeState( pSwitch, &amp;PrimaryState::instance() );

    std::cout &lt;&lt; "End switch to primary..." &lt;&lt; std::endl;

    return true;
}
bool StandbyState::primary2standby( HASwitch* pSwitch )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    std::cout &lt;&lt; "Current state is already standby" &lt;&lt; std::endl;

    return true;
}
StandbyState&amp; StandbyState::instance()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return s_Instance;
}
// HASwitch.h: interface for the HASwitch class.
//
//////////////////////////////////////////////////////////////////////
#if !defined(AFX_HASWITCH_H__AC109E28_51E4_46AE_BC07_9E93FA747B65__INCLUDED_)
#define AFX_HASWITCH_H__AC109E28_51E4_46AE_BC07_9E93FA747B65__INCLUDED_
#if _MSC_VER &gt; 1000
#pragma once
#endif // _MSC_VER &gt; 1000
#include &lt;boost/noncopyable.hpp&gt;
#include "HAState.h"
class HASwitch : public boost::noncopyable
{
    friend class HAState;
public:
    HASwitch();
    virtual ~HASwitch();
    static HASwitch&amp; instance();
    HASTATE getState() const;
    boolstandby2primary();
    boolprimary2standby();
private:
    void changeState( HAState* pNewState );
private:
    static HASwitch g_Instance;
    HAState* m_pCurState;
};
#endif // !defined(AFX_HASWITCH_H__AC109E28_51E4_46AE_BC07_9E93FA747B65__INCLUDED_)</pre>
&nbsp;
<pre class="brush:cpp">// HASwitch.cpp: implementation of the HASwitch class.
//
//////////////////////////////////////////////////////////////////////
#include "HASwitch.h"
#include "HAState.h"
#include "PrimaryState.h"
#include "StandbyState.h"
//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////
HASwitch HASwitch::g_Instance;
HASwitch::HASwitch()
{
    m_pCurState = &amp;StandbyState::instance();
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
HASwitch::~HASwitch()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
}
HASTATE HASwitch::getState() const
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return m_pCurState-&gt;getState();
}
bool HASwitch::standby2primary()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return m_pCurState-&gt;standby2primary(this);
}
bool HASwitch::primary2standby()
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    return m_pCurState-&gt;primary2standby(this);
}
void HASwitch::changeState( HAState* pNewState )
{
    std::cout &lt;&lt; "Call " &lt;&lt; __FUNCTION__ &lt;&lt; std::endl;
    m_pCurState = pNewState;
}
HASwitch&amp; HASwitch::instance()
{
    return g_Instance;
}</pre>
测试代码：
<pre class="brush:cpp">#include &lt;iostream&gt;
#include &lt;assert.h&gt;
#include "HASwitch.h"
int main(int argc , char ** argv)
{
    assert( HASwitch::instance().getState() == STANDBY );
    HASwitch::instance().standby2primary();
    assert( HASwitch::instance().getState() == PRIMARY );
    HASwitch::instance().primary2standby();
    assert( HASwitch::instance().getState() == STANDBY );
    return 0;
}</pre>
&nbsp;