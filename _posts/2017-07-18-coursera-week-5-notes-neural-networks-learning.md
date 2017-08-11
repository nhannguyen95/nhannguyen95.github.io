---
layout: post
title:  "Week 5 notes: Neural Networks: Learning"
date: 2017-07-18 14:22:00 +0700
categories: ['machine learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Introduction**
I think week 5 is the hardest week so far, I had to do tons of research on Backpropagation in order to fully understand it. It was really great when I eventually answered all the questions that came to mind.

This note is my attempt to explain the formulas appear in the week 5's videos, hope it helps you to understand it too.


##### **Cost Function and Backpropagation**

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
* If $$ j\neq0$$ then $$ D_{ij}^{(l)}=\frac{1}{m}\Delta_{ij}^{(l)} + \frac{\lambda}{m}\theta_{ij}^{(l)}$$
* If $$ j=0$$ then $$ D_{ij}^{(l)}=\frac{1}{m}\Delta_{ij}^{(l)}$$

And we have:

$$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)} }=D_{ij}^{(l)}$$

###### **Backpropagation intuition**

There is no $$ \delta$$ value for bias units (actually we can still compute it but no need to use it).

Backpropagation is totally opposite to Feedforward propagation, with each training example $$ (x, y)$$:
* Feedforward propagation:
  * input: $$ a^{(1)}=x$$
  * calculate $$ a^{(l)}$$ using $$ a^{(l-1)}$$
* Backpropagation:
  * input: $$ \delta^{(L)}=h_{\theta}(x)-y$$
  * calculate $$ \delta^{(l)}$$ using $$ \delta^{(l+1)}$$

and the weight matrices ($$ \theta$$ matrices) are used similarly in two algorithms.

##### **Backpropagation in Practice**

###### **Implementation Note: Unrolling Parameters**

Previously to optimize the cost function of Linear Regression and Logistic Regression, in Octave we do:

{% highlight matlab %}
function [jVal, gradient] = costFunction(theta)
...
optTheta = fminunc(@costFunction, initialTheta, options)
{% endhighlight %}

with $$ theta$$ and $$ gradient$$ are vectors.

Now in Neural Networks, they are not vectors anymore (but matrices)=> we "unroll" them into vectors.

For example, if we have:

$$ \theta^{(1)}\in\mathbb{R}^{10×11}$$

$$ \theta^{(2)}\in\mathbb{R}^{8×1}$$

and we want to put them in a single vector, in Octave we can do this:

{% highlight matlab %}
thetaVec = [ theta1(:) ; theta2(:)];
{% endhighlight %}

$$ theta1(:)$$: put all theta1's columns into 1 single column vector.
And to get back:

{% highlight matlab %}
theta1 = reshape(thetaVec(1:110), 10, 11);
theta2 = reshape(thetaVec(111:118), 8, 1);
{% endhighlight %}

###### **Gradient checking**

Two sides difference estimate:

$$ gradApprox=\frac{dJ(\theta)}{d\theta}=\frac{J(\theta+\epsilon)-J(\theta-\epsilon)}{2\epsilon}$$

this will give us a numerical estimate of the gradient at that point.

In case $$ \theta$$ is a vector:

$$ \frac{\partial J(\theta)}{\partial \theta_i}=\frac{J(..,\theta_i+\epsilon,..)-J(..,\theta_i-\epsilon,..)}{2\epsilon}$$

In implementation (again, $$ \theta$$ is a vector unrolled from weight matrices):

{% highlight matlab %}
for i = 1:n,
  thetaPlus = theta;
  thetaPlus(i) = thetaPlus(i) + EPSILON;
  thetaMinus = theta;
  thetaMinus(i) = thetaMinus(i) - EPSILON;
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*EPSILON);
end;
{% endhighlight %}

and then check that $$ gradApprox \approx DVec$$, with $$ DVec$$ is the derivative we got from Backpropagation.

