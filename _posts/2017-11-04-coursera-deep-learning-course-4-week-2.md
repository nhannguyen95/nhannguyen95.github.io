---
layout: post
title:  "Coursera Deep Learning Course 4 Week 2 notes: Deep convolutional models: case studies"
date: 2018-01-04 13:34:00 +0700
categories: ['machine learning', 'deep learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Case studies**

###### **Why look at case studies?**

Some classic networks:
* LeNet-5
* AlexNet
* VGG

The idea of these might be useful for your own work.

* ResNet (conv residual network): very deep network - 152 layers.

* Inception neural network

###### **Classic Networks**

**LeNet - 5**

<hr>
<center><a href="https://i.imgur.com/Uz9jML4.png" target="_blank">
<img width="600" src="https://i.imgur.com/Uz9jML4.png"/></a>
</center><center>Figure 1. LeNet - 5 network architecture.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Goal: recognize hand-written digits.

Trained on grayscale images (1 channel).

60K parameters.

As you go deeper into the network: width, height do down; number of channels increase.

One (or sometimes more than one) conv layer followed by a pooling layer, this arrangement is quite common.

Original LeNet - 5 paper reading note:

* Back then: people use sigmoid, tanh (not relu)
* To reduce computational cost (back then): there are different filters look at different channels of the input.
* There was non-linearity after pooling.
* Should focus on section 2 & 3.

**AlexNet**

<hr>
<center><a href="https://i.imgur.com/ucPEwqk.png" target="_blank">
<img width="600" src="https://i.imgur.com/ucPEwqk.png"/></a>
</center><center>Figure 2. AlexNet network architecture.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Has a lot of similarity to LeNet - 5, but much bigger.

60M parameters.

Used Relu activation function.

AlexNet paper reading nots:

* When the paper was written, GPUs slow: using multiple GPUs.
* There was another set of a layer: Local Response Normalization: normalize (i, j, :) - not used much today.

**VGG - 16**

To avoid having too many hyperparameters:
* All CONV = 3x3 filter, s = 1, same padding.
* All MAX-POOL = 2x2, s = 2.

<hr>
<center><a href="https://i.imgur.com/QIe13FP.png" target="_blank">
<img width="600" src="https://i.imgur.com/QIe13FP.png"/></a>
</center><center>Figure 3. VGG - 16 network architecture.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Downside: a large network to train - 138M parameters.

16 layers that have weights.

**VGG - 19**

A bigger version of VGG - 16.

###### **ResNets (Residual Network)**

Very deep networks are difficult to train because of vanishing and exploding gradient types of problems.

ResNet enables you to train very deep networks.

_Read more in this week's Residual Network assignment._

**Residual block**

_Read more in this week's Residual Network assignment._

###### **Why ResNets Work**

Taking notes later..

###### **Networks in Networks and 1x1 Convolutions**

<hr>
<center><a href="https://i.imgur.com/QIe13FP.png" target="_blank">
<img width="600" src="https://i.imgur.com/QIe13FP.png"/></a>
</center><center>Figure 4. 1x1 convolutions with 1-channel volumn and multi-channel volumn.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

With 1-channel volumn: 1x1 convolutions is simply just multiplying a matrix with a scalar.

With multi-channel volumn: 1x1 convolutions make more sense - it is a fully connected neural layer for each (h, w, :) position:
* Input is of 32
* Output is of number of filter (each filter plays a role as a single node of the neural layer)

That's the reason why 1x1 convolutions are also called **Network in Network**.

Although the idea of 1x1 convolutions wasn't used widely, it influenced many other neural network architectures.

**Using 1x1 convolutions**

When number of channels is too big, you can shrink that using some CONV 1x1 filters.

###### **Inception Network Motivation**

<hr>
<center><a href="https://i.imgur.com/cis8tEh.png" target="_blank">
<img width="600" src="https://i.imgur.com/cis8tEh.png"/></a>
</center><center>Figure 5. One inception module inputs 28x28x192 and outputs 28x28x256.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Idea: Instead of picking what filter/pooling to use, just do them all, and concat all the output.

Problem with inception layer: computational cost, for example to compute the output block of 5x5 filter, need `28x28x32 x 5x5x192 = 120M` multiplication.

To reduce (by a factor of 10): use 1x1 convolutions.

**Using 1x1 comvolution**

<hr>
<center><a href="https://i.imgur.com/JKw79YE.png" target="_blank">
<img width="600" src="https://i.imgur.com/JKw79YE.png"/></a>
</center><center>Figure 6. Use a 1x1 convolution (bottle neck) layer to reduce/shrink the number of channel first.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

The cost now = `28x28x16 x 1x1x192 + 28x28x32 x 5x5x32 = 12M`

Question: Does shrinking down the number of channel dramatically hurt the performance? - It doesn't seem to hurt.

###### **Inception Network**

<hr>
<center><a href="https://i.imgur.com/qhPZfZQ.png" target="_blank">
<img width="600" src="https://i.imgur.com/qhPZfZQ.png"/></a>
</center><center>Figure 7. One Inception module.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

<center><a href="https://i.imgur.com/QCvVQxo.png" target="_blank">
<img width="600" src="https://i.imgur.com/QCvVQxo.png"/></a>
</center><center>Figure 8. An Inception network.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

##### **Practical advices for using ConvNets**

###### **Using Open-Source Implementation**

No notes.

###### **Transfer learning**

**Your training set is small**

<hr>
<center><a href="https://i.imgur.com/yXRM4yc.png" target="_blank">
<img width="600" src="https://i.imgur.com/yXRM4yc.png"/></a>
</center><center>Figure 10. Transfer Learning with small traning set (just change the softmax layer).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

**Your training set is large**

<hr>
<center><a href="https://i.imgur.com/SXo7qXx.png" target="_blank">
<img width="600" src="https://i.imgur.com/SXo7qXx.png"/></a>
</center><center>Figure 11. With large traning set: freeze a few layer, train the rest (or use your own neural network).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

**Your training set is very large**

Use the trained weights as initialization.

###### **Data Augmentation**

Common augmentation method:
* Mirroring
* Random cropping
* Rotation
* Shearing
* Local warping
* Color shifting: PCA (Principles Component Analysis) Color Augmentation algorithm.

Implementing distortions during training

<hr>
<center><a href="https://i.imgur.com/ZEQiqeP.png" target="_blank">
<img width="600" src="https://i.imgur.com/ZEQiqeP.png"/></a>
</center><center>Figure 12. Using threads to distort data and some other threads to train (maybe in parallel).</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

###### **State of Computer Vision**

<hr>
<center><a href="https://i.imgur.com/dmLHBA9.png" target="_blank">
<img width="600" src="https://i.imgur.com/dmLHBA9.png"/></a>
</center><center>Figure 13. Data vs. hand-engineering.</center>
<center>
<i><a href="https://www.coursera.org/learn/neural-networks-deep-learning">Source: Coursera Deep Learning course</a></i></center>
<hr>

Machine Learning problems have two sources of knowledge:
* Labeled data.
* Hand engineering features/network architecture/other components.

In computer vision, because of the absence of more data, need to focus on (complex) network architecture.

When you don't have enough data, hand-engineering is a very difficult, very skillful task that requires a lot of insight.

**Tips for doing well on benchmarks/winning competitions**
(_Rarely used in real production/products/services to serve customers._)

* Ensembling:
  * Train several networks independently and average their outputs (y hat).
* Multi-crop at test time:
  * Run classifier on multiple versions of test images and average results. (10-crop algorithm)

**Use open source code**

* Use architectures of networks published in the literature.

* Use open source implementation if possible.

* Use pretrained models and fine-tune on your dataset.
