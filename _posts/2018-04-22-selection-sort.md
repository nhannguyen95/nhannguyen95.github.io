---
layout: post
title: "Selection sort"
date: 2018-04-22 22:24:00 +0200
categories: ['algorithm']
tags:
  - sorting
comments: true
mathjax: true
content_level: 1
img:
summary: Implementation and complexity analysis of selection sort algorithm
---

## **1. The idea**

Say we want to sort an array in ascending order, `N` = length of array

The selection sort algorithm's idea is that first we find the smallest element and move it to the front of the array, then find the 2nd smallest element move it, keep doing that until the array is sorted.

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif"
  cap="Figure 1. Selection sort visualization"
  src_cap="Wikipedia - Selection sort"
  src_url="https://en.wikipedia.org/wiki/Selection_sort"
%}

The selection sort is similar to the [bubble sort](https://nhannguyen95.github.io/bubble-sort/), they're just different in how the next element is found:
* In bubble sort, this's done by swapping.
* In selection sort, this's done by selection (thus the name _selection_ sort).

## **2. Implementation**

{% highlight c++ linenos %}
void selection_sort(vector<int>& arr) {
  for(int k = 0; k < n-1; k++) {
    int minId = k;
    for(int i = k+1; k < n; k++)
      if (arr[i] < arr[minId])
        minId = i;
  }
  swap(arr[k], arr[minId]);
}
{% endhighlight %}

## **3. Complexity analysis**

Let's call `N` = length of the array.

**Time complexity**: Regardless of the initial array is sorted or not, time complexity of selection sort is always $O(N^2)$ (proof is similar to [this](https://nhannguyen95.github.io/bubble-sort/#3-complexity-analysis).)

**Space complexity**: $ O(1)$.

## **4. Properties**

* The above implementation is not a stable one, but we can implement insertion sort as a stable sort by using an efficient data structure (like linked list) to squeeze the swapping step.
