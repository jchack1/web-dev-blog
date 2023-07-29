---
layout: post
title: "Binary trees; isSameTree problem"
date: 2023-07-28 15:30
tags: algorithms data-structures coding-problems recursion
navigation: true
class: post-template
current: post
---

It's time to dive into some binary tree coding problems.

But I think I need to learn more about binary trees, and how to tackle these kinds of problems first, so I actually have a chance of getting them right.

Resources:

- This was a quick, [helpful video](https://www.youtube.com/watch?v=GzJoqJO1zdI) that summarized binary trees nicely
- This [video from freeCodeCamp](https://www.youtube.com/watch?v=5cU1ILGy6dM) describes how to deal with binary search trees in JavaScript
- This [article from freeCodeCamp](https://www.freecodecamp.org/news/binary-search-tree-traversal-inorder-preorder-post-order-for-bst/) summarizing tree traversal

### Compared to other data structures - linked lists

Like linked lists, trees are made up of nodes that have data and pointers.

I've delved a bit into singly linked-lists, whose nodes have one pointer. In comparison, trees can have multiple pointers to the next nodes. In the case of a binary tree, a node points towards a maximum of 2 child nodes. Each node can have a `left` and `right` pointer, comparable to the `next` pointer in a linked list.

The top node of the tree is called the `root` node, comparable to the `head` node of a linked list. Nodes with no children are called `leaf` nodes. The path from the root to a leaf is called a `branch`.

### Traversing a binary tree

For starters, there are two main approaches to code a tree traversal:

- using a `while` loop and an "on" pointer (like for linked lists, using a "current" pointer with a while loop)
- recursion

When you use recursion it's called branching recursion. Since a node can have multiple children, your stack frame can be branching too.

Often you need to visit every node in the tree, but how do I know what order to visit in? There are three main types of traversal:

- <strong>in-order traversal</strong>: visit left subtree, then root, then right subtree. Like you've moving across the tree from left to right. If you printed out each node as you visted them, they would be in ascending order from left-most to right-most node.
- <strong>pre-order traversal</strong>: visit root, then left subtree, then right subtree. Like you've moving from top of tree to bottom (root to leaf). Use if you need to explore the tree from root to leaf, good for making copies of trees.
- <strong>post-order traversal</strong>: visit left subtree, right subtree, then root. Like you're travelling from the bottom up (leaf to root). Use if you need to explore leaves before roots. Good for deleting a tree.

### Coding problem: isSameTree, solved with recursion

The first coding problem I worked on to practice binary trees was, given two trees `p` and `q`, determine if they are the same. If they are the same, they have the same structure AND values at each node.

I decided to try solving this recursively. Sometimes I still struggle with recursion, due to its nested nature. One thing that helps me is to remember that I should always be returning whatever data type the final result is looking for. In this case, we are looking for a boolean, so my base cases must all return true or false. It's also helpful to think of recursion as repeating sub-problems. What is the sub-problem you are trying to solve here, over and over again?

In this case, the sub-problem is checking if the current nodes p and q are equal.

After checking base cases, you need some logic for checking your next values. That's where your recursive calls come in. Then, you need to do something with the result of those calls. The outer function could return at this point, or maybe you need to add the result to another data structure you're keeping track of, like a memo object.

This is what I came up with, and it submitted to leetcode successfully:

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function (p, q) {
  //base cases
  if (p === null && q === null) return true;

  if ((p === null && q !== null) || (q === null && p !== null)) {
    return false;
  }

  if (p.val !== q.val) return false;

  //continue exploring tree
  if (
    isSameTree(p.left, q.left) === false ||
    isSameTree(p.right, q.right) === false
  )
    return false;

  return true;
};
```

First, the base cases:

- both p and q are null: in this case return true, because they are equal, and we have reached a leaf node. This branch has nothing else to explore and if we got this far, it is the same for p and q
- one of p <strong>or</strong> q, <strong>not both</strong>, is null: if one node is null and the other is not, they are not <em>structurally</em> equal, so return false
- check if the <strong>values</strong> of p and q are equal. If not, return false. Otherwise, the nodes so far are equal and there is more tree to explore, so we can move on.

Next step: recursive calls

- we need explore the left and right subtrees for our p and q nodes. If either of these returns false, it means p and q were not equal.
- this is where you start to see branching recursion come into play, since you have to check the left and right child of every node.

Finally, if we got through all of that and never encountered unequal structure or values, we can return true.

### Next steps

I definitely need more practice with binary trees, so I'll continue learning and practicing and will update here if I find anything notable.
