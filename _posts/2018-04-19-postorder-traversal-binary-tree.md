---
layout: post
title: "Postorder traversal on binary tree"
date: 2018-04-19 16:49:00 +0200
# categories: ['algorithm']
tags:
  - tree
comments: true
mathjax: true
content_level: 1
img:
summary: Recursive and iterative code for postorder traversal on binary tree, analyze time and space complexity
---

## **1. Problem statement**
Given a binary tree, print its node values in postorder traversal.

{% include image.html
  url="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Binary_tree.svg/1200px-Binary_tree.svg.png"
  cap="Figure 1. Postorder of this binary tree is 2 5 11 6 7 4 9 5 2"
  src_cap="Wikipedia - Binary tree"
  src_url="https://en.wikipedia.org/wiki/Binary_tree"
%}

## **2. Solution**

Postorder traversal means we visit the tree in **left, right, root** order: the left child and right child are visited first, the root is visited last (thus the word _post_).

## **3. Implementation**

There is a solution that we perform [preorder traversal](https://nhannguyen95.github.io/preorder-traversal-binary-tree/) in the order **root, right, left**, then reverse the result to get the postorder traversal **left, right, root**. This is trivial and won't be discussed.

### **3.1. Recursion**

{% highlight c++ linenos %}
void postorder(TreeNode* root) {
  if (root == NULL) return;
  
  postorder(root->left);
  postorder(root->right);
  print("%d ", root->val);
}
{% endhighlight %}

Let's call `N` is the number of nodes, `H` is the height of the tree.

* Time complexity: $ O(N)$ since each node is visited exactly once.
* Space complexity: $ O(H)$, the call stack can reach `H` in depth and each recursive call needs $ O(1)$ to store its parameters.

### **3.2. Iteration**

{% highlight c++ linenos %}
void postorder(TreeNode* root) {
  stack<TreeNode*> stack;
  TreeNode* node = root;
  while(!stack.empty() || node != NULL) {
    if (node != NULL) {
      stack.push(node);
      node = node->left;
    } else {
      TreeNode* peek = stack.top();
      if (peek->right != NULL && peek->right != lastVisit) {
        node = peek->right;
      } else {
        print("%d ", peek->val);  // visit root
        stack.pop();
        lastVisit = peek;
      }
    }
  }
}
{% endhighlight %}

* Time complexity: $ O(N)$ since each node is visited exactly once.
* Space complexity: Space complexity is the maximum size of `stack`.  On a path from root to some node, easily observe that the stack need to store all of nodes on that path. Thus the space complexity is $ O(H)$.

## **4. Practice**

* [Interviewbit - Postorder Traversal](https://www.interviewbit.com/problems/postorder-traversal/)
* [Leetcode - Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)
