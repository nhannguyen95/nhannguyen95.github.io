---
layout: post
title:  "Coursera Deep Learning Course 1 Week 4 notes: Deep neural networks"
date: 2017-10-16 14:59:00 +0700
tags:
  - notes
  - deep learning
no-post-nav: 0
comments: 1
---

##### **Deep Neural Network**

###### **Deep L-layer neural network**

The word 'deep' refers to the number of layer of a neural network (not count the input layer).

Some notations:

* $$ L$$ = number of layer
* $$ n^{[l]}$$ = number of units in layer l ($$ n_{X} = n^{[0]})$$
* $$ a^{[l]}$$ = activations in layer l ($$ X = a^{[0]}, ~\hat{y} = a^{[L]}, ~a^{[l]} = g^{[l]}(z^{[l]})$$).

###### **Forward Propagation in a Deep Network**

$$ z^{[l]} = w^{[l]}a^{[l-1]} + b^{[l]}$$

$$ a^{[l]} = g^{[l]}(z^{[l]})$$

Vectorization for whole training set (stack lowercase matrices in column to obtain capital matrices):

$$ Z^{[l]} = W^{[l]}A^{[l-1]} + B^{[l]}$$

$$ A^{[l]} = g^{[l]}(Z^{[l]})$$

We can't avoid having a for loop iterating over all layers.

###### **Getting your matrix dimensions right**

No notes.

###### **Why deep representing**

No notes.

###### **Building blocks of deep neural networks**

<hr>
<center><a href="https://i.imgur.com/FbjRdQW.png" target="_blank">
<img width="600" src="https://i.imgur.com/FbjRdQW.png"/></a>
</center><center>Figure 1. Forward and backward implementation.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

###### **Forward and Backward Propagation**

**Forward propagation for layer l**

Input $$ a^{[l - 1]}$$

Output $$ a^{[l]}$$, cache($$ z^{[l]}, ~w^{[l]}, ~b^{[l]}$$)

* $$ z^{[l]} = w^{[l]}a^{[l - 1]} + b^{[l]}$$.

* $$ a^{[l]} = g^{[l]}(z^{[l]})$$.

**Backward propagation for layer l**

Input $$ da^{[l]}$$

Output $$ da^{[l - 1]}, ~dW^{[l]}, ~db^{[l]}$$

* $$ dz^{[l]} = da^{[l]} * {g^{[l]}}'(z^{[l]})$$.

* $$ dW^{[l]} = dz^{[l]}a^{[l - 1]}$$.

* $$ db^{[l]} = dz^{[l]}$$.

* $$ da^{[l - 1]} = W^{[l]}dz^{[l]}$$.

###### **Parameters vs Hyperameters**

**Parameters**: $$ W^{[l]}, ~b^{[l]}$$.

**Hyperparameters**: control parameters.
* Learning rate $$ \alpha$$.
* Number of iterations.
* Number of hidden layers L.
* Number of hidden units.
* Choice of activation functions.
* Many more...

Therefore, **applying deep learning is a very empirical process**.

###### **What does this have to do with the brain?**
