---
title: 'Key Concept: Polymorphism in C++'
id: 201
categories:
  - C++
date: 2007-09-05 18:22:00
tags:
---

    <TABLE style="TABLE-LAYOUT: fixed">
<TBODY>
<TR>
<TD>
<DIV class=cnt id=blog_text>

<FONT size=3>The crucial point about references and pointers to base-class types is that the **<A name=ch15term21></A>[<U><FONT color=#0000ff>static type</FONT></U>](mk:@MSITStore:E:%20学习资料%20C++学习资料%20C++Primer4E.chm::/0201721481/ch15lev1sec11.html#gloss15_21)**the type of the reference or pointer, which is knowable at compile timeand the **<A name=ch15term7></A>[<U><FONT color=#0000ff>dynamic type</FONT></U>](mk:@MSITStore:E:%20学习资料%20C++学习资料%20C++Primer4E.chm::/0201721481/ch15lev1sec11.html#gloss15_07)**the type of the object to which the pointer or reference is bound, which is knowable only at run timemay differ.</FONT>

<A name=idd1e114290></A><A name=idd1e114297></A><A name=idd1e114302></A><A name=idd1e114307></A><A name=idd1e114315></A><A name=idd1e114320></A><A name=idd1e114325></A><A name=idd1e114328></A><A name=idd1e114333></A><A name=idd1e114338></A><A name=idd1e114343></A><A name=idd1e114348></A><SPAN class=docEmphStrong><FONT size=3>The fact that the static and dynamic types of references and pointers can differ is the cornerstone of how C++ supports polymorphism.</FONT></SPAN>

<SPAN class=docEmphStrong><FONT size=3>When we call a function defined in the base class through a base-class reference or pointer, we do not know the precise type of the object on which the function is executed. The object on which the function executes might be of the base type or it might be an object of a derived type.</FONT></SPAN>

<SPAN class=docEmphStrong><FONT size=3>If the function called is nonvirtual, then regardless of the actual object type, the function that is executed is the one defined by the base type. If the function is virtual, then the decision as to which function to run is delayed until run time. The version of the virtual function that is run is the one defined by the type of the object to which the reference is bound or to which the pointer points.</FONT></SPAN>

<SPAN class=docEmphStrong><FONT size=3>From the perspective of the code that we write, we need not care. As long as the classes are designed and implemented correctly, the operations will do the right thing whether the actual object is of base or derived type.</FONT></SPAN>

<SPAN class=docEmphStrong><FONT size=3>On the other hand, an object is not polymorphicits type is known and unchanging. The dynamic type of an object (as opposed to a reference or pointer) is always the same as the static type of the object. The function that is run, virtual or nonvirtual, is the one defined by the type of the object.</FONT></SPAN>
<A name=ch15note07></A>
<DIV class=docNote>

<TABLE class=FCK__ShowTableBorders cellSpacing=0 cellPadding=1 width="90%" border=0>
<TBODY>
<TR>
<TD vAlign=top width=60>![](mk:@MSITStore:E:%20学习资料%20C++学习资料%20C++Primer4E.chm::/0201721481/images/0201721481/graphics/note.jpg;400478)</TD>
<TD vAlign=top>
<P class=docText><SPAN class=docEmphStrong>Virtuals are resolved at run time <SPAN class=docEmphasis>only</SPAN> if the call is made through a reference or pointer. Only in these cases is it possible for an object's dynamic type to be unknown until run time.</SPAN>
</TD></TR></TBODY></TABLE></P></DIV></DIV></TD></TR></TBODY></TABLE>
</div>