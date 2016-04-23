---
id: 171
title: When Machine Learning Meets Amazon Web Services (AWS)
date: 2014-01-09T23:48:09+00:00
author: lo
layout: post
guid: /?p=171
permalink: /when-machine-learning-meets-amazon-web-services-aws/
categories:
  - Kaggle
  - Machine Learning
tags:
  - Advice
  - AWS
  - Kaggle
  - Machine Learning
---
## Summary

Use [AWS Elastic Cloud](http://aws.amazon.com/ec2/) for machine learning to:

  * Decrease machine learning computing time, so you can try more new stuff faster.
  * Use specialized toolset requiring GPUs, like CUDA-CONVNET, which your old computer may not have.
  * Avoid hurting your computer health because you keep your computer busy for hours repeatedly.
  * You / your company got some money to spend. =]

## Long Version

<span style="line-height: 1.714285714; font-size: 1rem;">You might think your 16GB of RAM and 8 CPU cores notebook is good enough to run your machine learning algorithms. However, if you really did get hands-on, you most likely have encountered these major problems:</span>

  * **Slow.** Searching for the right parameters with multiple degrees of freedom takes a long time, even if each takes a minute or two. e.g. grid search the best C and gamma for SVMs.
  * **Low memory.** Your dataset expands after preprocessing or feature engineering, suddenly takes 22GB of RAM on your 16GB notebook. Suddenly, your algorithm slows down drastically.

But, really, the problems that really hurts are:

  * **Hampers creativity.** _You don&#8217;t know why it works, but it works anyway._ Sometimes results are discovered through sheer try-and-error, and being able to explore without waiting too long (hours!) helps a lot. It is not fun to wait for 5 minutes per iteration on a 150 fit.
  * **Durability.** Topping your CPU to 100% for dozen hours repeatedly shorten your computer&#8217;s life really easily, e.g. getting busy a lot clogs your CPU fan with dust.
  * **Limited toolsets.** For example, if you want to run the CUDA-CONVNET, you will need to have Fermi-generation GPUs, which you might not have it. It&#8217;s one of the more powerful deep learning tools out there now. The GPU instances on AWS are Fermi-generation GPU instances.

<span style="line-height: 1.714285714; font-size: 1rem;">Of course, these are just those that come out the top of my head. I use the Elastic Cloud to get a very powerful instance which maybe I only need for a few hours. This is especially useful for </span><span style="line-height: 1.714285714; font-size: 1rem;">Kaggle competitions, which you will have a big dataset and a lot of ideas to try. It may be costly in the long run, but you won&#8217;t need to buy a new computer for it&#8230; and then decide you don&#8217;t need it.</span>