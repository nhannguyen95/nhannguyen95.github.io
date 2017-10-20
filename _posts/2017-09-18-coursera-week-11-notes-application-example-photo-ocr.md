---
layout: post
title:  "Week 11 notes: Application Example: Photo OCR"
date: 2017-09-18 13:17:00 +0700
categories: ['machine learning']
tags: notes
no-post-nav: 0
comments: 1
---

##### **Introduction**
No notes.

##### **Photo OCR**

###### **Problem Description and Pipeline**

**Photo OCR Problem**

Photo Optical Character Recognition, recognize text from pictures.

**Photo OCR pipeline**

<hr>
<center><img src="https://i.imgur.com/d8gjmcf.png"/></center>
<center>Figure 1. Photo OCR pipeline</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

**Machine learning pipeline**

A system with many stages/components, several of which may use machine learning.

###### **Sliding Windows**

Let's consider a slightly easier problem than text detection: pedestrian detection (it's easier because the aspect ratio is not much different).

**Supervised learning for pedestrian detection**

<hr>
<center><img src="https://i.imgur.com/G59EdpZ.png"/></center>
<center>Figure 2. The input for pedestrian detection</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Train about 1000 or 10000 images using some neural network or other learning algorithm.

**Sliding Window**

You can use different image patches with different sizes (but the aspect ratio is the same) and scan/shift through the image, and detect the appearance of pedestrian at the position of the image patch.

The amount by which you shift the image patch (rectangle) over each time is called **the step size/stride parameter**.

<hr>
<center><img src="https://i.imgur.com/3x8l0N9.gif"/></center>
<center>Figure 3. Sliding Window illustration</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

**Back to text detection problem**

The input is similar to the pedestrian problem:

<hr>
<center><img src="https://i.imgur.com/8VKBSmF.png"/></center>
<center>Figure 4. The input for text detection problem</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Use sliding window, take the output of the classifier and apply an expansion operator:

<hr>
<center><img src="https://i.imgur.com/T0apvEi.png"/></center>
<center>Figure 5. Expansion operator takes the white blobs and expands it</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

We can now look at the connected components of the right most image (white regions) and draw bounding boxes around them.

If we use simple heuristic, we can rule out rectangles whose aspect ratios look "funny" (we know that boxes around text should be much wider than they are tall).

**1D Sliding window for character segmentation**

Next stage, how do we segment out the individual characters in the rectangle?

Again, use a supervised learning algorithm to detect if there a split/gap between two characters right in the middle of a image patch:

<hr>
<center><img src="https://i.imgur.com/2ABEHdz.png"/></center>
<center>Figure 6. Detect the gap between two characters to recognize individual characters</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

###### **Getting Lots of Data and Artificial Data**

**Describe the two variations of artificial data synthesis for helping collect data**

If we are essentially creating data from scratch.

If we already have small label training set and we somehow have amplify that training set or use a small training set to turn that into a larger training set.

**Apply artificial data synthesis techniques to collect text detection data**

We can take characters from different fonts and paste theses characters against different random backgrounds, this is called synthetic data.

**Synthesizing data by introducing distortions**

<hr>
<center><img src="https://i.imgur.com/XIomZx9.png"/></center>
<center>Figure 7. Take the character in the real data to create new training examples</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

We can also use distortion with speech or sound.

Distortion introduced should be representation of the type of noise/distortions in the test set.

Usually does not help to add purely random/meaningless noise to your data.

**Discussion on getting more data**
1. Make sure you have a low bias classifier before expanding the effort. (Plot learning curves). E.g. keep increasing the number of features/number of hidden units in neural network until you have a low bias classifier.

2. "How much work would it be to get 10x as much as data as we currently have?"
    * Artificial data synthesis.
    * Collect/label it yourself (How much time does it take?).
    * "Crowd source".

###### **Ceiling analysis: What part of the pipeline to work on next**

**Estimate the errors due to each component (ceiling analysis)**

What part of the pipeline should you spend most time trying to improve?

It's going to be very helpful to have a single rolled number evaluation metric for the learning system. We can pick the accuracy level.

The idea of ceiling analysis: in the OCR application, first we measure the accuracy of the overall system (say it's 72%). we will go through let say the first module of the machinery pipeline (text detection), and we will replace the work of this module by human (e.g. stimulate what happens if we have a text detection system with 100% accuracy). After that we measure the accuracy of the overall system again and the result this time is 89%. Do similarly on other module (**but keep the previous modules' accuracy as 100%**), we may get the below figures:

<hr>
<center><img src="https://i.imgur.com/MOq0exU.png"/></center>
<center>Figure 8. The accuracy of overall system received by making the modules in order has 100% accuracy</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

From this table:
* If we have a perfect text detection, we can have a 17% performance gain.
* If we have a perfect character segmentation, the performance goes up by only 1%.
* If we have a perfect character segmentation, the performance goes up by only 10%.

From there, we can decide which module/part is worth investing time.
