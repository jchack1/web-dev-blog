---
layout: post
title: "Learning data structures - advanced structures (trees)"
date: 2021-07-07 14:30
tags: computer-science data-structures
navigation: true
class: post-template
current: post
---

This is the last section of intro to data structures. We are now getting into non-linear data structures.

Everything up to this point has been a linear structure. The next data structures to cover are trees.

Once again following along with [this video](https://www.youtube.com/watch?v=zg9ih6SVACc&t=7744s) and supplementing with other sources.

### Trees

Nodes in a tree have a hierarchical relationship. Nodes can point to multiple nodes instead of just one, like we saw in linked lists.

There are lots of different types of trees, but one important one is called a <strong>binarysearch tree</strong> .

Binary search tree:

- some rules:
  - nodes can only have 2 children
  - the child to the left of the parent must be less than or equal to the parent while the child to the right must be greater than or equal to the parent
  - nodes cannot have the same value
- can perform searches on the tree in a logarithmic time
- at any given node during a search, choose left or right depending on if the value is less than or greater than the value at the current node
- these are good for storing large amounts of data because they are quick to search - for BigO notation, I believe that makes it O(log n)

Another tree that is used is called a <strong>Trie</strong>

- these store letters of the alphabet
- to retrieve word based data
- each node has an array that includes references to letters
- the nodes basically spell out words as you go down the different paths on the tree
- the end of a word is marked by a flag - so nodes contain pointers to letters that follow as well as flags for the ends of words

Heaps:

- parent nodes compare to child nodes and whether they are greater than or less than
- <strong>min-heap:</strong> root node is the minimum compared to the children
- <strong>max-heap:</strong> root node is the maximum compared to the children

Graphs:

- made up of nodes and edges (paths between the nodes)
- not linear
- undirected graphs: direction between nodes doesn't matter
- directed graph: direction does matter
- cyclic graph: nodes have a path back to itself (undirected count, since you can go any direction back to the start)
- acyclic graph: no path from one node back to itself
- edges can have a weight - kind of like distance between nodes

When to use trees?

- any time you have hierarchical data
- tries are used by things like spellcheck and autocomplete features
- heaps used for sorting algorithms like HeapSort, and priority queues
- graphs: used in shortest path algorithm, social media users, and more

### Conclusion

That's it for intro data structures! These posts probably read way more like notes than actual blog posts, but as I am still learning data structures I don't have much else to share.

It's interesting to learn the theory but I kept wondering how I would actually apply this in real projects. Especially in Javascript projects. Do all these data structures exist in Javascript? Do they go by different names? I still have a lot of questions.

My next step will be to learn about Javascript specific data structures and real life applications.
