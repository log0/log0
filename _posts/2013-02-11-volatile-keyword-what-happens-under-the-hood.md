---
id: 14
title: 'Volatile Keyword : What Happens Under The Hood'
date: 2013-02-11T09:36:39+00:00
author: lo
layout: post
guid: http://notes.chioka.in/?p=14
permalink: /volatile-keyword-what-happens-under-the-hood/
categories:
  - C++
tags:
  - C++
---
#### How does the volatile variable work behind the scenes?

Google volatile variable and you know that without the volatile keyword the compiler will cache results and cause programs incorrectness. But, why does it get wrong? Ever checked the assembly code?

First, the definition of volatile according to [a great article](http://www.drdobbs.com/cpp/volatile-the-multithreaded-programmers-b/184403766) on Dr.Dobbs by Andrei Alexandrescu has written about, very well:

> The `volatile` keyword was devised to prevent compiler optimizations that might render code incorrect in the presence of certain asynchronous events.

So what effect does volatile have on a compiler?

Concretely, let&#8217;s check out two examples with the help of constants (const). We know that constants are, well, constants. So it is impossible to modify the value, except in C++. So take these programs A and B which tries to modify a constant but one with a volatile keyword.

<pre>// Program A
#include &lt;cstdio&gt;</pre>

<pre>int main(){
    const volatile int i = 50;
    int * pi = const_cast&lt;int *&gt;(&i);
    printf("%d\n", i);
    *pi = 20;
    printf("%d\n", i);
}

<span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px;"> And this program B:</span></pre>

<pre>// Program B
#include &lt;cstdio&gt;</pre>

<pre>int main(){
    const int i = 50;
    int * pi = const_cast&lt;int *&gt;(&i);
    printf("%d\n", i);
    *pi = 20;
    printf("%d\n", i);
}</pre>

Output of Program A:

<pre>50
20</pre>

Output of Program B:

<pre>50
50</pre>

OK. These are facts, why? What happened? Let&#8217;s do some debugging via gdb. Compile the programs with symbols ( g++ -o <output> <source> -ggdb ):

Program A gdb output:

<pre>main () at const_cast.cpp:4
4 const volatile int i = 50;
(gdb) s
5 int * pi = const_cast&lt;int *&gt;(&i);
(gdb) p i
$1 = 50
(gdb) s
6 printf("%d\n", i);
(gdb) s
50
7 *pi = 20;
(gdb) p *pi
$2 = 50
(gdb) s
8 printf("%d\n", i);
(gdb) p *pi
$3 = 20
(gdb) s
20
9 }</pre>

Program B gdb output:

<pre>main () at const_cast2.cpp:44 const int i = 50;
 (gdb) s
 5 int * pi = const_cast&lt;int *&gt;(&i);
 (gdb) s
 6 printf("%d\n", i);
 (gdb) p i
 $1 = 50
 (gdb) p *pi
 $2 = 50
 (gdb) s
 50
 7 *pi = 20;
 (gdb) s
 8 printf("%d\n", i);
 (gdb) p i
 $3 = 20
 (gdb) p * pi
 $4 = 20
 (gdb) p
 $5 = 20
 (gdb) s
 50
 9 }</pre>

The gdb output of program B mysteriously prints the \*pi as 50 and not 20, even though we have checked that \*pi is really 20. Why? Now, we need some truth serums &#8211; assembly code.

Let&#8217;s get some assembly code ( g++ -g -c -Wa,-alh <source> ):

Assembly of program A, take note at bolded parts.

<pre>6:const_cast.cpp **** *pi = 20;
 42 .loc 1 7 0
 43 002d 8B442418 movl 24(%esp), %eax
 44 0031 C7001400 movl $20, (%eax)
 44 0000
 7:const_cast.cpp **** printf("%d\n", i);
 45 .loc 1 8 0
 46 0037 8B44241C movl <strong>28(%esp)</strong>, %eax
 47 003b 89442404 movl %eax, 4(%esp)
 48 003f C7042400 movl $.LC0, (%esp)
 48 000000
 49 0046 E8FCFFFF call printf</pre>

Assembly of program B, take note at bolded parts.

<pre>6:const_cast2.cpp **** *pi = 20;
 41 .loc 1 7 0
 42 002d 8B442418 movl 24(%esp), %eax
 43 0031 C7001400 movl $20, (%eax)
 43 0000
 7:const_cast2.cpp **** printf("%d\n", i);
 44 .loc 1 8 0
 45 0037 C7442404 movl <strong>$50</strong>, 4(%esp)
 45 32000000
 46 003f C7042400 movl $.LC0, (%esp)
 46 000000
 47 0046 E8FCFFFF call printf</pre>

Note in Program B, line 45 indicates that the compiler just push the value &#8220;50&#8221; into the second argument of printf, rather than in Program Line 46 it pushes the value of **i** from the stack.

It should be obvious now : Program B, without the volatile keyword on , has led the compiler to believe the constant variable **i** does not change. As thus, the compiler optimizes and assumes value pointed by pi (*pi) is 50. However, **the compiler is wrong**. By adding the volatile keyword, we tell the compiler not to be too smart to get things wrong.