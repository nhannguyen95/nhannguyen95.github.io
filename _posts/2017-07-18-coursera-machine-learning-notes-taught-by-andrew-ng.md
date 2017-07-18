---
layout: post
title:  "Coursera Machine Learning Notes (taught by Andrew Ng)"
date: 2017-07-18 14:22:00 +0700
categories:
tags:
no-post-nav: 0
comments: 1
---

##### **Week 5: Neural Networks: Learning**

###### **Cost Function**

###### **Backpropagation Algorithm**
As usual, to minimize $$ J(\theta)$$ using Gradient Descent, we need to compute $$ J(\theta)$$ itself and:

$$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)} }$$

As it's hard to solve this formula directly; we can modify it using a trick, which is applying the [Chain rule] [chain-rule]:

$$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)} } = \frac{\partial J(\theta)}{\partial z_{i}^{(l+1)} } ~  \frac{\partial z_{i}^{(l+1)}}{\partial \theta_{ij}^{(l)} } ~~~~~(1) $$

The second factor can be modified as:

$$ \frac{\partial z_{i}^{(l+1)}}{\partial \theta_{ij}^{(l)} } = \frac{\partial \sum_{j=0}^{s_{l}}\theta_{ij}^{(l)}a_{j}^{(l)}}{\partial \theta_{ij}^{(l)} } = a_{j}^{(l)}$$

with $$ s_{l}$$ is the number of units in $$ l$$-th layer (not counting bias unit), we iterate the sum from 0 including the bias unit.

And then we define a new value - "error" of node $$ i$$ in layer $$ l$$:

$$ \delta _{i}^{(l)} = \frac{\partial J(\theta)}{\partial z_{i}^{(l)} }$$

So the formula $$ (1)$$ can be re-written as:

$$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)} } = a_{j}^{(l)}\delta_{i}^{(l+1)}$$

So now we need to compute $$ \delta_{i}^{(l)}$$ for all $$ i, l$$; and there is a recursive formula that helps us do this:

$$ \delta_{i}^{(l)}=\sum_{k=1}^{s_{l+1}}\delta_k^{(l+1)}\theta_{ki}^{(l)}g'(z_{i}^{(l)}) ~~~~~ (2)$$

$$ =\sum_{k=1}^{s_{l+1}}\delta_k^{(l+1)}\theta_{ki}^{(l)}g(z_i^{(l)})(1-g(z_i^{(l)}))$$

$$ =\sum_{k=1}^{s_{l+1}}\delta_k^{(l+1)}\theta_{ki}^{(l)}a_i^{(l)}(1-a_i^{(l)})$$

with $$ g(z) = \frac{1}{1+e^{-z}}$$ is the [sigmoid function] [sigmoid-function], hence $$ g'(z) = g(z)(1-g(z)) $$

And the vectorization form of it:

$$ \delta^{(l)}=(\theta^{(l)})^{T}\delta^{(l+1)}.*g'(z^{l})$$

$$ =(\theta^{(l)})^{T}\delta^{(l+1)}.*a^{(l)}.*(1-a^{(l)})$$

with .* is the element-by-element (element wise) multiplication operation.

As we can observe: in order to calculate $$ \delta$$ in the current layer ($$ l$$), we need to know $$ \delta$$ of its next layer ($$ l+1$$, and hence the name Backpropagation); and the starting point of this recursive formula is the $$ \delta$$ of the output layer ($$ \delta^{(L)}$$). Now let's compute $$ \delta$$ for output layer:

At output layer, we have $$ l = L$$:

$$ \delta_{i}^{(L)}=\frac{\partial J(\theta)}{\partial z_{i}^{(L)}}$$

As we're using Stochastic Gradient Descent (SGD) method (this algorithm will update the $$ \theta$$ matrix for each training example), $$ J(\theta)$$ now will be the cost function with respect to a single training example $$ (x, y)$$ (differentiate this from the overall cost function):

$$ J(\theta) = J(\theta, x, y) = -ylog(h_{\theta}(x))-(1-y)log(1-h_{\theta}(x)) ~~~~~ (3)$$

