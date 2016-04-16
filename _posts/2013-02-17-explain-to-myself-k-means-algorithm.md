---
id: 26
title: 'Explain To Myself : K-Means Algorithm'
date: 2013-02-17T14:14:39+00:00
author: lo
layout: post
guid: http://notes.chioka.in/?p=26
permalink: /explain-to-myself-k-means-algorithm/
categories:
  - Machine Learning
tags:
  - Explain To Me
  - Machine Learning
  - Unsupervised Learning
---
This is in Chapter 6 of [Machine Learning](http://www.amazon.com/gp/product/0070428077/ref=as_li_qf_sp_asin_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0070428077&linkCode=as2&tag=notes0a-20) on K-Means Algorithm:

> Step 1: Calculate the expected value E\[z[i\]\[j\]] of each hidden variable z[i], assuming the current hypothesis h = (p1, p2) holds.
  
> Step 2: Calculate a new maximum likelihood hypothesis h&#8217; = (p1, p2), assuming the value taken on by each hidden variable z\[i\]\[j\] is its expected value E\[z[i\]\[j\]] calculated in Step 1. Then replace the hypothesis h = (p1, p2) by the new hypothesis h&#8217; = (p[i], p[i]) and iterate.

I didn&#8217;t understand it on first sight. So this is written for my own understanding.

&nbsp;

**Summary**

Let&#8217;s understand it in a simpler case, for 2 dimensions. The core idea is to randomly initialize K centroids and assign the closest points to these centroids. For the points associate with each centroid, calculate a new mean (which is the center actually). With these new centers, assign the points to these new centroids. Repeat until no changes in the centroid positions. This would result in the points near by the cluster gets grouped together to the centroid, thus grouping the points together somrt kind of similarity.

**Algorithmic Specifics**

For a total of M instances of data, represented as a set of points A as (a[1].x, a[1].y), (a[2].x, a[2].y), &#8230; (a[M].x, a[M].y) .

Randomly initialize a total of K  centroids points C as (c[1].x, c[1].y), (c[2]x, c[2].y), &#8230; (c[K].x, c[K].y) to non-zero values. These are the centroids.

Now:

Repeat {

Step 1: For each point a[i], calculate the distance to each centroid k. Assign a[i] to the closest centroid. &#8220;Closest&#8221; here can be the euclidean distance which is the sqrt (x^2 + y^2).

Step 2: Since all points are assigned to a particular centroid c[i], select all the points from a particular centroid c[i] and calculate the new centroid c'[i] by calculating the mean of these points. This c'[i] will be the new centroid, i.e. c[i] is now assigned the value of c'[i]. Do this for the rest of the centroids.

} Until there is no change in the values of the centroids C.

&nbsp;

&nbsp;