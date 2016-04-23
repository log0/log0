---
id: 321
title: 'Tea time with: Rapid Object Detection using a Boosted Cascade of Simple Features'
date: 2014-03-07T21:52:53+00:00
author: lo
layout: post
guid: /?p=321
permalink: /tea-time-with-rapid-object-detection-using-a-boosted-cascade-of-simple-features/
categories:
  - Machine Learning
  - Summaries
tags:
  - Computer Vision
  - Paper
  - Summary
---
[<img class="aligncenter size-medium wp-image-326" alt="Cascade Classifier" src="/wp-content/uploads/2014/03/cascade.png" width="580" height="343" sizes="(max-width: 580px) 100vw, 580px" />](/wp-content/uploads/2014/03/cascade.png)

Available [here](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf). The [Cascade Classifier](http://en.wikipedia.org/wiki/Cascading_classifiers) is one of the most popular face detection algorithms and the default choice in OpenCV libraries as well. Some notes after reading:

  * One of the most influential papers in computer vision research.
  * Introduced an approach of face detection that is highly accurate and very fast at that time.
  * Fast enough for face detection in real time video feeds.
  * 15 times faster than any of the previous work at the time.
  * **Intuition: Accurate and complex models are computational expensive. The input are subjected to a series of increasing accurate and expensive models, and the most expensive model will be used on only the most promising input.** So it asks a series of questions like: 1) Does model one thinks it is a face? If no, stop. If Yes, ask model two. 2) Does model two thinks it is a face? If no, stop. If yes, ask model three. And so on, until the last model also say yes.
  * **Feature-based than pixel-based.**  Haar-like features were created for the images using a fast algorithm enabled by the use of Integral Images. Note that working with pixels are generally very computational expensive.
  * **[Haar-like features](http://en.wikipedia.org/wiki/Haar-like_features) are basically high-level features over pixels.** For example, the feature observation that the region of the eyes is darker than the region of the cheeks. This can be used as a feature input to the model.

Similar ideas can definitely be applied to areas even outside of machine learning, where you want to use the simple approaches first before going deep into the big guns (more complex model). Only use it on areas where the returns are promising. In reality, you will want to do the same occasionally too: Only apply the more expensive solutions if the simpler ones do not work.