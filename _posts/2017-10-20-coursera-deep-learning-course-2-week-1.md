---
layout: post
title:  "Coursera Deep Learning Course 2 Week 1 notes: Practical aspects of Deep Learning"
date: 2017-10-10 16:20:00 +0700
categories: ['machine learning', 'deep learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Setting up your Machine Learning Application**

###### **Train/Dev/Test Sets**

**Applied ML is a highly iterative process**:

* You start with a simple idea.
* You start coding and try it, and get the result.
* Base on the outcome, you may refine the idea... and try to find a better one.

_**How quickly you can go around this cycle?**_

It seems impossible to get the correct value of hyperparameters at the first time.

Data is divided into:
* Training set.
* Hold-out cross validation set, or development set, or dev set.
* Test set.

The workflow: you keep training your algorithm on training set, use dev set to evaluate its performance, then you can take your best model and run it on test set in order to get an unbias estimate of how your algorithm is doing.

Common proportion:
* 70/30: if you don't have dev set.
* 60/20/20.

The gold of dev set is to help you quickly see which model is better, so dev set only need to be large enough, not exactly 20%. Similarly to test set.

So if the data is big:
* 98/1/1
* 99.5/0.4/0.1

**Mismatch train/test distribution**

Training set: cat pictures from internet (may have high quality image).

Dev/test sets: cat pictures from users using your app (may have low quality image).

Rule of thumps: make sure dev and test set come from same distribution.

Not having a test set might be okay. (Only dev set.)

###### **Bias/Variance**

The classify is high bias = The classify is underfitting the data.

The classify is high variance = The classify is overfitting the data.

Recap: 
