---
id: 517
title: Visualizing the Differences In L1-norm and L2-norm Loss Function
date: 2014-12-01T11:35:21+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=517
permalink: /visualizing-the-differences-in-l1-norm-and-l2-norm-loss-function/
categories:
  - Machine Learning
  - Mathematics
tags:
  - L1-norm
  - L2-norm
  - Loss Functions
  - Machine Learning
  - Python
  - Visualization
---
In an earlier post about the <a href="www.chioka.in/differences-between-l1-and-l2-as-loss-function-and-regularization/" target="_blank">differences between L1 and L2 as loss function and regularization</a>, one of the graph about L1-norm and L2-norm loss function is rather confusing to many readers, as I have seen from the comments. Reviewing it after a year, it wasn&#8217;t very clear as well, so today I generated some data and run a model over them. Here is how it looks like:

[<img class="aligncenter size-full wp-image-515" src="http://www.chioka.in/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization.png" alt="programmatic L1 vs L2 visualization" width="1096" height="716" srcset="http://ckieric.webfactional.com/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization.png 1096w, http://ckieric.webfactional.com/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization-580x378.png 580w, http://ckieric.webfactional.com/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization-940x614.png 940w, http://ckieric.webfactional.com/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization-624x407.png 624w" sizes="(max-width: 1096px) 100vw, 1096px" />](http://www.chioka.in/wp-content/uploads/2013/12/programmatic-L1-vs-L2-visualization.png)

The base model here used is a GradientBoostingRegressor, which can take in L1-norm and L2-norm loss functions. The green and red lines represent a model using L1-norm and L2-norm loss function respectively. A solid line represents the fitted model trained also with the outlier point (orange), and the dotted line represents the fitted model trained without the outlier point (orange).

I gradually move the outlier point from left to right, which it will be less “outlier” in the middle and more “outlier” at the left and right side. When the outlier point is less “outlier” (in the middle), L2-norm has less changes while the fitted line using L1-norm has more changes.

In the case of a more “outlier” point (upper left, lower right, where points are to the far left and far right), both norms still have big change, but again the L1-norm has more changes in general.

By visualizing data, we can get a better idea what stability is with respective to these two loss functions.

The code that generates this plot can be found <a href="https://github.com/log0/l1_and_l2_loss_function" target="_blank">here</a>, and online iPython Notebook link <a href="http://nbviewer.ipython.org/github/log0/l1_and_l2_loss_function/blob/master/Validating%20Stability.ipynb" target="_blank">here</a>.