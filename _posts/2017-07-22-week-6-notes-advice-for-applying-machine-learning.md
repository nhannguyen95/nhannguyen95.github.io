---
layout: post
title:  "Week 6 notes: Advice for applying machine learning"
date: 2017-07-22 10:24:00 +0700
categories: machine-learning
tags: notes
no-post-nav: 0
comments: 1
---

##### **Introduction**

##### **Evaluating a Learning Algorithm**

###### **Deciding What to Try Next**


**Debugging a learning algorithm**:

Suppose you have implemented regularized linear regression to predict housing. However, when you test your hypothesis on a new set of houses, you find that it makes unacceptably large errors in its predictions. What should you try next?
* Get more training examples <- waste of time.
* Try smaller sets of features.
* Try getting additional features.
* Try adding polynomial features: x<sub>1</sub><sup>2</sup>, x<sub>2</sub><sup>2</sup>, x<sub>1</sub>x<sub>2</sub>, etc.
* Try changing λ (decrease, increase).

People usually just randomly pick one of the method above to improve their model <- don't do that.

**Machinea learning diagnositc:** a test that you can run to gain insight what is/isn't working with a learning algorithm, and gain guidance as to how best to improve its performance.

=> it can take time to implement, but doing so can be a very good use of your time.


###### **Evaluating a Hypothesis**

Split the dataset into two portions:
* Training set (~70%), learn parameter θ from this (minimize training error J(θ)).
* Test set (~30%), compute J<sub>test</sub>(θ) on this.

*(*

*The J<sub>test</sub>(θ)'s definition ( the test set error(h<sub>θ</sub>(x<sub>test</sub>), y<sub>test</sub>) ) can be:*
* *squared error metric (for linear regression), or:*
* *or logistic function (for logistic regression, classification problem), or:*
* *misclassification error metric (0/1): for each (x, y) in test set; the error error(h<sub>θ</sub>(x), y) can be either 0 if it is correctly predicted, or 1 otherwise). Then J<sub>test</sub>(θ) = average(error of all (x, y) in test set).*

*)*

The hypothesis is good if J(θ) & J<sub>test</sub>(θ) are both low.

###### **Model Selection & Train/Validation/Test Sets**

Performance on the training set is not a good measure of generalization performance -> because of overfitting: just because a learning algorithm fits a training set well, that doesn't mean it's a good hypothesis (well predicting the test set).

**Model Selection problem**

Suppose we have 10 models:
1. h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x
2. h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x + θ<sub>2</sub>x<sup>2</sup> ...

10. h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x + ... + θ<sub>1</sub>x<sup>10</sup>

for each model, we fit it to the training set and this will give us 10 parameters θ:
1. θ<sup>(1)</sup>
2. θ<sup>(2)</sup> ...
10. θ<sup>(10)</sup>

in order to select a model, we can compute the error of these parameters on the test set, then see which model has the lowest test set error:
1. J<sub>test</sub>(θ<sup>(1)</sup>)
2. J<sub>test</sub>(θ<sup>(2)</sup>) ...
10. J<sub>test</sub>(θ<sup>(10)</sup>)

However, this will not be a fair estimate of how well the hypothesis generalizes, because we select the J<sub>test</sub> as a function with parameter d - the degree of the models -> it's something like we get stuck in overfitting on the test set, there is no warranty that the chosen model will do well on new examples that it hasn't been seen before.

=> Summary:
* we fit parameter θ to the training set -> overfitting.
* we fit parameter d to the test set -> overfitting.

**Evaluating a Hypothesis (cont)**

Instead of splitting in to 2 portions, we split in to 3:
* Training set (~60%).
* **Cross validation set (cv)**  (~20%).
* Test set (~20%).

We will also have their corresponding error functions J(θ), J<sub>cv</sub>(θ), J<sub>test</sub>(θ).

Then, instead of choosing the model that has the lowest test set error, we select one that has the lowest cross validation set error. And estimate generalization error for test set J<sub>test</sub>(*chosen* θ) (we might generally expect J<sub>cv</sub>(*chosen* θ) to be lower than J<sub>test</sub>(*chosen* θ)).

##### **Bias vs. Variance**

###### **Diagnosing Bias vs. Variance**

High bias problem -> θ ~ 0 -> underfitting problem.

High variance (bậc tự do) problem -> high degree model, many features... -> overfitting problem.

<center><img src="http://i.imgur.com/z5AJ8iV.png"/></center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center

If J<sub>cv</sub>(θ) or J<sub>test</sub>(θ) is high. Is it a bias problem (small d) or a variance problem (large d)?:

* Bias (underfit): J<sub>train</sub>(θ) is high, J<sub>cv</sub>(θ) ~ J<sub>train</sub>(θ).
* Variance (overfir): J<sub>train</sub>(θ) is low, J<sub>cv</sub>(θ) >> J<sub>train</sub>(θ).

###### **Regularization and Bias/Variance**
