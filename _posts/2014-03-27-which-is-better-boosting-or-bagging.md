---
id: 382
title: 'Which is Better : Boosting or Bagging'
date: 2014-03-27T20:04:57+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=382
permalink: /which-is-better-boosting-or-bagging/
categories:
  - Machine Learning
tags:
  - Ensemble methods
  - Machine Learning
  - Paper
  - Python
---
A question we sometimes encounter during applying machine learning is : Should we use [bagging](http://en.wikipedia.org/wiki/Bootstrap_aggregating), [boosting](http://en.wikipedia.org/wiki/Boosting_(machine_learning)) or even just plain classifier?

Both bagging and boosting falls into an umbrella technique called [ensemble learning](http://en.wikipedia.org/wiki/Ensemble_learning).

Bagging is to have multiple classifiers trained on different under-sampled subsets and allow these classifiers to vote on a final decision, contrasting with just using one classifier.

Boosting is to have a series of classifiers to train on the dataset, but gradually putting more emphasis on training examples that the previous classifiers have failed (i.e. harder for classifiers to get it right), in the hope of that the next classifier will focus on these harder examples. So in the end, you will have a series of classifiers who are in general balanced but slightly more focused on the hard training examples.

**In practice, boosting beats bagging in general, but either bagging and boosting will beat a plain classifier. **This is shown in the paper [Bagging, Boosting and C4.5](http://home.eng.iastate.edu/~julied/classes/ee547/Handouts/q.aaai96.pdf) where the author makes comparisons between bagging, boosting and C4.5 over two dozens of datasets, and shows that boosting performs better in most cases.

If you&#8217;re like me who wants something out-of-the-box first, for bagging, Scikit-Learn&#8217;s bleeding edge has this [BaggingClassifier](http://scikit-learn.org/dev/modules/generated/sklearn.ensemble.BaggingClassifier.html) which you can try, or can be implemented by yourself fairly easily. For boosting, try two algorithms on a dataset using any algorithm you like (non-bagged non-boosted) and using [GradientBoostingClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingClassifier.html), in general I found the GB is quite slow but does better in general.