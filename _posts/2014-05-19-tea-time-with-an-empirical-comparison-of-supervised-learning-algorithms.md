---
id: 416
title: 'Tea Time With: An Empirical Comparison of Supervised Learning Algorithms'
date: 2014-05-19T08:12:21+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=416
permalink: /tea-time-with-an-empirical-comparison-of-supervised-learning-algorithms/
categories:
  - Machine Learning
  - Summaries
tags:
  - Machine Learning
  - Paper
  - Summary
---
# Goals

Find out which learning algorithm is better or worse than the others in a systematic manner.

Paper can be downloaded [here](http://www.cs.cornell.edu/~caruana/ctp/ct.papers/caruana.icml06.pdf).

# Key Points

[<img class="aligncenter size-full wp-image-417" src="http://www.chioka.in/wp-content/uploads/2014/05/model-scores.png" alt="model scores" width="825" height="597" srcset="http://ckieric.webfactional.com/wp-content/uploads/2014/05/model-scores.png 825w, http://ckieric.webfactional.com/wp-content/uploads/2014/05/model-scores-580x419.png 580w, http://ckieric.webfactional.com/wp-content/uploads/2014/05/model-scores-624x451.png 624w" sizes="(max-width: 825px) 100vw, 825px" />](http://www.chioka.in/wp-content/uploads/2014/05/model-scores.png)

<div>
  <div style="color: #000000;">
    <ul>
      <li>
        The top models are: Boosted decision trees, random forests, bagged decision trees, SVM, neural networks.
      </li>
      <li>
        The worst models are: Naive Bayes, Logistic Regression, Decision Trees.
      </li>
      <li>
        Even the best model can perform poorly on some datasets. Note that the results present means the algorithm ranked top performs in general the best, albeit it performs poorly in some datasets.
      </li>
      <li>
        If a model selection is done by looking into the test set (data snooping), the final model performance increases, which is expected. However, models with the high variance increases the most (like ANN, SVM, but not boosted or bagged models, which decreases variance but increases bias), because validation sets does not always select the model with the best performance on the final test set.
      </li>
      <li>
        Platt Scaling and Isotonic Regression are techniques to scale a classification of a model to posterior probabilities (i.e. [0,1] scale).
      </li>
    </ul>
    
    <h1>
      About the Experiment
    </h1>
    
    <div>
      <ul>
        <li>
          Run multiple learning algorithms against 11 problems each with a 5-fold cross-validation for model selection.
        </li>
        <li>
          All learning algorithms: Support Vector Machines (SVMs), Artificial Neural Network (ANN), Logistic Regression (LOGREG), Naive Bayes (NB), K Nearest Neighbours (KNN), Random Forests (RF), Decision Trees (DT), Bagged Trees (BAG-DT), Boosted Trees (BST-DT), Boosted Stumps (BST-STMP).
        </li>
        <li>
          Training size is 5000, and the rest of the data is always testing set.
        </li>
        <li>
          A total of 8 metrics, including accuracy, area under ROC curve, etc.
        </li>
        <li>
          Scores are calibrated via Platt Scaling or Isotonic Regression for models that give binary output.
        </li>
      </ul>
    </div>
  </div>
</div>