Again, at the output layer, we have:

$$ h_{\theta}(x)=a^{(L)}=g(z^{(L)})=\frac{1}{1+e^{-z^{(L)}}}$$

Substitute this into $$ (3)$$, and then take derivative:

$$ \frac{\partial J(\theta)}{\partial z^{(L)}}=h_{\theta}(x) - y$$

As a result:

$$ \delta_{i}^{(L)}=\frac{\partial J(\theta)}{\partial z_{i}^{(L)}}=(h_{\theta}(x))_{i} - y_{i}$$

Now we will prove $$ (2)$$:

$$ \delta _{i}^{(l)} = \frac{\partial J(\theta)}{\partial z_{i}^{(l)} }=\sum_{k=1}^{s_{l+1}}\frac{\partial J(\theta)}{\partial z_k^{(l+1)}}\frac{\partial z_k^{(l+1)}}{\partial z_i^{(l)}}$$

$$=\sum_{k=1}^{s_{l+1}}\delta_k^{(l+1)}\frac{\partial z_k^{(l+1)}}{\partial z_i^{(l)}} ~~~~~ (4)$$

Because we have:

$$ z_k^{(l+1)} = \sum_{j=0}^{s_l}\theta_{kj}^{(l)}a_j^{(l)}$$

so:

$$ \frac{\partial z_k^{(l+1)}}{\partial z_i^{(l)}}=\frac{\partial \sum_{j=0}^{s_l}\theta_{kj}^{(l)}a_j^{(l)}}{\partial z_i^{(l)}}=\sum_{j=0}^{s_l}\theta_{kj}^{(l)}\frac{\partial a_j^{(l)}}{\partial z_i^{}(l)}$$

$$ =\sum_{j=0}^{s_l}\theta_{kj}^{(l)}\frac{\partial g(z_j^{(l)})}{\partial z_i^{}(l)}=\theta_{ki}^{(l)}\frac{\partial g(z_i^{(l)})}{\partial z_i^{}(l)}=\theta_{ki}^{(l)}g'(z_i^{(l)})$$

Substitute this into (4):

$$ \delta _{i}^{(l)}=\sum_{k=1}^{s_{l+1}}\delta_k^{(l+1)}\theta_{ki}^{(l)}g'(z_i^{(l)})$$

Backpropagation algorithm:
* Training set $$ {(x^{(1)}, y^{(1)}), .., (x^{(m)}, y^{(m)})}$$
* Set $$ \Delta_{ij}^{(l)}=0$$ (for all i, j, l).
* For i = 1 to m
  * Set $$ a^{(1)}=x^{(i)}$$
  * Perform forward propagation to compute $$ a^{(l)}$$ for l = 2, 3.., L
  * Using $$ y^{(i)}$$, compute $$ \delta^{(L)}=a^{(L)}-y^{(i)}$$
  * Compute $$ \delta^{(L-1)},..,\delta^{(2)}$$
  * Update: $$ \Delta_{ij}^{(l)}~+=~a_j^{(l)}\delta_i^{(l+1)}$$
* If $$ j\neq0$$ then $$ D_{ij}^{(l)}=\frac{1}{m}\Delta_{ij}^{(l)} + \lambda\theta_{ij}^{(l)}$$
* If $$ j=0$$ then $$ D_{ij}^{(l)}=\frac{1}{m}\Delta_{ij}^{(l)}$$

And we have:

$$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)} }=D_{ij}^{(l)}$$

###### **Backpropagation intuition

##### **References**
[1] [Backpropagation Algorithm]( http://ufldl.stanford.edu/wiki/index.php/Backpropagation_Algorithm)

[2] [How to derive errors in neural network with the backpropagation algorithm?](https://stats.stackexchange.com/questions/94387/how-to-derive-errors-in-neural-network-with-the-backpropagation-algorithm)

[chain-rule]: https://en.wikipedia.org/wiki/Chain_rule
[sigmoid-function]: https://en.wikipedia.org/wiki/Sigmoid_function
