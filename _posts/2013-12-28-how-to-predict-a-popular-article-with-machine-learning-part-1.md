---
id: 242
title: How to Predict A Popular Article with Machine Learning (Part 1)
date: 2013-12-28T19:16:21+00:00
author: lo
layout: post
guid: /?p=242
permalink: /how-to-predict-a-popular-article-with-machine-learning-part-1/
categories:
  - Machine Learning
tags:
  - Data preprocessing
  - Machine Learning
  - Python
  - Supervised Learning
  - Text Classification
---
This is the first post of a series of posts on applying machine learning to practical business problems. My language of choice will by Python, as our problem will be mainly a text classification problem.

Recently, I was asked to see if we could predict if an article on [Lifehack](http://www.lifehack.org) will become popular or not. If an article gets popular, it will be shared many times. This is a **supervised learning problem.**

**Our task is to build a model to predict if an article on Lifehack will get more than shared over 3000 times.** I will explain why it is 3000 below.

# Data Preparation Process

<span style="line-height: 1.714285714; font-size: 1rem;">Before we could throw our data into the algorithms automagically for results. There are a few things we need to do:</span>

  1. <strong style="line-height: 1.714285714; font-size: 1rem;">Data collection. </strong>Without data, machine learning cannot be applied. I used Scrapy to collect data from Lifehack.
  2. **Data preprocessing.** The data needs to be cleaned of problems such as duplicated posts, invalid share counts, and other problems. We also need to construct the training and test ing set here.
  3. **Data transformation.** The data has to be in a format that is friendly to our machine learning before we can throw it to the algorithm.

## Step 1: Data collection

<span style="line-height: 1.714285714; font-size: 1rem;">I used </span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://www.scrapy.org">Scrapy</a><span style="line-height: 1.714285714; font-size: 1rem;"> to collect the data. It is a wonderful library that makes web scraping a lot easier than you write one yourself handling the exceptional cases. I will not go into the technical details of writing the scraper.</span>

<span style="line-height: 1.714285714; font-size: 1rem;">In summary, we will collect the HTML and the URL of the post. I also collected the title, share count, category, publication date of the article, but this can all be extracted at later stages from the HTML of the article. As follows:</span>

    # items.py
    from scrapy.item import Item, Field
    class PopItem(Item):
        title = Field()
        url = Field()
        shares = Field()
        category = Field()
        raw_content = Field() # the main body of the post, with HTML tags
        text_content = Field() # the main body of the post, stripped of HTML tags
        posted_at = Field()
    

I collected a total of 10,000 articles. But as you will see in the next step, a portion of them is unusable.

## Step 2: Data preprocessing

**You will most likely get wrong results without inspecting your data for problems.** And unless you are doing Kaggle or homework, no one will give you the training and testing set.

### Cleaning potential invalid data

<span style="line-height: 1.714285714; font-size: 1rem;">We will check for <strong>data-duplication</strong>, which there are actually a lot due to the scraper considering URLs like these as equals:</span>

<pre>http://www.lifehack.org/articles/lifehack/fifty-50-tools-which-can-help-you-in-writing.html
http://www.lifehack.org/articles/lifehack/fifty-50-tools-which-can-help-you-in-writing.html?utm_campaign=innerlink&utm_medium=feellikethemediaistrollingyou&utm_source=post</pre>

They should be equal, so we must clean up these duplicate data.

There are many other checks such as **checking for negative or unusually high share count data, etc**. For instance, there are two articles which have like 100 times more than 3rd most shared article, you&#8217;ll want to confirm if it&#8217;s the a problem of the website or our data collection script.

### Assign classes to articles

By inspecting the the raw share counts, we can see that most articles have zero shares or at most hundred of shares. Ideally, an article having more than 10,000 shares seems like a good candidate. However, very few articles match this criteria (less than 300), which you will see will affect us badly due to a classic problem called **[Class Imbalance Problem](/class-imbalance-problem/)**. As such, I will select a lower threshold. All articles with more than 3000 shares are considered positive examples of a popular article, while the rest are considered negative examples.

### Constructing the training and testing sets

**Because we want to benchmark our data consistently, we need to have a consistent testing set.** Also note that we have a very imbalance data set where there are far more negative examples than positive examples.

I allocated 2/3 of the positive examples to the training set, and 1/3 of the positive examples to the testing set. 2000 negative examples are allocated to the testing set, and the rest of the negative examples are put into the training set. Visualized as below:

[<img class="aligncenter size-medium wp-image-300" alt="dataset partition early" src="/wp-content/uploads/2013/12/dataset-partition-early-580x281.png" width="580" height="281" srcset="/wp-content/uploads/2013/12/dataset-partition-early-580x281.png 580w, /wp-content/uploads/2013/12/dataset-partition-early-624x303.png 624w, /wp-content/uploads/2013/12/dataset-partition-early.png 708w" sizes="(max-width: 580px) 100vw, 580px" />](/wp-content/uploads/2013/12/dataset-partition-early.png)

Lastly, note that** it is very important that the training and testing data do not overlap at all, or you will get optimistically biased results. **We will not touch our testing set from now own.

## <span style="font-size: 1.285714286rem; line-height: 1.6;">Step 3: Data transformation</span>

We need to make sure our data is friendly to the input of the model algorithms. Concretely, we need to:

  1. Extract the **raw ****content** of the article body, but removing all those headers, footers, non-article related information.
  2. Extract the **textual content** out of the raw content, which we will strip all the HTML tags away for simplicity, and we can build a TF-IDF matrix later.
  3. Extract the **title** from the article, which we hypothesize to be very important (we get caught by titles first).

There are a lot of other features we can extract but for simplicity, we will just use these features in this post.

In the next post, I will address how we will get the data into the model and get the results out.

&nbsp;

&nbsp;