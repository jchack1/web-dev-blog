---
layout: post
title: "More data structures - JavaScript"
date: 2022-03-11 15:30
tags: javascript data-structures
navigation: true
class: post-template
current: post
---

Last year I began learning more about data structures. I found myself often wondering how the theory I was learning would apply to JavaScript, the language I code in most frequently.

Currently I am going through a [tutorial on JavaScript data structures](https://www.youtube.com/watch?v=t2CEgPsws3U&list=WL&index=21&t=3s), and I will make some notes here as I learn.

You are able to use built-in data structures in JavaScript for some of these, and others you need to code yourself.

The instructor uses object-oriented programming, creating classes for the types of data structures, and methods within those classes that can be used to interact with the data structure.

Some helpful articles:

- [https://www.educative.io/blog/javascript-data-structures](https://www.educative.io/blog/javascript-data-structures)
- [https://www.freecodecamp.org/news/10-common-data-structures-explained-with-videos-exercises-aaff6c06fb2b](https://www.freecodecamp.org/news/10-common-data-structures-explained-with-videos-exercises-aaff6c06fb2b)

### Stack

Stacks operate with a Last In First Out (LIFO) functionality - the last thing you put onto your stack is the first thing that will be removed, like if you had a stack of books or plates.

Examples of JavaScript functions that work like this:

- the .push() method, which adds to the top of a stack
- the .pop() method, which removes from the top of a stack

JavaScript already has the build in methods that enable you to use an array as a stack.

### Set

Like an array, but all items are unique - no duplicates.

You can [read more about sets on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set). There is a built in Set function in ES6.

Using this functionality, you create a new set using the Set() constructor, like this:

```js
const mySet = new Set();
```

There are several built-in methods you can use to interact with your set.

- add() - adds a value to the set
- has() - checks if the set has a particular item
- delete() - deletes a value from the set
- clear() - clears the whole set

There are some methods that you might want to use with sets generally, but aren't included in ES6. You'd need to create your own set class with these methods:

- union: combines multiple sets and leaves out duplicate items
- intersection: compares two sets, returns a new set that includes items that are in <strong>both</strong> sets
- difference: compares two sets, returns items that are in one set but not the other set
- subset: tests if one set is a subset of another set - so if one set is fully contained within another set

### Queue

Follows a First In First Out (FIFO) pattern - the first item placed into the queue is the first item that is processed.

In JavaScript you can use an array for this, or create your own class with more methods.

Some built in JavaScript methods:

- .push() - add to the end of an array
- .shift() - remove from the beginning of an array

Some methods that queues sometimes use, that are not built into JavaScript:

- front() - tells you what's at the beginning of your queue - in JavaScript we would just do `myArray[0] `
- size() - just use `myArray.length` to get size
- isEmpty() - just check the length of the array `myArray.length === 0 `

You can also create <strong>priority queues.</strong> Again, this is a functionality you would need to create. You pass in the item as well as it's priority, and it is added to the queue based on priority.

One way of checking priorities is to use a for loop. Each item you put into the queue is its own array, at the 0 index is the item and at 1 index is a number for its priority. For each element already in the queue, compare its priority value to the incoming item's priority value. If the incoming priority value is less than the item we are comparing to, we add it to the queue at this position using the "splice" method.

### Tree

A tree is a branching data structure, where all data points are called nodes. The top node is called the "root" node. Nodes with their own branches are called parent nodes, and nodes branching from them are called child nodes.

In a <strong>binary tree,</strong> each node can only have two branches. Nodes in a left subtree must have a value of less than or equal to the parent node. Nodes in a right subtree must have a value of greater than or equal its parent node.

Using binary search, you don't have to check every single item for what you're looking for, and you are able to skip about half the tree. Time taken for the search is proportional to the logarithm of the number of items in the tree, with a big O notion of O(log n). So it's pretty fast, but slower than searching a hash table.

Some tree traversal methods (ways of exploring the data in the tree):

- in order: begin search at left-most node and end at right-most node; gives values in order from smallest to largest
- pre order: start at root nodes before looking at leaves
- post order: start at leaf nodes before going to roots
- level order: explores all nodes on a level of the tree before moving on to the next level, starting at the root node

### Hash table

Hash tables are used for key-value pairs, like maps or objects. They are very efficient and searching doesn't depend on the number of items, with a big O notion of O(1).

They work by putting your key into a hash function, which assigns strings to a number, usually an index in an array. The value is stored here.

### Linked list

A list where items are stored in nodes. Nodes contain the item and a reference to the next node.

When performing any operation on a linked list you always have to start at the beginning of the list, or head node. You can't just access something in the middle of the list without going through the items preceding it.

### Binary heap

It has a similar structure to a binary tree, but the ordering is different and is one of two types:

- max heap: parent nodes are greater than or equal to child nodes
- min heap: parent nodes are less than or equal to child nodes

The order the values within a level does not matter. Levels are filled from left to right.

Arrays are used to implement heaps in JavaScript. You add values to the array starting at <strong>index 1, not 0. Index 0 is assigned null.</strong>

You can calculate where elements are in the array using the calculations below, where i is your current index (or position in the tree):

- Left child is i \* 2
- Right child is i \* 2 + 1
- Parent is i / 2

### Graphs

Graphs contain nodes, or vertices, connected by edges. You can have directed or undirected graphs - meaning, the edges of the graph can have direction, or no direction, respectively.

An example of an undirected graph could be a social network, where nodes are people and edges are whether or not they are connected/know each other.

How to traverse a graph:

- breadth-first search: start at one node, visit all its neighbours that are one edge away first, then visits all their neighbours. Keep in mind the graph is directed or undirected - a node could look like it's one edge away, but if you can't move in that direction, you can't go to that node in one move.

### Conclusion

I was hoping to find more concrete examples of real applications of data structures in JavaScript. What I did get out of this, though, is a review of basic data structures, and an understanding of how I could create any of these data structures using classes in JavaScript.

There is a lot more learning I can do with data structures and algorithms. One big take-away is that choosing the right data structure is all about the problem you are trying to solve, and how to solve it efficiently. My strategy going forward may be to deal with it on a case by case basis - when trying to solve a problem I can use my basic knowledge of data structures to think about the most efficient solution, and do more research from there if needed. Built-in JavaScript methods and data structures, like arrays, maps, and objects, have been enough to solve my problems so far, so maybe I don't need to get too advanced yet.
