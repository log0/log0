---
id: 173
title: 'Explain to Me : What is the Kernel Trick?'
date: 2013-11-07T22:39:20+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=173
permalink: /explain-to-me-what-is-the-kernel-trick/
categories:
  - Machine Learning
tags:
  - Explain To Me
  - Kernel
  - Machine Learning
  - Support Vector Machines
---
The Kernel Trick is a technique in machine learning to avoid some intensive computation in some algorithms, which makes some computation goes from infeasible to feasible.

## So how is it applied?

Concretely, when you see the pattern of **x<sub>i</sub><sup>T</sup>** **x<sub>j</sub>** ( the dot product of **x<sub>i</sub>** transpose and **x<sub>j</sub>** , where **x** is an observation ): **x<sub>i</sub><sup>T</sup> x<sub>j</sub>** can be replaced by K(**x<sub>i</sub>** **x<sub>j</sub>**). Here, K is a kernel, which has many implementations: linear, polynomial, RBF, etc.

## Give me an example.

The most well-known case would be the Support Vector Machines. The problem state of SVM is a constrained optimization problem as follows:

[<img class="size-full wp-image-175 aligncenter" alt="SVM problem statement" src="http://www.chioka.in/wp-content/uploads/2013/11/pic1.png" width="319" height="86" />](http://www.chioka.in/wp-content/uploads/2013/11/pic1.png)

To solve the above efficiently, usually the problem is modeled by as a Lagrange primal problem statement<sup>[1]</sup> (above) but solved by the dual version of problem statement, which becomes the following:

[<img class="size-full wp-image-176 aligncenter" alt="SVM problem statement dual" src="http://www.chioka.in/wp-content/uploads/2013/11/pic2.png" width="381" height="145" />](http://www.chioka.in/wp-content/uploads/2013/11/pic2.png)

Notice the red I boxed above, that is a **very** computationally intensive operation. Suppose **x** is a (10000 x 1) vector, the dot product would introduce 10000 * 10000 = 100000000 multiplications. Suppose you use a linear kernel K(**x<sub>i</sub>, x<sub>j</sub>**) to replace the above red boxed part, there will be only ~10000 multiplications.

**It means it is ~10000 times faster.**

## Conclusion

Machine learning, in practice, will always try to model problems into above or other ways such that the problem can be solved much more efficiently. The Kernel Trick is one of the most important one. The above is an extremely simplified version of what happened only, interested readers should dive deeper into related literature.

&nbsp;

<sup>[1]</sup> &#8211; The primal and dual (together called the duality) are the two sides of the problem, which solving the dual problem statement gives the optimal solution to the primal problem statement (under some constraints). This is grossly simplified.

<h4 style="color: #333333; font-style: normal;">
  Ref:
</h4>

<ul style="color: #333333; font-style: normal; line-height: 24px;">
  <li>
    <a style="font-style: normal;" href="http://www.cs.berkeley.edu/~jordan/courses/294-fall09/lectures/optimization/slides.pdf">Introduction to Convex Optimization by Machine Learning</a>
  </li>
  <li>
    <a style="font-style: normal;" href="http://en.wikipedia.org/wiki/Kernel_trick">The Kernel Trick</a>
  </li>
</ul>

&nbsp;