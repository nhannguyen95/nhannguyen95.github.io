---
layout: post
title:  "Week 3 - Hyperparameter tuning, Batch Normalization and Programming Frameworks"
date: 2017-10-31 20:27:00 +0700
# categories: ['machine learning', 'deep learning']
tags:
  - Coursera Deep Learning Course 2 - Improving Deep Neural Networks notes
comments: true
mathjax: true
content_level: 3
img: /images/deep-learning-ai.png
summary: Convolutional Neural Networks notes
---

## **Hyperparameter Tuning**

### **Tuning process**

Most important hyperparameters:
* Learning rate α.
* Momentum term β (0.9 is a good default).
* Mini-batch size.
* Number of hidden units.

Some less important hyperparameters:
* Number of layers.
* Hyperparameters in learning rate decay.
* β<sub>1</sub>, β<sub>2</sub>, ε (0.9, 0.999, 10<sup>-8</sup> are good default values).

{% include image.html
  url="https://i.imgur.com/ECau3cP.png"
  cap="Figure 1. Try random values, don't use a grid."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

The reasons you shouldn't use a grid is that it's difficult to know in advance which hyperparameters is the most important one for your problem.

For example, hyperparater1 = learning rate α (important), hyperparameter2 = ε (hardly matter): you may train 25 models and only try out 5 values of α (because all ε values give the same answer).

In practice, you might be searching over many more hyperparameters (instead of only 2).

**Coarse to fine sampling scheme**

{% include image.html
  url="https://i.imgur.com/OmW6ulI.png"
  cap="Figure 2. After doing a coarse sample of the entire square, you find that your algorithm works really well on the blue-circled points, then you may want to focus more resources on searching within the smaller blue square if you're suspecting that the best setting, you can then sample more densely into smaller square."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **Using an appropriate scale to pick hyperparameters**

This lesson talks about the way to correctly random the values of hyperparameters.

[Discrete uniform distribution](https://en.wikipedia.org/wiki/Discrete_uniform_distribution)

[Continuous uniform distribution](https://en.wikipedia.org/wiki/Uniform_distribution_(continuous))

**Picking hyperparameters at random**

Says we want to randomly pick value for $n^{[l]}$ using (discrete) uniform distribution, and the value is between 50 and 100, this is reasonable.

**Appropriate scale for hyperparameters**

However, if we random the value for the learning rate between 0.0001 and 1, we will spend 90% ($$ \frac{0.9}{1 - 0.0001}$$) resources for searching values between 0.1 and 1, and only 10% for searching values between 0.0001 and 0.1, this doesn't seem good.

Instead, it will be more reasonable to search for the learning rate on a log scale (instead of using linear scale).

For example, if we use the type of logarithmic scale (base 10), we represent α = 10<sup>r</sup>, and we random r using continuous uniform distribution (instead of random on α).

```
r = -4 * np.random.rand()  // 4 = log10(0.0001)
alpha = 10**r
```

**Hyperparameters for exponentially weighted averages**

In case you want to random the value of momentum term β from 0.9 to 0.999, it doesn't make sense to sample on linear scale.

Instead, we random 1 - β, now we will sample between 0.001 to 0.1 (and it returns to the case of learning rate).

More formal mathematical justification for why we're doing this: when β is close to 1, the sensitivity of the results you get changes, even with very small changes to β:

If β goes from 0.9 to 0.9005, it's no big deal. (it still takes the average over 10 days)

If β goes from 0.999 to 0.9995, this will have a huge impact on your algorithm. (it changes from taking the average over 1000 days to 2000 days).

This whole sampling process causes you to sample more densely in the region of when β is close to 1. (or when 1 - β is close to 0.)

### **Hyperparameters tuning in practice: Pandas vs. Caviar**

In terms of how people search for hyperparameters, there are 2 major different ways:
* **Babysitting one model** even as it's training over many days or several weeks: watching the model performance and patiently nudging the learning rate up or down. This is used when you have a huge data but don't have enough resources to train a lot of models at the same time.
* **Training many models in parallel**: and pick the best one. This is used when you have enough resources.

We also call the first approach the **Panda** approach, and the latter **Caviar** strategy (related to how these 2 animals raise their children).

## **Batch Normalization**

### **Normalizing activations in a network**

Batch normalization makes your hyperparameters search problem much easier and makes your neural networks much more robust and will also enable you to much more easily train even very deep networks.

We previously normalize the input feature X ($a^{[0]}$), in a (deep) neural network, can we also normalize $a^{[l]}$ (hidden units) so as to train $W^{[l+1]}, b^{[l+1]}$ faster? (Since $a^{[l]}$ is the input of layer l+1.)

This is what Batch Normalization does, we actually normalize $z^{[l]}$ (not $a^{[l]}$, there are some debate about this, but in practice, normalizing z first is much more efficiently).

**Implementing Batch norm**

At some layer l: ($$ z^{[l](i)} = z^{(i)}$$ for the sake of simplicity).

$$ \mu = \frac{1}{m}\sum_{i}z^{(i)}$$

$$ \sigma^{2} = \frac{1}{m}\sum_{i}(z^{(i)} - \mu)^{2}$$

$$ z^{(i)}_{norm} = \frac{z^{(i)} - \mu}{\sqrt{\mu^{2} - \epsilon}}$$

Now $$ z^{(i)_{norm}}$$ will have mean 0 and variance 1. But we want to make the hidden unit values have other means and variances as well, and **we can control the mean and variance by setting**:

$$ \tilde{z}^{(i)} = \gamma z^{(i)}_{norm} + \beta$$

$$ \gamma$$ and $$ \beta$$ are learnable parameters, they are updated the same way we update W and b.

Intuition: for example if you're using Sigmoid activation function, you may not want your z values clustered in the region of mean 0 and variance 1:

{% include image.html
  url="https://i.imgur.com/2DLfCIj.png"
  cap="Figure 3. Sigmoid function and the distribution of mean 0 and variance 1 data."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

in order to better take advantage of the nonlinearity of sigmoid function.

### **Fitting Batch Norm into a neural network**

$$ a^{[l]} = g^{[l]}(\tilde{z}^{[l]})$$

**Working with mini-batches**

In practice, batch norm is usually applied with mini-batches of the training set.

Because batch norm zeros out the mean of $z^{[l]}$ values (say in layer l), we can eliminate the parameters $b^{[l]}$:

$$ z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]} = W^{[l]}a^{[l-1]}$$

Actually they are replaced by $$ \beta^{[l]}$$ parameters (which is used to control the mean of z values):

$$ \tilde{z}^{[l]} = \gamma^{[l]}z^{[l]}_{norm} + \beta^{[l]}$$

**Implementing gradient descent with Batch norm**

```
for t = 1 .. number of minibatches
  Compute forward propagation on X^{t}
    In each hidden layer, use BN to replace z^[l] with z~^[l].

  Use back propagation to compute dW^[l], db^[l], dβ^[l],
   dɣ^[l]

  Update the parameters
   (you can perform the update with momentum, RMSProp, Adam..)
```

### **Why does Batch Norm work?**

**Covariate shift**:

If you learned some X to Y mapping, if the distribution of X changes, then you might need to retrain your learning algorithm.

And this is true even if the ground true function mapping from X to Y remains unchanged.

(For example you're training a cat classifier on the training set with only black cat images, and you predict on the test set which has colored cat images).

**Why covariate shift is a problem with neural networks?**

Consider the layer l = 3, this layer has the input which is the activation units of the 2nd layer $a^{[2]}$, and is also affected by the previous layers' values; it bases on all of those values and try to map them to $ \hat{y}$. (i.e. learn $W^{[>=3]}, b^{[>=3]}$ parameters so the network does a good job.)

However, $a^{[2]}$ changes all the time (because $W^{[<3]}, b^{[<3]}$) change all the time), and so it's suffering from the problem of covariate shift.

**What batch norm does** is that it reduces the amount that the distribution of these hidden unit values shifts around. It means that the hidden unit values may change, but their mean and variance remain the same. Therefore, it limits the amount to which updating the parameters in the earlier layers can affect the distributions of values that the 3rd layer sees. Or in other words:
* It causes the previous hidden unit values become more stable.
* Even as the earlier layers keep learning, the amounts that this forces the later layers to adapt to its changes is reduced.
* And so it allows each layer of the network to learn by itself (a little bit more independently of other layers).

And this has the effect of speeding up of learning in the whole network.

**Batch norm also has regularization effect**

* Each mini-batch is scaled by the mean/variance computed on just that mini-batch.
* This adds some noise to the values $z^{[l]}$ within that mini-batch. So similar to dropout, it adds some noise to each hidden layers' activations.
* This has a slight regularization effect.

If you use a bigger mini-batch size, you're reducing noises and therefore reducing the regularization effect.

### **Batch Norm at test time**

Go back here (to take notes) after finishing programming assignment.

## **Multi-class classification**

### **Softmax Regression**

$$ C$$ = number of classes.

The output layer of the neural network now has C node (C > 2), the value of node i is the probability that the input X belongs to class i. (All probability sums up to 1.)

The standard model for getting neural network to do multi-class classification uses what's called a **Softmax layer**:

$$ z^{[L]} = W^{[L]}a^{[L-1]} + b^{[L]}$$

(Softmax) Activation function:

$$ t = e^{z^{[L]}} ~ \text{(element wise)}$$

$$ \hat{y}_{i} = a^{[L]}_{i} = \frac{t_{i}}{\sum_{j=1}^{C}t_{j}}$$

### **Training a softmax classifier**

Softmax regression generalizes logistic regression to C classes. If C = 2, softmax reduces to logistic regression.

**Lost function**

$$ \it\unicode{xA3}(\hat{y}, y) = -\sum_{j=1}^{C}y_{j}log\hat{y}_{j}$$

**Cost function**

$$ J(W, b) = \frac{1}{m}\sum_{i=1}^{m}\it\unicode{xA3}(\hat{y}^{(i)}, y^{(i)})$$

The dimension of $$ \hat{y}, y$$ is `(C, m)` (stack the input horizontally).

## **Introduction to programming frameworks**

### **Deep learning frameworks**

* Caffe/Caffe 2
* CNTK
* DL4J
* Keras
* Lasagne
* mxnet
* PaddlePaddle
* TensorFlow
* Theano
* Torch

### **TensorFlow**

No notes.
