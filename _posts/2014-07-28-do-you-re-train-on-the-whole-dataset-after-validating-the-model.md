---
id: 479
title: Do You Re-train on the Whole Dataset After Validating the Model?
date: 2014-07-28T09:48:48+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=479
permalink: /do-you-re-train-on-the-whole-dataset-after-validating-the-model/
categories:
  - In Practice
  - Machine Learning
tags:
  - Cross Validation
  - Machine Learning
  - Python
---
Suppose we have a dataset split into 80% for training and 20% for validation, do you do A) or B?

Method A)

  1. Train on 80%
  2. Validate on 20%
  3. Model is good, train on 100%.
  4. Predict test set.

Method B)

  1. Train on 80%
  2. Validate on 20%
  3. Model is good, use this model as is.
  4. Predict test set.

In this post, I&#8217;ve posted <a href="http://www.kaggle.com/forums/t/9831/do-you-re-train-on-the-whole-dataset-after-validating-the-model/" target="_blank">this question on Kaggle</a> and I&#8217;ll summarize the answers here.

For myself, I do A), with the following reasons aggregated:

  * <a href="https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/35179.pdf" target="_blank">More data is better</a>. In case of time time series, including more recent data is always better.
  * Cross validation is used to validate the hyper-parameters to train a model, rather than the model itself. You then pick the best parameters to re-train a model.

References:

  * <a href="http://stats.stackexchange.com/questions/11602/training-with-the-full-dataset-after-cross-validation" target="_blank">http://stats.stackexchange.com/questions/11602/training-with-the-full-dataset-after-cross-validation</a>
  * <a href="http://www.kaggle.com/forums/t/9831/do-you-re-train-on-the-whole-dataset-after-validating-the-model/" target="_blank">http://www.kaggle.com/forums/t/9831/do-you-re-train-on-the-whole-dataset-after-validating-the-model/</a>