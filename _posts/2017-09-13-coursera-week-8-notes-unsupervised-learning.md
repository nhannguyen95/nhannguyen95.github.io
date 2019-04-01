---
layout: post
title:  "Week 8 notes: Unsupervised Learning"
date: 2017-09-13 09:20:00 +0700
tags:
  - notes
  - machine learning
no-post-nav: 0
comments: 1
---

##### **Introduction**
No notes.

##### **Clustering**

###### **Introduction**
No notes.

###### **K-Means Algorithm**
K-Means is by far the most popular, the most widely used clustering algorithm.

K-Means is an iterative algorithm and it does two things:
* Cluster assignment step.
* Move centroid step.

Input:
* K (number of clusters).
* Traning set {x<sup>(1)</sup>, .., x<sup>(m)</sup>}. x<sup>(i)</sup> is n dimensional vectors.

**K-means algorithm**

Randomly initialize K cluster centroids μ<sub>1</sub>, .., μ<sub>K</sub>.

Repeat {

* for i = 1 to m (Cluster assignment step)
  * c<sup>(i)</sup> = index (from 1 to K) of cluster centroid closest to x<sup>(i)</sup>.
* for k = 1 to K (Move centroid step)
  * μ<sub>K</sub> = average (mean) of points assigned to cluster k.

}

###### **Optimization Objective**

μ<sub>c<sup>(i)</sup></sub> = cluster centroid of cluster to which example x<sup>(i)</sup> has been assigned.

**Optimization objective:** is to minimize the cost function:

$$ J(c^{(1)}, .., c^{(m)}, \mu _{1}, .., \mu _{K}) = \frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)} - \mu _{c^{(i)}} \right \|^{2}$$

Cluster assignment step: minimize J w.r.t c<sup>(i)</sup>.

Move centroid step: minimize J w.r.t μ<sub>k</sub>.

The cost function J must decrease over the number of iterations.

Cost function J here is also called **distortion function**.

###### **Random Initialization**

Help K-means avoid local optima.

**Recommended way**

Should have K < m

Randomly pick K training examples as K cluster centroids.

Depending on the random initialization, K-means can end up at different solutions. And in particular, K-means can actually end up at local optima.

**Local optima**

<hr>
<center><img src="https://i.imgur.com/84ZAcYr.png"/></center>
<center>Figure 1. Different local optimas</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

The term local optima refers to local optima of the cost function J.

If you want to increase the chance that K-means will find the best possible clustering, try multiple random initializations:

for i = 1 to 100 {

  * Randomly initialize K-means.
  * Run K-means; get c, μ.
  * Compute cost function J (distortion).

}

Pick the clustering that gave lowest cost J (out of 100).

###### **Choosing the Number of Clusters**

Elbow method: run K-means with different Ks and compute corresponding cost function Js, plot the graph (K, J)  then choose K at the Elbow point.

Shortcoming: The graph in practical don't usually have the Elbow shape, hard to know where is the location of the Elbow.

<hr>
<center><img src="https://i.imgur.com/ENJwL5w.png"/></center>
<center>Figure 2. Elbow method</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

**Choosing the value of K**

Is to ask for what purpose are you running K-means?

What is the number of K that serves that? whatever later purpose that you actually run the K-means for.

##### **Motivation**

###### **Motivation 1: Data Compression**
No notes.

###### **Motivation 2: Visualization**
No notes.

##### **Principal Component Analysis**

###### **Principal Component Analysis Problem Formulation**

Reduce from n-dimension to k-dimension: Find k vector u<sup>(1)</sup>, u<sup>(2)</sup>, .., u<sup>(k)</sup> onto which to project the data, so as to minimize the projection error.

###### **Principal Component Analysis Algorithm**

**Data preprocessing**

Training set: x<sup>(1)</sup>, .., x<sup>(m)</sup>.

Preprocessing (feature scaling/mean normalization)

$$ \mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x_{j}^{(i)}$$

Replace each x<sub>j</sub><sup>(i)</sup> with x<sub>j</sub> - μ<sub>j</sub>.

If different features on different scales (e.g., x<sub>1</sub> = size of house, x<sub>2</sub> = number of bedrooms), scale features to have comparable range of values.

**PCA Algorithm**

Reduce data from n-dimensions to k-dimensions.

Compute "covariance matrix" (ma trận hiệp phương sai):

$$ \Sigma = \frac{1}{m}\sum_{i=1}^{m}(x^{(i)})(x^{(i)})^{T}$$

