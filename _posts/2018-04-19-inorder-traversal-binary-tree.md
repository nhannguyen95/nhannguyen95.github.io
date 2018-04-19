---
layout: post
title: "Inorder traversal on binary tree"
date: 2018-04-19 16:04:00 +0200
# categories: ['algorithm']
tags:
  - tree
comments: true
mathjax: true
content_level: 1
img:
summary: Recursive and iterative code for inorder traversal on binary tree, analyze time and space complexity
---

## **1. Problem statement**
Given a binary tree, print its node values in inorder traversal.

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Binary_tree.svg/1200px-Binary_tree.svg.png"
  cap="Figure 1. Inorder of this binary tree is 2 7 5 6 11 5 4 9"
  src_cap="Wikipedia - Binary tree"
  src_url="https://en.wikipedia.org/wiki/Binary_tree"
%}

## **2. Solution**

Inorder traversal means we visit the tree in **left, root, right** order: the left node is visited first, then the root the right child (thus the word _in_).

## **3. Implementation**

### **3.1. Recursion**

{% highlight c++ linenos %}
void inorder(TreeNode* root) {
  if (root == NULL) return;
  
  inorder(root->left);
  print("%d ", root->val);
  inorder(root->right);
}
{% endhighlight %}

Let's call `N` is the number of nodes, `H` is the height of the tree.

* Time complexity: $ O(N)$ since each node is visited exactly once.
* Space complexity: $ O(H)$, the call stack can reach `H` in depth and each recursive call needs $ O(1)$ to store its parameters.

### **3.2. Iteration**

Solution 1: (more intuitive)

{% highlight c++ linenos %}
void inorder(TreeNode* root) {
  stack<TreeNode*> stack();
  TreeNode* node = root;
  while(!stack.empty() || node != NULL) {
    while(node != NULL) {
      stack.push(node);
      node = node->left;
    }
    node = stack.top(); stack.pop();
    print(node->val);
    node = node->right;
  }
}
{% endhighlight %}

Solution 2:

{% highlight c++ linenos %}
void inorder(TreeNode* root) {
  stack<TreeNode*> stack();
  TreeNode* node = root;
  while(!stack.empty() || node != NULL) {
    if (node != NULL) {
      stack.push(node);
      node = node->left;
    } else {
      node = stack.top(); stack.pop();
      print(node->val);
      node = node->right;
    }
  }
}
{% endhighlight %}

* Time complexity: $ O(N)$ since each node is visited exactly once.
* Space complexity: space complexity is the maximum size of `stack`. On a path from root to some node, easily observe that the stack need to store all of nodes on that path. Thus the space complexity is $ O(H)$.

## **4. Properties**

* Inorder traversal on a binary search tree will return a increasing array order.

## **5. Practice**

* [Interviewbit - Inorder Traversal](https://www.interviewbit.com/problems/inorder-traversal/)
* [Leetcode - Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
