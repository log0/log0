---
id: 42
title: 'Explain To Myself : Ensemble Methods'
date: 2013-06-02T18:58:46+00:00
author: lo
layout: post
guid: http://notes.chioka.in/?p=42
permalink: /explain-to-myself-ensemble-methods/
categories:
  - Machine Learning
tags:
  - Ensemble methods
  - Explain To Me
  - Machine Learning
---
**What are ensemble methods?**

Ensemble methods refer to an machine learning algorithm that is combination of classifiers to tackle a classification problem by using the results from all these classifications to determine a label (usually by a vote).

A vote is a majority based voting mechanism. For instance, of 10 classifiers, 4 voted for label X and 6 voted for label Y, the voting result is label Y has 6 > 4.

**Why is it useful?**

It can be better than using a single classifier in many cases. Suppose there are 4 independent classifiers classifying two labels X and Y, then observe that a data instance with label Y predicted by these classifiers:

<table border="1">
  <tr>
    <th>
      Classifier
    </th>
    
    <th>
      1
    </th>
    
    <th>
      2
    </th>
    
    <th>
      3
    </th>
    
    <th>
      4
    </th>
  </tr>
  
  <tr>
    <td>
      Label
    </td>
    
    <td>
      Y
    </td>
    
    <td>
      X
    </td>
    
    <td>
      Y
    </td>
    
    <td>
      Y
    </td>
  </tr>
</table>

Even though classifier 2 predicted X (which is wrong), through voting, the final result is still Y (3 Y > 1 X).

Thus, the result is wrong only if more than half of the classifiers are wrong.

**When does it fail?**

If the error rate is larger than 0.5 for each classifier, then the classifiers will be correct only if more than half of the classifiers are correct. The net result is that the set of classifiers predict more wrong answers, in this case do not use ensemble methods.

**What is the error rate of the ensemble method, assuming the error rate of each classifier is 0.15?**

It is given by the general formula of:

> error\_rate\_of_ensemble = 0
  
> for i in [floor(n/2)+1, n]:
  
> error\_rate\_of\_ensemble += nCr(i, n) \* error\_rate^i \* (1 &#8211; error_rate)^(n-i)
> 
> where n = number of classifiers,
  
> error_rate = error rate of each classifier (assumed equal here)

In our case, if the error rate of a single classifier is 0.15, the error rate of the ensemble would be only 0.01.

&nbsp;