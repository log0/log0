---
title: What is "Embedding" in Deep Learning?
date: 2016-10-03T00:05:19+00:00
author: lo
layout: post
permalink: /what-is-embedding-in-deep-learning
nkweb_code_in_head:
  - default
nkweb_Use_Custom_js:
  - default
nkweb_Custom_js:
  - 
nkweb_Use_Custom_Values:
  - default
nkweb_Custom_Values:
  - 
nkweb_Use_Custom:
  - 'false'
nkweb_Custom_Code:
  - 
categories:
  - Machine Learning
tags:
  - Machine Learning
  - Deep Learning
  - Tutorial
---
An *embedding* is a way to represent sparse high dimensional vectors in a compact low dimensional vector to make it easier for a machine learning model to learn.

Machine learning models expects numeric inputs. Neural networks are no exceptions. If the input is a piece of text, then each word needs to be converted to numerics before passing to the neural network as input. One way to represent a word is using a "one hot" vector. A "one hot" vector is usually a sparse vector with only one non-zero entry. The non-zero entry represents the word.

For instance, if a vocabulary has only three words: "Lettuce", "Boat" and "Spinach". Then to represent the word "Lettuce", the vector is [1 0 0]^T. For "Boat", the vector is [0 1 0]^T. For "Spinach", it will be [0 0 1]^T.

[TODO: insert image to visualize one hot vector of size 3]

<br/>

### One Hot Vectors Seems to Do the Job Fine. What is the Problem Here?

There are a couple of problems:

1. **Large weight matrices results in more training data needed, increased memory requirements and longer training time.**
2. **One Hot Vector does not give meaning between words. The index of the words in the vector carries no meaning.** "Lettuce" and "Boat" is one distance untis apart, but "Lettuce" and "Spinach" are two distance units apart. Shouldn't "Lettuce" and "Spinach" be closer since they are much closely related?

We will explain more in the next section.

<br/>

### Large Weight Matrices

If the vocabulary is very large (e.g. a one hot vector of size 200000), and the connecting neural network layer has 500 neurons, there will be a total of (200000 x 500 = 100000000) weights. This requires 0.37G of memory to represent the weight matrix in memory.

[TODO: insert image to visualize one hot vector connecting to neural network layer with weight connections shown, highlighting the weight matrix.]

Having large weight matrices introduce these issues:

- **Need more training data.** More data is needed for the model to learn the weights. If the training data is expensive to get, your model may not learn properly.
- **Unable to run model or reduced batch size.** Since more memory is needed to represent the weight matrix, if the GPU does not have enough memory, the model may not run or at least your batch size is significantly reduced. The reduced batch size will result in longer training time.
- **Longer training time.** More computational cost as more mathematical operations are needed for a larger weight matrix.

To mitigate these issues, the straight forward way is to reduce the size of the weight matrix, we can reduce the vocabulary size or number of neurons in the neural network layer, but can we do better?

<br/>

### Lack of Information Between Words In One Hot Vector

The other problem is that the word order in a one hot vector does not carry any meaning other than some word appeared earlier during construction of the vocabulary. So the fact that "Lettuce" and "Boat" is closer in a one hot vector does not mean "Lettuce" and "Boat" is closer than to "Lettuce" and "Spinach" in meaning. In fact, there is no meaning conveyed. This information can be used to allow machine learning models to learn better.

<br/>

### How does embedding help?

What if we can reduce the one hot vector from size 200000 to size 500? There will be a total of (500 x 500 = 250000) weights. It has 400 times fewer weights. Embedding does exactly this.

[TODO: Explain the reduced dimensionality.]

[TODO: Explain the W(A) - (B) ~= W(C) - W(D) concept. for problem 2]

[TODO: Alternatives]

[TODO: How to use embedding in machine learning?]

[TODO: Talking about reusable.]

[TODO: Talk about how to train W(W)]

[TODO: Code?]