**Implementation Note**:
* Implement backprop to comput DVec.
* Implement numerial gradient check to compute gradApprox.
* Make sure they give similar values.
* Turn off gradient checking. Use backprop code (DVec) for learning.

**Important**:
* Be sure to disable your gradient checking code before training your classifier. If you run numerical gradient computation on every iteration of gradient descent (or in the inner loop of *costFunction(...)*), your code will be __very slow__ (because the gradient checking code is very computationally expensive).

The main reason why we use the backprop algorithm rather than the numerical gradient computation method during learning is that the numerical gradient algorithm is very slow (and the backprop one is much more efficient).

###### **Random Initialization**

In Neural Networks, initializing all $$ \theta$$ values as 0 (or a same value) doesn't work. Because after each update, parameters corresponding to inputs going into each hidden units (headed units) are identical => **highly redundant representation** => prevents the network from doing something interesting.

Solution: **Random Initialization (symmetry breaking)**.

Initialize each $$ \theta$$ to a random value in $$ [-\epsilon, \epsilon]$$:

{% highlight matlab %}
theta1 = rand(10, 11) * (2*INIT_EPSILON) - INIT_EPSILON;
theta1 = rand(8, 1) * (2*INIT_EPSILON) - INIT_EPSILON;
{% endhighlight %}

The $$ \epsilon$$ here has nothing to do with the $$ \epsilon$$ used in gradient checking.

One effective strategy for choosing $$ \epsilon_{init}$$ is to base it on the number of units in the network. A good choice of $$ \epsilon_{init}$$ is:

$$ \epsilon_{init} = \frac{\sqrt{6}}{\sqrt{L_{in} + L_{out}}}$$

where $$ L_{in} = s_{l}$$ and $$ L_{out} = s_{l+1}$$ are the number of units in the layers adjacent to $$ \theta^{(l)}$$.

###### **Putting it together**

The first thing we need to do when training a networks: **pick some network architecture**. => how many hidden layers, how many units in each layer...

Number of input units: Dimension of feature $$ x^{(i)}$$.

Number of output units: Number of classes.

Reasonable default:
* 1 hidden layer (most common).
* if >1 hidden layer, have same number of hidden units in every layer (usually the more the better). And also the number of hidden units is usually comparable to the dimension of x (number of features): 3 or 4 times of that (or greater).

**Steps to train a neural networks**:
* Randomly initilize weights (usually small value near 0).
* Implement forward propagation to get $$ h_{\theta}(x^{(i)})$$ for any $$ x^{(i)}$$
* Implement code to compute cost function $$ J(\theta)$$.
* Implement backprop to compute partial derivatives $$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)}}$$.

(*In implementation, we use a for loop:*

{% highlight matlab %}
for i = 1:m
  % perform forward propagation and backpropagation using example (x(i), y(i))
  % (get activation and delta term for each layer)
{% endhighlight %}
)

* Use gradient checking to compare $$ \frac{\partial J(\theta)}{\partial \theta_{ij}^{(l)}}$$ computed using backpropagation vs. using numerical estimate of gradient of $$ J(\theta)$$.
Then disable gradient checking code.
* Use gradient descent or advanced optimization method with backpropagation to try to minimize $$ J(\theta)$$ as a function of parameters $$ \theta$$.

In Neural Networks, $$ J(\theta)$$ is a non-convex function; so gradient descent algorithm can get stuck in local minima. In practice, this is not a huge problem; because it can find a good local optima if it doesn't even get to the global one.

###### **Application of Neural Networks**

No notes.

##### **Application of Neural Networks**

##### **References**
[1] [Backpropagation Algorithm]( http://ufldl.stanford.edu/wiki/index.php/Backpropagation_Algorithm)

[2] [How to derive errors in neural network with the backpropagation algorithm?](https://stats.stackexchange.com/questions/94387/how-to-derive-errors-in-neural-network-with-the-backpropagation-algorithm)

[chain-rule]: https://en.wikipedia.org/wiki/Chain_rule
[sigmoid-function]: https://en.wikipedia.org/wiki/Sigmoid_function
