---
layout: post
title: "Week 4 - Special applications: Face recognition & Neural style transfer"
date: 2018-01-24 14:14:00 +0700
tags:
  - notes
  - deep learning
comments: true
mathjax: true
content_level: 3
img: https://i.imgur.com/jWpIK11.png
summary: Convolutional Neural Networks notes
---

## **Face Recognition**

### **What is face recognition?**

Face verification: Given an input image and a person ID, output if the image is that of the claimed person.

Face recognition: Given an input image and K persons, output the ID if the image is any of the K persons (or "not recognized").

Face recognition is harder than face verification, to apply face verification into face recognition, you may need a high accuracy verification system.

### **One Shot Learning**

One shot learning problem: learning from only one picture (example) to recognize the person again.

Normal way: feed (forward) the image into a CNN to predict:
* This doesn't really work well because the training set is (too) small to train a robust network.
* Furthermore, whenever there is one more new person in the training data, you have to change the output of the CNN over again.

Solution: Learning a "similarity" function:

$d(\text{img1, img2}) = \text{degree of difference between}$

If $d(\text{img1, img2}) \le \tau$, same person.

If $d(\text{img1, img2}) \gt \tau$, different person.

### **Siamese Network**

Normally when given an image $x^{(1)}$, you input it to a ConvNet then take the softmax output layer to classify it.

In Siamese Network, you remove this softmax output layer and only consider the last non-output fully connected layer (say it has 128 activation units). Think this layer as an "encoding of $x^{(1)}$ (or: the parameters of NN define an encoding of $x^{(1)}$), and let's call it $f(x^{(1)})$.

The similarity function (between two images $x^{(1)}$ and $x^{(2)}$) is defined as:

$$d(x^{(1)}, x^{(2)}) = \left \| f(x^{(1)})-f(x^{(2)}) \right \|_ {2}^{2}$$

How about the cost function to train this kind of NN? Find out in the next lesson.

### **Triplet Loss**

**Learning objective**

We will look at 3 images at a time:
* Anchor image (A)
* Positive image (N): a picture of the same person as in Anchor image
* Negative image (P): a picture of a different person than in Anchor image

Our learning objective:

$$\left \| f(A)-f(P) \right \|^{2} \le \left \| f(A)-f(N) \right \|^{2}$$
$$\Leftrightarrow \left \| f(A)-f(P) \right \|^{2} - \left \| f(A)-f(N) \right \|^{2} \le 0$$

To prevent $f(x) = 0$, we add a margin hyperparameter $\alpha$:

$$\left \| f(A)-f(P) \right \|^{2} - \left \| f(A)-f(N) \right \|^{2} + \alpha \le 0$$

**Loss function**

Given 3 images A, P, N; our loss function is defined:

$$ \it\unicode{xA3}(A, P, N) = max(\left \| f(A)-f(P) \right \|^{2} - \left \| f(A)-f(N) \right \|^{2} + \alpha, 0)$$

And the cost function:

$$ J = \sum_{i=1}^{m}\it\unicode{xA3}(A^{(i)}, P^{(i)}, N^{(i)})$$

So say if you have a training set with 10k pictures of 1k persons, you need to generate triples from that 10k pictures.

You do need your training set to contain multiple pictures of the same person (because of the present of A, P).

But after having trained the system, you can apply it to your one shot learning problem.

**Choosing the triplets A, P, N**

During training; if A, P, N are chosen randomly, $d(A,P) + \alpha \le d(A, N)$ is easily satisfied. And your NN won't learn much from this.

You want to choose triplets that're "hard" to train on: $d(A,P)$ is close to $d(A,N)$.

Detail: 2nd mentioned paper .

