---
layout: post
title:  "Week 2 - Deep convolutional models: case studies"
date: 2018-01-04 13:34:00 +0700
tags:
  - notes
  - deep learning
comments: true
mathjax: true
content_level: 3
img: /images/cnn/residual-net-paper.png
summary: Convolutional Neural Networks notes
---

## **Case studies**

### **Why look at case studies?**

Some classic networks:
* LeNet-5
* AlexNet
* VGG

The idea of these might be useful for your own work.

* ResNet (conv residual network): very deep network - 152 layers.

* Inception neural network

### **Classic Networks**

#### **LeNet - 5**

{% include image.html
  url="https://i.imgur.com/Uz9jML4.png"
  cap="Figure 1. LeNet - 5 network architecture."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

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

#### **AlexNet**

{% include image.html
  url="https://i.imgur.com/ucPEwqk.png"
  cap="Figure 2. AlexNet network architecture."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

Has a lot of similarity to LeNet - 5, but much bigger.

60M parameters.

Used Relu activation function.

AlexNet paper reading nots:

* When the paper was written, GPUs slow: using multiple GPUs.
* There was another set of a layer: Local Response Normalization: normalize (i, j, :) - not used much today.

#### **VGG - 16**

To avoid having too many hyperparameters:
* All CONV = 3x3 filter, s = 1, same padding.
* All MAX-POOL = 2x2, s = 2.

{% include image.html
  url="https://i.imgur.com/QIe13FP.png"
  cap="Figure 3. VGG - 16 network architecture."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

Downside: a large network to train - 138M parameters.

16 layers that have weights.

#### **VGG - 19**

A bigger version of VGG - 16.

### **ResNets (Residual Network)**

Very deep networks are difficult to train because of vanishing and exploding gradient types of problems.

ResNet enables you to train very deep networks.

_Read more in this week's Residual Network assignment._

#### **Residual block**

_Read more in this week's Residual Network assignment._

### **Why ResNets Work**

Taking notes later..

### **Networks in Networks and 1x1 Convolutions**

{% include image.html
  url="https://i.imgur.com/7ioqzYt.png"
  cap="Figure 4. 1x1 convolutions with 1-channel volumn and multi-channel volumn."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

With 1-channel volumn: 1x1 convolutions is simply just multiplying a matrix with a scalar.

With multi-channel volumn: 1x1 convolutions make more sense - it is a fully connected neural layer for each (h, w, :) position:
* Input is of 32
* Output is of number of filter (each filter plays a role as a single node of the neural layer)

That's the reason why 1x1 convolutions are also called **Network in Network**.

Although the idea of 1x1 convolutions wasn't used widely, it influenced many other neural network architectures.

#### **Using 1x1 convolutions**

When number of channels is too big, you can shrink that using some CONV 1x1 filters.

### **Inception Network Motivation**

{% include image.html
  url="https://i.imgur.com/cis8tEh.png"
  cap="Figure 5. One inception module inputs 28x28x192 and outputs 28x28x256."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

Idea: Instead of picking what filter/pooling to use, just do them all, and concat all the output.

Problem with inception layer: computational cost, for example to compute the output block of 5x5 filter, need `28x28x32 x 5x5x192 = 120M` multiplication.

To reduce (by a factor of 10): use 1x1 convolutions.

#### **Using 1x1 convolution**

{% include image.html
  url="https://i.imgur.com/JKw79YE.png"
  cap="Figure 6. Use a 1x1 convolution (bottle neck) layer to reduce/shrink the number of channel first."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

The cost now = `28x28x16 x 1x1x192 + 28x28x32 x 5x5x32 = 12M`

Question: Does shrinking down the number of channel dramatically hurt the performance? - It doesn't seem to hurt.

### **Inception Network**

{% include image.html
  url="https://i.imgur.com/qhPZfZQ.png"
  cap="Figure 7. One Inception module."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

{% include image.html
  url="https://i.imgur.com/QCvVQxo.png"
  cap="Figure 8. An Inception network."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

## **Practical advices for using ConvNets**

### **Using Open-Source Implementation**

No notes.

### **Transfer learning**

#### **Your training set is small**

{% include image.html
  url="https://i.imgur.com/yXRM4yc.png"
  cap="Figure 9. Transfer Learning with small traning set (just change the softmax layer)."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

#### **Your training set is large**

{% include image.html
  url="https://i.imgur.com/SXo7qXx.png"
  cap="Figure 10. With large traning set: freeze a few layer, train the rest (or use your own neural network)."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

#### **Your training set is very large**

Use the trained weights as initialization.

### **Data Augmentation**

Common augmentation method:
* Mirroring
* Random cropping
* Rotation
* Shearing
* Local warping
* Color shifting: PCA (Principles Component Analysis) Color Augmentation algorithm.

Implementing distortions during training:

{% include image.html
  url="https://i.imgur.com/ZEQiqeP.png"
  cap="Figure 11. Using threads to distort data and some other threads to train (maybe in parallel)."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **State of Computer Vision**

{% include image.html
  url="https://i.imgur.com/dmLHBA9.png"
  cap="Figure 12. Data vs. hand-engineering."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

Machine Learning problems have two sources of knowledge:
* Labeled data.
* Hand engineering features/network architecture/other components.

In computer vision, because of the absence of more data, need to focus on (complex) network architecture.

When you don't have enough data, hand-engineering is a very difficult, very skillful task that requires a lot of insight.

#### **Tips for doing well on benchmarks/winning competitions**
(_Rarely used in real production/products/services to serve customers._)

* Ensembling:
  * Train several networks independently and average their outputs (y hat).
* Multi-crop at test time:
  * Run classifier on multiple versions of test images and average results. (10-crop algorithm)

#### **Use open source code**

* Use architectures of networks published in the literature.

* Use open source implementation if possible.

* Use pretrained models and fine-tune on your dataset.
