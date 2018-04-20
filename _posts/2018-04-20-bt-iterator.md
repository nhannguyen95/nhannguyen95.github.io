---
layout: post
title: "Binary tree iterators"
date: 2018-04-20 12:54:00 +0200
# categories: ['algorithm']
tags:
  - tree
comments: true
mathjax: true
content_level: 1
img:
summary: How to build preorder, inorder, postorder binary tree iterators that allow us to treat a binary tree as an array for further processing
---

## **1. Problem statement**
Given a binary tree, build iterator classes that can traverse a binary tree (in preorder, inorder, postorder) and have following operators:
* `next`: return current node and move the iterator 1 step.
* `hasNext`: check if we're done traversing the tree or not.

Usage example:

{% highlight c++ linenos %}
// Inorder binary tree iterator
InorderBTIterator it(root);
while(it.hasNext()) {
  // Visit node in inorder
  print("%d ", it.next()->val);
}
{% endhighlight %}

## **2. Solution**

All we need to do is wrap the iterative implementation of the traversal algorithm inside `next` method, and return the node when we visit it.

## **3. Implementation**

I just write the code for preorder and inorder binary tree iterator, the postorder iterator's is totally similar.

Preorder binary tree iterator:

{% highlight c++ linenos %}
class PreorderBTIterator {
private:
	stack<TreeNode*> stk;
public:
	PreorderBTIterator(TreeNode* root) {
        if (root != NULL) stk.push(root);
	}

	TreeNode* next() {
		while(!stk.empty()) {
			TreeNode* ret = stk.top();
			stk.pop();

			if (ret->right != NULL) stk.push(ret->right);
			if (ret->left != NULL) stk.push(ret->left);

			return ret;
		}
	}

	bool hasNext() {
		return !stk.empty();
	}
};
{% endhighlight %}

_Side notes: we can use [Morris inorder traversal](https://nhannguyen95.github.io/morris-inorder-tree-traversal/) implementation to reduce the space complexity (basically we can use any iterative tree traversal implementation in `next` method)._

Inorder binary tree iterator:

{% highlight c++ linenos %}
class InorderBTIterator {
private:
	TreeNode* node;
	stack<TreeNode*> stk;
public:		
	InorderBTIterator(TreeNode* root) : node(root) {}

	TreeNode* next() {
		while(!stk.empty() || node != NULL) {
			if (node != NULL) {
				stk.push(node);
				node = node->left;
			} else {
				TreeNode* ret = stk.top();
				stk.pop();
				node = ret->right;
				return ret;
			}
		}

	}

	bool hasNext() {
		return (!stk.empty() || node != NULL);
	}
};
{% endhighlight %}


