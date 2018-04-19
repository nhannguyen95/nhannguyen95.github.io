---
layout: post
title: "Preorder traversal on binary tree"
date: 2018-04-19 14:42:00 +0200
# categories: ['algorithm']
tags:
  - tree
comments: true
mathjax: true
content_level: 1
img:
summary: Recursive and iterative code for preorder traversal on binary tree, analyze time and space complexity
---

## **1. Problem statement**
Given a binary tree, print its node values in preorder traversal.

## **2. Solution**

Preorder traversal means we visit the tree in **root, left, right** order: the root node is visited first (thus the word _pre_), then its left and right child.

## **3. Implementation**

### **3.1. Recursion**

{% highlight c++ linenos %}
void preorder(TreeNode* root) {
  if (root == NULL) return;
  
  print("%d ", root->val);
  preorder(root->left);
  preorder(root->right);
}
{% endhighlight %}

### **3.2. Iteration**

Using stack:

{% highlight c++ linenos %}
void preorder(TreeNode* root) {
  stack<TreeNode*> stack({root});
  while(!stack.empty()) {
    TreeNode* node = stack.top();
    stack.pop();
    if (node == NULL) continue;
    print("%d ", node->val);  // visit node
    stack.push(node->right);  // push right first..
    stack.push(node->left);   // ..then push left, so that left is visited before right
  }
}
{% endhighlight %}

## **4. Properties**

* Two binary trees have the same structure if and only if they have the same preorder traversal.

## **5. Pratice**

* [Interviewbit - Preorder Traversal](https://www.interviewbit.com/problems/preorder-traversal/)
* [Leetcode - Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)