$$ \Sigma$$ is going to be an nxn matrix.

Compute "eigenvectors" (vector riêng) of matrix $$ \Sigma$$:

$$ [U, V, S] = svd(Sigma);$$

_SVD: Singular Value Decomposition_

U is an nxn matrix, column ith is corresponding to u<sup>(i)</sup> vector. And if you want to reduce data from n-dimensions to k-dimensions, just take out the k first columns: u<sup>(1)</sup>, ..,u<sup>(k)</sup> which are the k directions on to which we want to project the data, we call this matrix U<sub>reduce</sub> - an nxk matrix.

Project x (n-dimensions) onto U<sub>reduce</sub> to get z (k-dimensions):

$$ z = U_{reduce}^{T} ~x$$

##### **Applying PCA**

###### **Reconstruction from Compressed Representation**

Given the point z in k-dimensions, how can we go back to x in n-dimensions?

$$ x_{approx} = U_{reduce} ~ z$$

###### **Choosing the Number of Principal Components**

k = number of principal components that we've retained.

**Choosing k**

Average squared projection error: $$ \frac{1}{m}\sum_{i = 1}^{m}\left \| x^{(i)} - x_{approx}^{(i)} \right \|^{2}$$, minimize this.

Total variation in the data: $$ \frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)} \right \|^{2}$$, on average: how far are my training examples from the origin?

Typically, choose k to be smallest value so that:

$$ \frac{\frac{1}{m}\sum_{i = 1}^{m}\left \| x^{(i)} - x_{approx}^{(i)} \right \|^{2}}{\frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)} \right \|^{2}} \leq 0.01 ~~~~~ (*)$$

_it means 99% of variance is retained."_

**Algorithm**

Try PCA with k = 1

Compute U<sub>reduce</sub>, z<sup>(1)</sup>, .., z<sup>(m)</sup>, x<sup>(1)</sup><sub>approx</sub>, ..,x<sup>(m)</sup><sub>approx</sub>.

Check if (*) is less than 0.01? If not, increase k and try again, until we find the satisfied value of k.

But doing this can be expensive because with each k, we have to recalculate everything. There's another way to do this:

$$ [U, S, V] = svd(Sigma)$$

S is a diagonal matrix, and for given k:

$$ (*) = 1 - \frac{\sum_{i=1}^{k}S_{ii}}{\sum_{i=1}^{n}S_{ii}}$$

and check if:

$$ \frac{\sum_{i=1}^{k}S_{ii}}{\sum_{i=1}^{n}S_{ii}} \ge 0.99$$

_again, it means 99% of variance retained._

###### **Advice for applying PCA**

**Supervised learning speedup**

Input: (x<sup>(1)</sup>, y<sup>(1)</sup>), ..,(x<sup>(m)</sup>, y<sup>(m)</sup>).

x are very high demensional, say 10000.

Extract input:
* Unlabeled dataset: x<sup>(1)</sup>, ..,x<sup>(m)</sup> in R<sup>10000</sup>.

* Then we apply PCA and get z<sup>(1)</sup>, ..,z<sup>(m)</sup> in R<sup>1000</sup>.

New training set: (z<sup>(1)</sup>, y<sup>(1)</sup>), ..,(z<sup>(m)</sup>, y<sup>(m)</sup>).

Then we can pass this into learning algorithms: linear regression, neural network...

Note: Mapping x<sup>(i)</sup> to z<sup>(i)</sup> should be defined by running PCA only on the training set. This mapping can be applied as well to the examples x<sub>cv</sub><sup>(i)</sup> and x<sub>test</sub><sup>(i)</sup> in the cross validation and test sets.

**Application of PCA**

Compression
* Reduce memory/disk needed to store data.
* Speedup learning algorithm.

Visualization

**Bad use of PCA: to prevent overfitting**

Use z instead of x to reduce the number of features to k < n.

Thus, fewer features, less likely to overfit.

This might work OK, but is not a great way to address over-fitting, (use regularization instead); because PCA doesn't care what the value of y is (the label), and it might throw away some valuable information.

**PCA is sometimes used where it shouldn't be**

People usually design ML system with the involvement of PCA right from the beginning. But in fact, one should ask this question first: How about doing the whole thing without using PCA?

Before implementing PCA, first try running whatever you want to do with the original/raw data x. Only if that doesn't do what you want, then implement PCA and consider using z.
