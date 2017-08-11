---
layout: post
title:  "Week 6 notes: Advice for applying machine learning"
date: 2017-07-22 10:24:00 +0700
categories: ['machine learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Introduction**
No notes.

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
*  h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x
*  h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x + θ<sub>2</sub>x<sup>2</sup>

...
* h<sub>θ</sub>(x) = θ<sub>0</sub> + θ<sub>1</sub>x + ... + θ<sub>1</sub>x<sup>10</sup>

for each model, we fit it to the training set and this will give us 10 parameters θ (by minimizing J(θ)):
* θ<sup>(1)</sup>
* θ<sup>(2)</sup>

...
* θ<sup>(10)</sup>

for example J(θ) in linear regression problem:
$$ J(\theta)=\frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2} + \frac{\lambda}{2m}\sum_{j=1}^{n}\theta^{2}_{j}$$


in order to select a model, we can compute the error of these parameters on the test set, then see which model has the lowest test set error:
* J<sub>test</sub>(θ<sup>(1)</sup>)
* J<sub>test</sub>(θ<sup>(2)</sup>)

...
* J<sub>test</sub>(θ<sup>(10)</sup>)

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
*(Bias/variance as a function of the polynome degree)*

High bias problem **->** θ ~ 0 **->** underfitting problem.

High variance (bậc tự do) problem -> high degree model, many features... -> overfitting problem.

<hr>
<center><img src="http://i.imgur.com/z5AJ8iV.png"/></center>
<center>Figure 1. Bias/variance as a function of the polynome degree</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

If J<sub>cv</sub>(θ) or J<sub>test</sub>(θ) is high. Is it a bias problem (small d) or a variance problem (large d)?:

* Bias (underfit): J<sub>train</sub>(θ) is high, J<sub>cv</sub>(θ) ~ J<sub>train</sub>(θ).
* Variance (overfir): J<sub>train</sub>(θ) is low, J<sub>cv</sub>(θ) >> J<sub>train</sub>(θ).

###### **Regularization and Bias/Variance**

Large λ **->** θ is heavily penalized **->** θ ~ 0 **->** h<sub>θ</sub>(x) ~ θ<sub>0</sub> **->** underfitting.

Small λ **->** θ don't change much **->** high-order polynomials exist **->** possibly overfitting.

Intermediate λ **->** good => how can we choose this value?

**Choosing the regularization parameter λ**

Suppose we have model:

$$ h_{\theta}(x)=\theta_{0} + \theta_{1}x + \theta_{2}x^{2} + \theta_{3}x^{3} + \theta_{4}x^{4}$$

$$ J(\theta)=\frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2} + \frac{\lambda}{2m}\sum_{j=1}^{n}\theta^{2}_{j}$$

And the error function of the datasets:

$$ J_{train}(\theta)=\frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2}$$

$$ J_{cv}(\theta)=\frac{1}{2m_{cv}}\sum_{i=1}^{m_{cv}}(h_{\theta}(x_{cv}^{(i)})-y_{cv}^{(i)})^{2}$$

$$
J_{test}(\theta)=\frac{1}{2m_{test}}\sum_{i=1}^{m_{test}}(h_{\theta}(x_{test}^{(i)})-y_{test}^{(i)})^{2}$$

Then we try some value of λ:
* Try λ = 0
* Try λ = 0.01 (double λ)
* Try λ = 0.02

...
* Try λ = 10.24 (total 12 tries)

at i<sup>th</sup> try, we minimize J(θ) in order to find θ (perform on the training set). Then we choose one that has lowest J<sub>cv</sub>(*chosen* θ) among 12 tries (perform on cross validation). Finally report how well it does on the test set (by computing J<sub>test</sub>(*chosen* θ)).

**Bias/variance as a function of the regularization parameter λ**

<hr>
<center><img src="http://i.imgur.com/B50IFz9.png" width="280"/></center>
<center>Figure 2. Bias/variance as a function of the regularization parameter λ</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

###### **Learning curves**

**Learning curve:** used to sanity check that your algorithm is working correctly, or improve the performance of the algorithm

=> diagnose if a physical learning algorithm may be suffering from bias, or variance problem or a bit of both.

<hr>
<center><img src="http://i.imgur.com/NmLuhFP.png" width="550"/></center>
<center>Figure 3. Bias/variance as a function of the polynome degree.</center>
<center>When m is small, it's pretty easy to fit every single one of the training examples perfectly, so the error will be small.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

**Using learning curve to diagnose a high bias problem:**

<hr>
<center><img src="http://i.imgur.com/GCaPK7C.png" width="350"/></center>
<center>Figure 4. Learning curve in high bias problem.</center>
<center>There are so few parameters & so much data (when m is large), the performance on the training set & the cross validation set will be very similar.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Low training set size (small m): causes J<sub>train</sub>(Θ) to be low and J<sub>cv</sub>(Θ) to be high.

Large training set size (large m): causes both J<sub>train</sub>(Θ) and J<sub>cv</sub>(Θ) to be high with J<sub>train</sub>(Θ)≈J</sub>cv</sub>(Θ).

If a learning algorithm is suffering from high bias, getting more training data will not (by itself) help much.

**Using learning curve to diagnose a high variance problem:**

<hr>
<center><img src="http://i.imgur.com/oFOcGoy.png" width="350"/></center>
<center>Figure 5. Learning curve in high variance problem.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Low training set size (small m): J<sub>train</sub>(Θ) will be low and J<sub>cv</sub>(Θ) will be high.

Large training set size (large m): J<sub>train</sub>(Θ) increases with training set size and J<sub>cv</sub>(Θ) continues to decrease without leveling off. Also, J<sub>train</sub>(Θ) < J<sub>cv</sub>(Θ) but the difference between them remains significant.

If a learning algorithm is suffering from high variance, getting more training data is likely to help.

###### **Deciding What to Try Next (revisited)**

Suppose you have implemented regularized linear regression to predict housing. However, when you test your hypothesis on a new set of houses, you find that it makes unacceptably large errors in its predictions. What should you try next?
* Get more training examples <- waste of time.
  * fixes high variance.
* Try smaller sets of features.
  * fixes high variance.
* Try getting additional features.
  * fixes high bias.
* Try adding polynomial features:
 x<sub>1</sub><sup>2</sup>, x<sub>2</sub><sup>2</sup>, x<sub>1</sub>x<sub>2</sub>, etc.
  * fixes high bias.
* Try decreasing λ.
  * fixes high bias.
* Try increasing λ.
  * fixes high variance.

**Apply bias-variance diagnosis to select a neural network architecture**

<hr>
<center><img src="http://i.imgur.com/ePKC8xW.png" width="500"/></center>
<center>Figure 6. Neural networks and overfitting.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Usually, using a larger neural network by using regularization to address overfitting is more effective than using a smaller neural network.

Using a single hidden layer is a good starting default. You can train your neural network on a number of hidden layers using your cross validation set. You can then select the one that performs best.

**Model Complexity Effects:**

* Lower-order polynomials (low model complexity) have high bias and low variance. In this case, the model fits poorly consistently.
* Higher-order polynomials (high model complexity) fit the training data extremely well and the test data extremely poorly. These have low bias on the training data, but very high variance.
* In reality, we would want to choose a model somewhere in between, that can generalize well but also fits the data reasonably well.
