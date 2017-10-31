---
layout: post
title:  "Coursera Deep Learning Course 2 Week 1 notes: Practical aspects of Deep Learning"
date: 2017-10-20 16:20:00 +0700
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

The goal of dev set is to help you quickly see which model is better, so dev set only need to be large enough, not exactly 20%. Similarly to test set.

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

[Recap](https://nhannguyen95.github.io/2017/07/22/coursera-week-6-notes-advice-for-applying-machine-learning)

Assume that optimal error = 0%:

|Train error|1%|15%|15%|0.5%|
|-|-|-|-|-|
|**Dev errror**|**11%**|**16%**|**30%**|**1%**|
||high variance|high bias|both|low bias, low variance|

###### **Basic Recipe for Machine Learning**

High bias? (training data performance):
* Bigger network.
* Training longer.
* Neural Networks architecture search.

High variance? (dev set performance):
* More data.
* Regularization.
* Neural Networks architecture search.

We don't have tools to reduce bias/variance without hurting the other one => **Bias Variance tradeoff**.

##### **Regularizing your neural network**

###### **Regularization**

L2 regularization:

$$ \left \| w \right \|_{2}^{2} = \sum_{j=1}^{n_x}w_{j}^{2} = w^{T}w$$

L1 regularization:

$$ \left \| w \right \|_{1} = \sum_{j=1}^{n_x}\left | w_{j} \right |$$

Regularization term in neural network:

$$ \frac{\lambda}{2m}\sum_{l=1}^{L}\left \| w^{[l]}\right \|^{2}_{F}$$

where $$ \left \| w^{[l]}\right \|^{2}_{F} = \sum_{i=1}^{n^{[l]}}\sum_{j=1}^{n^{[l-1]}}(w_{ij})^{2}$$ is called **Frobenius norm**. (L2 for matrix).

The alternative name for L2 regularization is **weight decay**.

###### **Why regularization reduces overfitting?**

Regularization makes $$ w^{[l]} \approx 0$$, thus reduce the impact of a lot of hidden units, and the neural network may become simpler:

<hr>
<center><a href="https://i.imgur.com/AyE8Uxs.png" target="_blank">
<img src="https://i.imgur.com/AyE8Uxs.png"/></a>
</center><center>Figure 1. Regularization makes the neural network simpler.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Another intuition: in case of using $$tanh$$ as activation function:

If $$ \lambda$$ large, $$ w^{[l]}$$ is penalized much and become small, and $$ z^{[l]} = w^{[l]}a^{[l-1]} + b^{[l]}$$ small, then $$ tanh(z^{[l]})$$ will be roughly linear:

<hr>
<center><a href="https://i.imgur.com/GcJtmXj.png" target="_blank">
<img src="https://i.imgur.com/GcJtmXj.png"/></a>
</center><center>Figure 2. The red roughly linear part of tanh function when z is small.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Every layer will be roughly linear, and as a result: the neural network is just a linear network.

Thus regularization reduces overfitting.

###### **Dropout Regularization**

<hr>
<center><a href="https://i.imgur.com/o27QROW.png" target="_blank">
<img width="600" src="https://i.imgur.com/o27QROW.png"/></a>
</center><center>Figure 3. Eliminate some hidden activation units by probability.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

**Implementing dropout ("Inverted dropout")**

We define a variable `keep_prob`.

At layer (say) 3, create a random vector d<sup>[3]</sup> which is the same size with a<sup>[3]</sup>:

```
d3 = np.random.rand(a3.shape[0], a3.shape[1]) < keep_prob
a3 = np.multiply(a3, d3)
a3 /= keep_prob
```

For example `keep_prob = 0.8`, `a3` will be reduced by 1 - `keep_prob` = 0.2 percent (20% of the elements of `a3` will be zeroed out), in order to not reduce the expected value of `z4=w4.a3 + b4`, we need to divide `a3` by `keep_prob`.

The line `a3 /= keep_prob` is called **inverted dropout** technique: no matter what you set the value of `keep_prob` to, the expected value of `a3` is ensure to remain the same.

You should randomly zero different hidden units: e.g. in the iteration 1 of gradient descent, you may zero out some hidden units; the second iteration, may be you zero out different part of hidden units, and so on...

**Making predictions at test time**

Do not using drop out, just perform feed forward to compute the output.

###### **Understanding Dropout**

**1<sup>st</sup> intuition**: doing with simpler neural network has regularization effect.

**2<sup>nd</sup> intuition**: can't rely on any one feature (because they can be randomly zeroed out), so have to spread out the weight. And by spreading out the weight, this will tend to have the **Shrinking the square norm of the weight** effect. (Similar effect to L2 regularization).

Each layer can have different `keep_prob` value:
* With layer you don't worry about overfitting: can set `keep_prob` value to be high (say 1.0).
* Otherwise: decrease `keep_prob` value at the layer where you're most worry about overfitting.

Dropout can technically apply for input layer, but in practice it is not used that often.

Downside: cost function J is not well-defined any longer, so we can't plot J as a function of number of iteration to debug.

###### **Other regularization methods**

**Increase data size by data augmentation**: rotate image, flipping image horizontally...

<hr>
<center><a href="https://i.imgur.com/yoXOPFe.png" target="_blank">
<img width="600" src="https://i.imgur.com/yoXOPFe.png"/></a>
</center><center>Figure 4. Data augmentation.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

**Early stopping**

<hr>
<center><a href="https://i.imgur.com/MqWRRdd.png" target="_blank">
<img width="600" src="https://i.imgur.com/MqWRRdd.png"/></a>
</center><center>Figure 5. Stop halfway to avoid overfitting and achieve mid-size value of parameters.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Downside: In ML, you need to care about Optimizing cost function J and Avoiding overfitting. ML will be easier to think about when you have tools for Optimizing J, then it is completely a separate task to not overfit (reduce variance).

This principle is sometimes called **Orthogonalization**, and this is the idea that you want to think about 1 task at a time.

Early stopping couples these 2 tasks: you no longer can work on these 2 problems independently; because when you stop gradient descent early, you're sort of breaking the Optimization of J, so you're not doing a great job reducing J; you also simultaneously try to not overfit.

So instead of using different tools to solve 2 problems, you're using 1 tool that kind of mixes the 2.

Alternative method: using L2 regularization, then you can just train the neural network as long as possible. But downside: you have to try a lot of value of lambda (Early stopping doesn't need to do this, and it just runs gradient descent once).

##### **Setting up your optimization problem**

###### **Normalizing inputs**

<hr>
<center><a href="https://i.imgur.com/wEogiXJ.png" target="_blank">
<img width="600" src="https://i.imgur.com/wEogiXJ.png"/></a>
</center><center>Figure 6. Illustrate normalization with 2 step: subtract mean and divide variance.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

<hr>
<center><a href="https://i.imgur.com/zT8rUZO.png" target="_blank">
<img width="600" src="https://i.imgur.com/zT8rUZO.png"/></a>
</center><center>Figure 7. Normalization helps gradient descent run faster.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

###### **Vanishing/Exploding gradients**

When you're training a very deep network, your derivative (slope) can sometimes get very very big (or very small). This makes training difficult.

###### **Weight Initialization for Deep Networks**

A partial solution for this problem: more careful choice of random weight initialization.

```
wl = np.random.randn(shape) * np.sqrt(var)
```

where `var` is either:

* $$ \frac{2}{n^{[l-1]}}$$ for ReLU activation function.

* $$ \frac{1}{n^{[l-1]}}$$ for tanh activation function.

* Another version of `var`: $$ \frac{2}{n^{[l-1]} + n^{[l]}}$$

All of this is to set the variance of `wl` to be equal to `var` (thus avoid `zl` exploding).

###### **Numerical approximation of gradients**

$$ f'(\theta) = \lim_{\epsilon \rightarrow  0}\frac{f(\theta + \epsilon) - f(\theta - \epsilon)}{2\epsilon}$$

###### **Gradient Checking**

**Gradient checking for a neural network**

Take W<sup>[1]</sup>, b<sup>[1]</sup>... and reshape into a big vector θ.

Take dW<sup>[1]</sup>, db<sup>[1]</sup>... and reshape into a big vector dθ.

<hr>
<center><a href="https://i.imgur.com/837tN7R.png" target="_blank">
<img width="600" src="https://i.imgur.com/837tN7R.png"/></a>
</center><center>Figure 8. Implementation of Grad Check.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

###### **Gradient Checking Implementation Notes**

Don't use in training - only in debug (because it's expensively computational).

If algorithm fails grad check, look at components (correspond W, b) to try to identify bug.

Remember regularization.

Grad check doesn't work with dropout.

Run at random initialization, perhaps again after some training (after some number of iterations).
