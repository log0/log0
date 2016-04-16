---
id: 61
title: Why Is Training Set Error Zero In SVM and Random Forest?
date: 2013-08-03T13:14:40+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=61
permalink: /why-is-training-set-error-zero-in-svm-and-random-forest/
categories:
  - Machine Learning
tags:
  - Explain To Me
  - Machine Learning
---
### QUESTIONS

**Q: Why if a model like SVM or Random Forest is trained and evaluated for training error, you will get zero error? (And on the other hand, when a model like a Gaussian Naive Bayes classifier is used instead, the training error is non-zero in general.)**

******A: SVM and decision-tree based variants are instance based classifiers, and thus can fit with the data perfectly. a Guassian Naives Bayes classifier is not.**

### **More explanation**

For the case of SVM, an SVM model is actually a representation of the examples in space where a margin is picked to separate the examples with a gap as wide as possible. For Random Forest (a decision tree based classifier) model, the classifying rules are all derived exactly from the examples and thus has the capability to classify perfectly to the examples given.