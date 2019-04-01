---
layout: post
title:  "Week 7 notes: Support Vector Machines"
date: 2017-08-30 13:47:00 +0700
tags:
  - notes
  - machine learning
no-post-nav: 0
comments: 1
---

##### **Introduction**
No notes.

##### **Large Margin Classification**

###### **Optimization Objective**

**Support Vector Machine**

Learn complex non-linear function.

<hr>
<center><img src="http://i.imgur.com/73icLSl.png"/></center>
<center>Figure 1. Relation between Logistic Regression and SVM</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

###### **Large Margin Intuition**

**SVM Decision Boundary**

Consider a case where we set constant C to be a very large value, when minimizing the optimization objective, we are going to be highly motivated to choose a value, so that the first term is equal to 0. So what would it take to make this first term equal to 0.

<hr>
<center><img src="http://i.imgur.com/0uYq8eE.png"/></center>
<center>Figure 2. SVM optimization objective</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

When the first term is equal to 0, we need to minimize $$ \frac{1}{2}\sum_{i=1}^{n} \theta_{i} ^{2}$$ (ignored θ<sub>0</sub>).

**Linear separable case**

The obtained decision boundary when minimizing the optimization objective will have the margin as large as possible (hence the name **Large Margin Intuition**).

This means SVM will choose the black decision boundary instead of the pink and green one:

<hr>
<center><img src="http://i.imgur.com/pXZSSal.png"/></center>
<center>Figure 3. Large margin classifier</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

###### **Mathematics Behind Large Margin Intuition**

**Vector Inner Product**

$$ u^{T}v = p\left \|u\right \|$$

`p` = length of projection of `v` onto `u`. p can be positive or negative.

<hr>
<center><img src="http://i.imgur.com/XS5iB07.png"/></center>
<center>Figure 4. Vector Inner Product</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

**SVM Decision Boundary**

We can rewrite the optimization objective of SVM as follow:
$$ \min_{\theta}\frac{1}{2}\sum_{j=1}^{n}\theta_{j}^{2} = \frac{1}{2}\left \| \theta \right \|^{2}$$

s.t.

$$ p^{(i)}\left \| \theta \right \| \geq 1 ~~~ if ~~ y^{(i)}=1$$

$$ p^{(i)}\left \| \theta \right \| \lt -1 ~~~ if ~~ y^{(i)}=0$$

where p<sup>(i)</sup> is the projection of x<sup>(u)</sup> onto the vector θ.

Simplification: θ<sub>0</sub> = 0.

According to the illustration below, with the minimal value of the magnitude of θ, the absolute value of p will large as much as possible (hence the large margin).

<hr>
<center><img src="http://i.imgur.com/ZrXtYUv.png"/></center>
<center>Figure 5. When the magnitude of θ is minimal, the margin is large</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

More intuitive illustration:

<hr>
<center><img src="http://i.imgur.com/xhifXxk.png"/></center>
<center>Figure 5. The margin line</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>


##### **Kernels**

It's a technique.

###### **Kernels I**



###### **Kernels II**
