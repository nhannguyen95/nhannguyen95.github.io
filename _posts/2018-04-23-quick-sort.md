---
layout: post
title: "Quick sort"
date: 2018-04-23 17:34:00 +0200
tags:
  - algorithm
comments: true
mathjax: true
content_level: 1
img: https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif
summary: Implementation and complexity analysis of quick sort algorithm
---

## **1. The idea**

Say we want to sort an array in ascending order.

The quick sort algorithm's idea is that you choose a random element in the array, let's call it _pivot_, then you reorder the array so that it has the form `elements < pivot, pivot, elements > pivot`, then do the same thing for those 2 unsorted parts.

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif"
  cap="Figure 1. Quick sort visualization"
  src_cap="Wikipedia - Quick sort"
  src_url="https://en.wikipedia.org/wiki/Quicksort"
%}

## **2. Implementation**

`N` = length of array.

Say we want to quick sort an array from index `left` to `right`. Like I mentioned, first you need to choose a _pivot_ value and reorder the array, the reordering phase is called **partition**.

Theorically, you can choose whatever pivot value you want, since I find the [**Lomuto partition scheme**](https://en.wikipedia.org/wiki/Quicksort#Lomuto_partition_scheme) intuitive, I'm gonna pick the last element as the pivot: 

{% highlight c++ linenos %}
void quick_sort(vector<int>& arr) {
  int n = arr.size();
  quick_sort(arr, 0, n-1);
}

// overload
void quick_sort(vector<int>& arr, int left, int right) {
  // int pivotIndex = right;
  int correctPivotIndex = partition(arr, left, right);
}

int partition(vector<int>& arr, int left, int right) {
  int i = left;
  for(int j = left; j < right; j++)
    if (arr[j] <= arr[right])
      swap(arr[i++], arr[j]);
  swap(arr[i], arr[right]);
  return i;
}

void partition
{% endhighlight %}

Some notice here:
* The chosen pivot value is `arr[pivotIndex]`.
* After partition: `arr[left..correctPivotIndex] <= pivot value` and `arr[correctPivotIndex+1..right] > pivot value`.
* **The pivot value is now placed at its correct position in the sorted array (i.e. at `correctPivotIndex`-th index).**

Now we need to sort the 2 unsorted part recursively (and also add the base case):

{% highlight c++ linenos %}
// overload
void quick_sort(vector<int>& arr, int left, int right) {
  if (left >= right) return;
  
  // int pivotIndex = right;
  int correctPivotIndex = partition(arr, left, right);
  quick_sort(arr, left, correctPivotIndex-1);
  quick_sort(arr, correctPivotIndex+1, right);
}
{% endhighlight %}

_It's late I need to go home.._
