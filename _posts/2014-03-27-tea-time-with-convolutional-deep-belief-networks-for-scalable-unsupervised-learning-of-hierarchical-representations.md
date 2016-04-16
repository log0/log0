---
id: 365
title: 'Tea Time With: Convolutional Deep Belief Networks for Scalable Unsupervised Learning of Hierarchical Representations'
date: 2014-03-27T09:38:31+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=365
permalink: /tea-time-with-convolutional-deep-belief-networks-for-scalable-unsupervised-learning-of-hierarchical-representations/
categories:
  - Machine Learning
  - Summaries
tags:
  - Deep Learning
  - Machine Learning
  - Paper
  - Summary
---
# <span style="font-size: 1.5rem; line-height: 1.5;">Goals</span>

The deep learning algorithms at that time do not scale to high dimensional input. This <a href="http://web.eecs.umich.edu/~honglak/icml09-ConvolutionalDeepBeliefNetworks.pdf" target="_blank">paper</a> tries to address this problem.

# Key Points

Reasons why deep learning algorithms do not scale:

  * At the time of the paper, [Restricted Boltzmann Machine](http://en.wikipedia.org/wiki/Restricted_Boltzmann_machine) chokes at 30 x 30 pixels input, much lower than realistic requirements. Convolutional Deep Belief Network (CDBN) tries to scale this up to 200 x 200 pixels.
  * RBM choke because the same feature detector learned at a location cannot be used on another location (i.e. not translation invariant).

How convoluted deep belief network works on a high level:

  * Deep learning stresses on having every next layer learns a higher feature representation of the lower layer&#8217;s features. For example, think of recognizing digits as three parts: 1) Detect lines from raw pixels, 2) Detect corners, contours based on lines, 3) Detect unique digits features based on corners, contours.

[<img class="aligncenter size-full wp-image-373" alt="layered-features" src="http://www.chioka.in/wp-content/uploads/2014/03/layered-features.png" width="516" height="431" />](http://www.chioka.in/wp-content/uploads/2014/03/layered-features.png)

  * <span style="line-height: 1.714285714; font-size: 1rem;">Introduces the Convolutional Deep Belief Network (CDBN), which is just stacking layers of Convolutional Restricted Boltzmann Machine (CRBM) over each other. Each CRBM acts as a feature detector. The stacking for a CRBM looks like below:</span>

[<img class="aligncenter size-full wp-image-372" alt="convoluted-rbm" src="http://www.chioka.in/wp-content/uploads/2014/03/convoluted-rbm.png" width="496" height="389" />](http://www.chioka.in/wp-content/uploads/2014/03/convoluted-rbm.png)

How does CDBN solves the scaling problem:

  * A traditional Restricted Boltzmann Machine (RBM) has no pooling layer. The CRBM, which makes up the CDBN, has this extra pooling layer on top of hidden layer which shrinks the hidden layer details before feeding it to the next layer in the CDBN. Shrinking is to drop the details learnt by a factor or 2 or 3. This effectively drops some noises which allows small translations to happen, reducing computational burden as well.
  * In DBNs, when a given feature is learnt (i.e. the weights are learnt), it is not shared across other locations of the image. This redundancy means extra computational work. CDBNs have weights that are shared among all locations in an image.

# Findings

  * [<img class="aligncenter size-full wp-image-375" alt="features-learned" src="http://www.chioka.in/wp-content/uploads/2014/03/features-learned.png" width="1014" height="334" srcset="http://ckieric.webfactional.com/wp-content/uploads/2014/03/features-learned.png 1014w, http://ckieric.webfactional.com/wp-content/uploads/2014/03/features-learned-580x191.png 580w, http://ckieric.webfactional.com/wp-content/uploads/2014/03/features-learned-940x309.png 940w, http://ckieric.webfactional.com/wp-content/uploads/2014/03/features-learned-624x205.png 624w" sizes="(max-width: 1014px) 100vw, 1014px" />](http://www.chioka.in/wp-content/uploads/2014/03/features-learned.png)Above is the features learnt by the CDBN visualized. The upper row is what the 2nd layer of the CDBN &#8216;sees&#8217;, and the lower row is what the 3rd layer of the CDBN &#8216;sees&#8217;. It is pretty close to what a human would expect as well.
  * One interesting result is that the CDBN trained on the Kyoto natural image dataset, when tested on the Caltect 101 dataset, yielded a result of ~65.4%, on par with state of the art. Meaning that the CDBN could have built some real good generalization, and not just the things it was trained to recognize.

The <a href="http://web.eecs.umich.edu/~honglak/icml09-ConvolutionalDeepBeliefNetworks.pdf" target="_blank">paper</a> contains much more insight which readers are encouraged to read the paper and <a href="http://arnetminer.org/publication/convolutional-deep-belief-networks-for-scalable-unsupervised-learning-of-hierarchical-representations-1210248.html" target="_blank">presentation</a>.