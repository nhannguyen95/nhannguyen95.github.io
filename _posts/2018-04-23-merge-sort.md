---
layout: post
title: "Merge sort"
date: 2018-04-23 16:29:00 +0200
categories: ['algorithm']
tags:
  - sorting
comments: true
mathjax: true
content_level: 1
img: https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif
summary: Implementation and complexity analysis of merge sort algorithm
---

## **1. The idea**

Say we want to sort an array in ascending order.

The merge sort algorithm's idea is that assuming you're already had the first half and the second half of the array sorted, you can now merge them to create a sorted array (thus the name _merge_ sort).

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif"
  cap="Figure 1. Merge sort visualization"
  src_cap="Wikipedia - Merge sort"
  src_url="https://en.wikipedia.org/wiki/Merge_sort"
%}

## **2. Implementation**

`N` = length of array.

Say we want to sort an array from index 0 to `N-1` (a whole array).

{% highlight c++ linenos %}
void merge_sort(vector<int>& arr) {
  int n = arr.size();
  merge_sort(arr, 0, n-1);
}
{% endhighlight %}

Like I mentioned: if we are sorting the array from index `left` to index `right`, and we have the first half and second half sorted, we can merge them in linear time so that the whole `left` to `right` is sorted:

{% highlight c++ linenos %}
// overload
void merge_sort(vector<int>& arr, int left, int right) {
	
	int mid = left + (right - left) / 2;

	// Assume that [left, mid] and (mid, right] is sorted,
	// now we merge them
	int* temp = new int[right-left+1];
	int i = left, j = mid+1, k = 0;
	while(i<=mid && j<=right) {
		if (arr[i] <= arr[j]) temp[k++] = arr[i++];
		else temp[k++] = arr[j++];
	}
	while(i <= mid) temp[k++] = arr[i++];
	while(j <= right) temp[k++] = arr[j++];

	// Copy temp back to arr[left, right]
	for(k = left, k <= right; k++)
		arr[k] = temp[k-left];

	delete temp;
}
{% endhighlight %}

How do we get the first half and second half sorted? We can do it recursively (divide and conquer) by calling `merge_sort` for those 2 halves. Our base case is when `left >= right`, we need to do nothing:

{% highlight c++ linenos %}
void merge_sort(vector<int>& arr, int left, int right) {
  // Base case
	if (left >= right) return;
  
	int mid = left + (right - left) / 2;
  
  merge_sort(arr, left, mid);     // sort first half
  merge_sort(arr, mid+1, right);  // sort second half

	// Now we merge them
	int* temp = new int[right-left+1];
	int i = left, j = mid+1, k = 0;
	while(i<=mid && j<=right) {
    // If tie, take the left one, this makes
    // merge sort stable.
		if (arr[i] <= arr[j]) temp[k++] = arr[i++];
		else temp[k++] = arr[j++];
	}
	while(i <= mid) temp[k++] = arr[i++];
	while(j <= right) temp[k++] = arr[j++];

	// Copy temp back to arr[left, right]
	for(k = left, k <= right; k++)
		arr[k] = temp[k-left];

	delete temp;
}
{% endhighlight %}

## **3. Complexity analysis**

**Time complexity**: 

Let's call $T(N)$ = number of operations when compute `merge_sort(arr, left, right)` given $right - left = N$.

In each `merge_sort` function:
* We sort 1st and 2nd halves, which takes $2T(\frac{N}{2})$.
* We merge those two halves, which takes $2N$ operations.

So: $T(N) = 2T(N/2) + 2N$, easy to show that this has a time complexity of $O(NlogN)$ (in average and worst case).

**Space complexity**:

Let's call $S(N)$ = number of storage units when compute `merge_sort(arr, left, right)` given $right - left = N$.

In each `merge_sort` function:
* We sort 1st and 2nd halves, which takes $max(S(\frac{N}{2})$.
* We allocate the `temp` array which takes $N$ storage units.

So: $S(N) = max(S(\frac{N}{2}), N) = N$, so the space complexity is $O(N)$.

If using linked-list, we can merge arrays without `temp`, the space complexity is $O(1)$.

## **4. Properties**

* Merge sort is a stable sort algorithm.

