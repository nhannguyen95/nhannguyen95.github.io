---
layout: post
title: "Bubble sort"
date: 2018-04-22 20:43:00 +0200
categories: ['algorithm']
tags:
  - sorting
comments: true
mathjax: true
content_level: 1
img: https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif
summary: Implementation and complexity analysis of bubble sort algorithm
---

## **1. The idea**

Let's say we want to sort an array in ascending order.

The bubble sorting algorithm's idea is to iteratively bring the 1st largest element to the end of the array, then the 2nd largest element next to the 1st, then the 3rd largest element next to the 2nd and so on:

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif"
  cap="Figure 1. Bubble sort visualization"
  src_cap="Wikipedia - Bubble sort"
  src_url="https://en.wikipedia.org/wiki/Bubble_sort"
%}

Why the name _bubble_? Because if you image the array is a lake, each element is at a different level of water depth, the last element is the water surface. Then the process to slowly bring the largest elements to the surface is just like the bubbles "floating" up.

## **2. Implementation**

{% highlight c++ linenos %}
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for(int k = 1; k < n; k++) {
      // Now we bring the k-th largest element
      // to the end of the array (i.e. to the
      // (n-k) index)
      bool hasSwap = false;
      for(int i = 0; i < n-k; i++)
        if (arr[i] > arr[i+1]) {
          swap(arr[i], arr[i+1]);  // bubble up
          hasSwap = true;
        }
      // Break if there's no more swap
      if (!hasSwap) break;
    }
}
{% endhighlight %}

## **3. Complexity analysis**

Let's call `N` = length of the array.

**Time complexity**: In order to bubble up _k-th_ largest element to the correct position, we need to loop from the beginning of the array through `N-k` elements (to bubble it up). Because `k` is from 1 to `N-1` (we smallest element will be automatically placed in correct order), the total of time we need to visit the elements will be:

$$ \sum_{k=1}^{N-1}N-k = (N-1) + (N-2) + .. + 1 = \frac{(N-1)N}{2}$$

Thus the time complexity is $ O(N^2)$ in worst and average case.

In the best case: the array is already sorted, the time complexity is $ O(N)$; it's like the bubble sort algorithm can check if the array is sorted in $ O(N)$ (by checking if there's any swap).

**Space complexity**: $ O(1)$.
