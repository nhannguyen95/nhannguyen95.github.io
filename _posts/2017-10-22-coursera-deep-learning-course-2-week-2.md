---
layout: post
title:  "Coursera Deep Learning Course 2 Week 2 notes: Optimization algorithms"
date: 2017-10-22 09:16:00 +0700
categories: ['machine learning', 'deep learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Optimization algorithms**

###### **Optimization algorithms**

x<sup>{i}</sup>, y<sup>{i}</sup>: the i<sup>th</sup> mini batch.

For example if we have m = 5000000 and the batch size = 1000, then there are 5000 mini batches.

<hr>
<center><a href="https://i.imgur.com/5L10VMS.png" target="_blank">
<img width="600" src="https://i.imgur.com/5L10VMS.png"/></a>
</center><center>Figure 1. Implementation of Mini-batch gradient descent.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Passing this for loop is called doing 1 `epoch` of training (means a single pass through the training set).

Batch gradient descent: 1 epoch allows us to take only 1 gradient descent step.

Mini-batch gradient descent: 1 epoch allows us to take (say) 5000 gradient descent step.

###### **Understanding mini-batch gradient descent**

<hr>
<center><a href="https://i.imgur.com/FbSYeEy.png" target="_blank">
<img width="600" src="https://i.imgur.com/FbSYeEy.png"/></a>
</center><center>Figure 2. It's ok if the cost function doesn't go down on every iteration while running Mini-batch gradient descent.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

[Recall](https://nhannguyen95.github.io/2017/09/15/coursera-week-10-notes-large-scale-machine-learning)

Mini-batch size = m: Batch gradient descent (BGD).

Mini-batch size = 1: Stochastic gradient descent (SGD).

In practice, Mini-batch size = (1, m):
* If you use BGD: costly computation per iteration.
* If you use SGD: lose the speed up from vectorization.

If small training set: just use BGD.

If bigger traniing set: mini-batch size = 64, 128, 256, 512...

Make sure mini-batch size fits in CPU/GPU memory.

###### **Exponentially weighted averages**

<hr>
<center><a href="https://i.imgur.com/lFo9ig0.png" target="_blank">
<img src="https://i.imgur.com/lFo9ig0.png"/></a>
</center><center>Figure 3. Exponentially weighted averages.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Blue scatter: **θ<sub>i</sub> = x** (temperature of day i is x, 1 ≤ i ≤ 365).

Red line (**Moving average/Exponentially weighted averages of the daily temperature**): **v<sub>0</sub> = 0, v<sub>i</sub> = βv<sub>i-1</sub> + (1 - β)θ<sub>i</sub>**.

Intuition: v<sub>i</sub> is between v<sub>i-1</sub> and θ<sub>i</sub>. The higher β is, the closer v<sub>i</sub> is to v<sub>i-1</sub>.

Another intuition: v<sub>i</sub> as approximately averaging over $$ \frac{1}{1 - \beta}$$ days' temperature.

###### **Understanding exponentially weighted averages**

**Implementing exponentially weighted averages**

V<sub>θ</sub> = 0

V<sub>θ</sub> = βV<sub>θ</sub> + (1 - β)θ<sub>1</sub>

V<sub>θ</sub> = βV<sub>θ</sub> + (1 - β)θ<sub>2</sub>

...

###### **Bias correction in exponentially weighted averages**

**Problem**

<hr>
<center><a href="https://i.imgur.com/oXc9LEH.png" target="_blank">
<img src="https://i.imgur.com/oXc9LEH.png"/></a>
</center><center>Figure 4. The expected green curve with β = 0.98, and the real purple curve.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

This is because the initialization of v<sub>0</sub> = 0.

**How to fix?**

v<sub>i</sub>(new) = v<sub>i</sub>(old) / (1- β<sup>t</sup>)

###### **Gradient descent with momentum**

This almost always works faster than the standard gradient descent.

This is what happens when you're using Mini-batch Gradient Descent:

<hr>
<center><a href="https://i.imgur.com/FsikbuO.png" target="_blank">
<img width="600" src="https://i.imgur.com/FsikbuO.png"/></a>
</center><center>Figure 5. This up and down (blue curve) oscillations slows down gradient descent and prevent you from using a larger learning rate (because it may be overshooting and end up diverging - purple curve).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Gradient descent with momentum:

```
On iteration t:
  Compute dW, db on current mini-batch.
  VdW = β.VdW + (1 - β).dW
  Vdb = β.Vdb + (1 - β).db

  W = W - α.VdW
  b = b - α.Vdb
```

This smooths out the steps of gradient descent:

<hr>
<center><a href="https://i.imgur.com/7wvu396.png" target="_blank">
<img width="600" src="https://i.imgur.com/7wvu396.png"/></a>
</center><center>Figure 6. The step curve of gradient descent with momentum (red curve).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Imagine you take a ball and roll it down a bowl shape area:
* The derivatives term dW, db: provide acceleration to the ball.
* Momentum term VdW, Vdb: represent the velocity.
* β: plays the role of friction (prevents the ball from speeding up without limit).

Hyperparameters: α, β. The most comment value of β is 0.9 (average on last 10 gradient iterations).

People don't usually use bias correction in gradient descent with momentum because only after about 10 iterations, your moving average will have already warmed up and is no longer a bias estimate.

###### **RMSprop**

**Root Mean Square prop**

```
On iteration i:
  Compute dW, db on current mini-batch.
  SdW = β2.SdW + (1 - β2).dW**2
  Sdb = β2.Sdb + (1 - β2).db**2

  W = W - α.dW / (sqrt(SdW) + ε)
  b = b - α.db / (sqrt(Sdb) + ε)
```

By putting momentum and RMSprop together, you can get an even better optimization algorithm.

###### **Adam optimization algorithm**

**Adam = Adaptive moment estimation**

<hr>
<center><a href="https://i.imgur.com/mnE5YwA.png" target="_blank">
<img width="600" src="https://i.imgur.com/mnE5YwA.png"/></a>
</center><center>Figure 7. Adam optimization algorithm.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Hyperparameters choice:
* α: needs to be tune
* β1 = 0.9
* β2 = 0.999
* ε = 10<sup>-8</sup>

###### **Learning rate decay**

Learning rate decay = slowly reduce learning rate over time.

<hr>
<center><a href="https://i.imgur.com/d18roRI.png" target="_blank">
<img width="600" src="https://i.imgur.com/d18roRI.png"/></a>
</center><center>Figure 8. Algorithm with a fixed value of learning rate may never converge (blue curve).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

**Learning rate decay**

$$ \alpha = \frac{1}{1 + decayRate * epochNum}\alpha_{0}$$

$$ \alpha_{0}$$ and $$ decayRate$$ become hyperparameters you need to tune.

**Other learning rate decay methods**

* $$ \alpha = 0.95^{epochNum}\alpha_{0}$$: exponentially decay.

* $$ \alpha = \frac{k}{\sqrt{epochNum}}\alpha_{0}$$ or $$ \frac{k}{\sqrt{t}}\alpha_{0}$$

* Discrete staircase: decrease learning rate (say by half) after some iterations.

* Manual decay: manually decrease learning rate.

###### **The problem of local optima**

<hr>
<center><a href="https://i.imgur.com/isnTNWt.png" target="_blank">
<img width="600" src="https://i.imgur.com/isnTNWt.png"/></a>
</center><center>Figure 9. The local optima problem people often worried about (left) and the reality in deep learning.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Most point of zero gradients (zero derivatives) are not local optima like points in the left graph, but saddle points in the right graph.

If in a very high dimension space (say 20000), for a point to be a local optima, all of its 20000 directions need to be increasing or decreasing, and the chance of that happening is maybe very small (2<sup>-20000</sup>).

So the local optima is not problem, but:

**Problem of plateaus**

A plateau is a region where the derivative is close to zero for a long time.

Plateaus can really slow down learning.

Summary:
* You're unlikely to get stuck in a bad local optima (so long as you're training a reasonably large neural network, lots os parameters, and the cost function J is defined over a relatively high dimensional space).
* Plateaus can make learning slow, and some of the optimization algorithms may help you.
