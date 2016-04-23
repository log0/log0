---
id: 468
title: 'Python Doesn&#8217;t Use All CPU Cores'
date: 2014-07-19T08:45:36+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=468
permalink: /python-doesnt-use-all-cpu-cores/
categories:
  - Python
tags:
  - Advice
  - Caveats
  - Code
  - Parallelization
  - Python
---
Is your Python script refusing to use all CPU scores on your server, and just using 1 CPU core out of the 16 CPU cores?

It could be that the core affinity is messed up.

As it turns out, some modules mess with the core affinity on import, which end up forcing your Python script to use only one CPU core in the end. Just add this before you run the Python script:

<pre class="lang-py prettyprint prettyprinted" style="color: #000000;"><code>&lt;span class="pln">os&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">system&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str" style="color: #800000;">"taskset -p 0xffffffff %d"&lt;/span>&lt;span class="pun">%&lt;/span>&lt;span class="pln"> os&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">getpid&lt;/span>&lt;span class="pun">())&lt;/span></code></pre>

And then voila! It looks right now!

[<img class="aligncenter size-full wp-image-469" src="http://www.chioka.in/wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o.jpg" alt="10495829_10204248476553916_8213806856117433374_o" width="1154" height="853" srcset="/wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o.jpg 1154w, /wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o-580x428.jpg 580w, /wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o-940x694.jpg 940w, /wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o-624x461.jpg 624w" sizes="(max-width: 1154px) 100vw, 1154px" />](http://www.chioka.in/wp-content/uploads/2014/07/10495829_10204248476553916_8213806856117433374_o.jpg)

You can find more details in this StackOverflow [thread](http://stackoverflow.com/questions/15639779/what-determines-whether-different-python-processes-are-assigned-to-the-same-or-d).