---
layout: post
title: "Week 2 - Neural Networks Basics"
date: 2017-10-10 16:20:00 +0700
# categories: ['machine learning', 'deep learning']
tags:
  - Coursera Deep Learning Course 1
comments: true
mathjax: true
content_level: 3
img: https://i.imgur.com/p8JojW8.png
summary: Convolutional Neural Networks notes
---

## **Logistic Regression as a Neural Network**

### **Binary Classification**

To store an image, the computer stores three separate matrices corresponding to the red, green, and blue color channels of the image.

{% include image.html
  url="https://i.imgur.com/Yt0iTte.png"
  cap="Figure 1. Three separate 5x4 matrices corresponding to three color channels (to store a 5x4 pixel image)."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

We can unroll the matrices to obtain an input features x.

By convention, let's call X is the nxm matrix obtained by stacking input x in column:

$$ X =  \begin{bmatrix}
x^{(1)} & ...  & x^{(m)}
\end{bmatrix}\in \mathbb{R}^{n \times m}$$

Similarly, matrix Y for the output y:

$$ Y =  \begin{bmatrix}
y^{(1)} & ...  & y^{(m)}
\end{bmatrix}\in \mathbb{R}^{1 \times m}$$

### **Logistic Regression**

Logistic Regression is used when the output labels y in supervised learning problem are all 0 or 1 (binary classification problem).

Given input x, we want:

$$ \hat{y} = P(y = 1 | x), ~0 \leq \hat{y} \leq 1$$

Parameters:

$$ w \in \mathbb{R}^{n_{X}}, ~b \in \mathbb{R}$$

We can predict $$ \hat{y}$$ as a linear function of input x:

$$ \hat{y} = w^{T}x + b$$

Actually this is what we do in linear regression, to ensure $$ 0 \leq \hat{y} \leq 1$$, we wrap it in the sigmoid function:

$$ \hat{y} = \sigma(w^{T}x + b) = \frac{1}{1 + e^{-(w^{T}x + b)}} $$

**When implement neural networks, it will be easier if we separate $$ w$$ and $$ b$$ (e.g. not unroll them into an unique parameter vector).**

### **Logistic Regression Cost Function**

Loss (error) function (defined with the respect to a single training example):

$$ \it\unicode{xA3} (\hat{y}, y) = -(ylog\hat{y} + (1 - y)log(1 - \hat{y}))$$

Cost function (the average of the loss functions of the entire training set):

$$ J(w, b) = \frac{1}{m} \sum_{i=1}^{m} \it\unicode{xA3} (\hat{y}^{(i)}, y^{(i)})$$

### **Gradient Descent**

$$ J(w, b)$$ is a convex function.

Update:

$$ w = w - \alpha \frac{dJ(w, b)}{dw}$$

$$ b = b - \alpha \frac{dJ(w, b)}{db}$$

### **Derivatives**

No notes.

### **More Derivative Examples**

No notes.

### **Computation Graph**

Say $$ f(x, y, z) = (x + y)z$$, to compute $$ f$$ we do 2 steps:

* Compute $$ q = x + y$$
* Compute $$ f = q * z$$

### **Derivatives with a Computation Graph**

Backpropagation is a training algorithm consisting of 2 steps:
* **Feed forward** the values: input values at the input layer and it travels from input to hidden and from hidden to output layer.

* Calculate the error (we can do this because this is supervised learning) and **propagate it back** to the earlier layers.

So, forward-propagation is part of the backpropagation algorithm but comes before back-propagating.

We can easily compute:

$$ \frac{\partial f}{\partial z} = q, ~ \frac{\partial f}{\partial q} = z$$

and:

$$ \frac{\partial q}{\partial x} = \frac{\partial q}{\partial y} = 1$$

However, we only care about the gradient of $$ f$$ with respect to its input $$ x, y, z$$. The **chain rule** tells us that the correct way to "chain" theses gradient expressions together is through multiplication, for example:

$$ \frac{\partial f}{\partial x} = \frac{\partial f}{\partial q}\frac{\partial q}{\partial x}$$

So how is this interpreted as Feedforward-propagation and Back-propagating?

{% highlight python linenos %}
# set some inputs
x = -2
y = 5
z = -4

# perform the forward pass
q = x + y  # q becomes 3
f = q * z  # f becomes -12

# perform the backward pass (backpropagation) in reverse order:
# first backprop through f = q * z
dfdz = q  # df/dz = q = 3
dfdq = z  # df/dq = z = -4
# now backprop through q = x + y
dfdx = 1.0 * dfdq  # The multiplication is the chain rule.
dfdy = 1.0 * dfdq
{% endhighlight %}

{% include image.html
  url="https://i.imgur.com/p8JojW8.png"
  cap="Figure 2. A circuit diagram visualizes the above computation."
  src_cap="CS231n Convolutional Neural Networks for Visual Recognition"
  src_url="http://cs231n.github.io/optimization-2/"
%}

The **forward pass** compute values (shown in green)  from inputs to outputs.

The **backward pass** then performs backpropagation which starts at then end and recursively applies the chain rule to compute the _local gradients_ of its inputs with respect to its output value.

Once the forward pass is over, during backpropagation the gate will eventually learn about the gradient of its output value on the final output of the entire circuit.

Chain rule says that the gate should take that gradient and multiply it into every gradient it normally computes for all of its inputs.

**One more example: Sigmoid function**

$$ f(w, x) = \frac{1}{1 + e^{-(w_{0}x_{0}+w_{1}x_{1}+w_{2})}}$$

{% include image.html
  url="https://i.imgur.com/pDmQ4l5.png"
  cap="Figure 3. Circuit diagram that visualizes the f(w, x) function, I added some variable names on some gates for computational illustration reasons."
  src_cap="CS231n Convolutional Neural Networks for Visual Recognition"
  src_url="http://cs231n.github.io/optimization-2/"
%}

According to the operators on the gates, we have these following relations:

$$
u = v + t \\
f = \frac{1}{p} \\
p = q + 1
$$

Some backpropagation calculations for intuition:

$$
\frac{\partial f}{\partial f} = 1 ~ (\text{at f = 0.73}) \\
\frac{\partial f}{\partial p} = -\frac{1}{p^{2}} = -0.53 ~ (\text{at p = 1.37}) \\
\frac{\partial f}{\partial q} = \frac{\partial f}{\partial p} * \frac{\partial p}{\partial q} = -0.53 * 1 = -0.53 ~ (\text{at q = 0.37})
$$

and:

$$ \frac{\partial f}{\partial v} = \frac{\partial f}{\partial u} \frac{\partial u}{\partial v} = 0.2 * 1 = 0.2 ~ (\text{at v = -2.0})$$

And the backprop for this neuron in code:

{% highlight python linenos %}
w = [2, -3, -3]  # assume some random weights and data
x = [-1, -2]

# forward pass
dot = w[0] * x[0] + w[1] * x[1] + w[2]
f = 1.0 / (1 + math.exp(-dot))  # sigmoid function

# backward pass
ddot = (1 - f) * f
dx = [w[0] * ddot, w[1] * ddot]
dw = [x[0] * ddot, x[1] * ddot, 1.0 * ddot]
{% endhighlight %}

### **Logistic Regression Gradient Descent**

Recap:

$$
z = w^{T}x + b \\
\hat{y} = a = \sigma(z) \\
\it\unicode{xA3}(a, y) = -(ylog(a) + (1 - y)log(1 - a))
$$

{% include image.html
  url="https://i.imgur.com/J0od0YO.png"
  cap="Figure 4. Computation graph to compute gradient of loss function with respect to a single training example."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

After finishing computing $$ dw_{1}, dw_{2}, db$$, we can perform the gradient descent update with respect to this single training example:

$$
w_{1} = w_{1} - \alpha ~ dw_{1} \\
w_{2} = w_{2} - \alpha ~ dw_{2} \\
b = b - \alpha ~ db
$$

### **Gradient Descent on m Examples**

$$
J(w, b) = \frac{1}{m}\sum_{i=1}^{m} \it\unicode{xA3}(a^{(i)}, y^{(i)}) \\
\frac{\partial J(w, b)}{\partial w_{i}} = \frac{1}{m}\sum_{i=1}^{m}\frac{\partial \it\unicode{xA3}(a^{(i)}, y^{(i)})}{\partial w_{i}}
$$

{% include image.html
  url="https://i.imgur.com/fp4aHAL.png"
  cap="Figure 5. Unvectorized algorithm to compute derivative."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}


## **Python and Vectorization**

### **Vectorization**

[ELI5: Why does vectorized code run faster than looped code?](https://www.reddit.com/r/explainlikeimfive/comments/4namsc/eli5why_does_vectorized_code_run_faster_than/)

[SIDM - Single Instruction, Multiple Data](https://en.wikipedia.org/wiki/SIMD)

### **More Vectorization Examples**

Neural network programming guideline:

**Whenever possible, avoid explicit for-loops.**

### **Vectorizing Logistic Regression**

No notes.

### **Vectorizing Logistic Regression's Gradient Ouput**

{% include image.html
  url="https://i.imgur.com/Xbh4Tdb.png"
  cap="Figure 6. Vectorizing implementation of Logistic Regression."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **Broadcasting in Python**

No notes.

### **A note on python/numpy vectors**

(n,): neither a column vector nor a row vector (rank 1 array in Python).

When programming neural network, DO NOT USE RANK 1 ARRAY.

assert(a.shape == (5, 1)).

### **Quick tour of Jupyter/iPython Notebooks**

No notes.

### **Explanation of logistic regression cost function**

No notes.

## **References**

[1] [Backpropagation Intuition - CS231n Convolutional Neural Networks for Visual Recognition
](http://cs231n.github.io/optimization-2/)