{% include image.html
  url="https://i.imgur.com/jWpIK11.png"
  cap="Figure 1. Training set using triplet loss."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **Face Verification and Binary Classification**

Treat face recognition as a binary classification problem (also an alternative to the triplet loss for training a system like this):

{% include image.html
  url="https://i.imgur.com/IISy8S6.png"
  cap="Figure 2. The new network is composed of 2 Siamese network (with the same parameters). Input: a pair of image, output: binary value y (y = 1 if same person and otherwise)"
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

The training set is now:
* Input: a pair of image.
* Output: 1 if same person and otherwise.

## **Neural Style Transfer**

### **What is neural style transfer?**

{% include image.html
  url="https://i.imgur.com/CBSPqXj.png"
  cap="Figure 3. One picture is worth a thousand words"
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **What are deep ConvNets learning?**

Pick a unit in layer (say) 1 of CNN, find (say) 9 image patches that maximize the unit's activation.

Repeat for other units, this is what you can get:

{% include image.html
  url="https://i.imgur.com/HgFnwvg.png"
  cap="Figure 4. This gives you a sense that trained hidden units in layer 1 looks for relatively simple features such as edge or a particular shade of color."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

What if we do this for deeper layers:

{% include image.html
  url="https://i.imgur.com/GwunwDG.png"
  cap="Figure 5. The deeper the layers, the more complicated features and patters they detect."
  src_cap="Coursera Deep Learning course"
  src_url="https://www.coursera.org/learn/neural-networks-deep-learning"
%}

### **Cost Function**

To build a Neural Style Transfer system, let's define a cost function for the generated image:

$J(G) = \alpha J_{content}(C, G) + \beta J_{style}(S, G)$

$J_{content}(C, G)$: measures how similar is the **content** of the content image to the **content** of the generated image.

$J_{style}(S, G)$: measures how similar is the **style** of the style image to the **style** of the generated image.

$\alpha, \beta$: hyperparameters to control the relative weighting between $J_{content}(C, G)$ and $J_{style}(S, G)$ (1 hyperparameter is enough but the authors of Neural Style Transfer Algorithm use 2).

**Find the generated image G**:
1. Initiate G randomly, G: w x h x3
2. Use gradient to minimize $J(G)$: $G = G - \frac{\partial}{\partial G}J(G)$ (You're actually updating the pixel values of G)
3.

### **Content Cost Function**

Say you use hidden layer l to compute content cost.

Use pre-trained ConvNet. (E.g., VGG network)

Let $a^{[l] (C)}$ and $a^{[l] (G)}$ be the activation of layer l on the images.

If $a^{[l] (C)}$ and $a^{[l] (G)}$ are similar, both images have similar content.

$$J_{content}(C, G) = \frac{1}{2} \left \| a^{[l] (C)} - a^{[l] (G)}  \right\|^2$$

### **Style Cost Function**

**Meaning of the "style" of an image**

Say you're using layer l's activation to measure "style".

Define style as correlation between activations across channels $n_c$.

So what does it mean for two channels to be correlated/uncorrelated?

**Intuition about style of an image**

Let's say the first channel (of layer l's volume) detect features/patterns A, the second channel detect features/patterns B.

We say these two channels are correlated if whatever part of the image has the features/patterns A, that part of image will probably has the features/patterns B as well (and uncorrelated otherwise).

And so, if we use this notion, we can measure the degree of correlation between channels in the generated image and that will tell us (in the generated image) how similar is the style of the generated image to the style of the input style image.

**Style matrix**

Let $a^{[l]}_{i,j,k}$ = activation at $(i,j,k)$.

We compute matrix $G^{[l]}$ is $n_{c}^{[l]} \times n_{c}^{[l]}$, $G^{[l]}_{kk'}$ will measure how correlated are the activations between channel k and k':

$$G^{[l]}_{kk'}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a^{[l]}_{i,j,k}a^{[l]}_{i,j,k'}$$

Large $G^{[l]}_{kk'}$ means channel k and k' are correlated.

Denote $G^{[l] (S)}$ the style matrix for the style image and $G^{[l] (G)}$ the style matrix for the generated image.

$$J_{style}^{[l]}(S, G) = \frac{1}{(2n_H^{[l]}n_W^{[l]}n_C^{[l]})^2} \left \| G^{[l](S)} - G^{[l](G)} \right \|^2_F = \frac{1}{(2n_H^{[l]}n_W^{[l]}n_C^{[l]})^2} \sum_k \sum_{k'}(G^{[l](S)}_{kk'} - G^{[l](G)}_{kk'})^2$$

It turns out that you get more visually pleasing results if you use the style cost function from multiple different layers. The overall style cost function:

$$J_{style}(S,G) = \sum_{l}\lambda^{[l]}J_{style}^{[l]}(S, G)$$

### **1D and 3D Generalizations**

Many ideas you learn about images (2D data) also apply to 1D and 3D data.

## **Mentioned Papers**

[1. DeepFace closing the gap to human level performance, 2014]()

[2. A unified embedding for face recognition and clustering, 2015]()

[3. Visualizing and understanding convolutional networks, 2013]()

[4. A neural algorithm of artistic style., 2015]()
