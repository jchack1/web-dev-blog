---
layout: post
title: "Does linked list have a cycle - slow/fast pointer algorithm"
date: 2023-07-19 15:30
tags: javascript coding-problems algorithms
navigation: true
class: post-template
current: post
---

Today's leetcode problem involved a linked list.

I understand the basic concept of a linked list, but I have never dealt with them with real code. There is no built-in JavaScript data structure for linked lists, so you need to come up with it yourself.

### What is a linked list?

A linked list is a list of elements (nodes) where each element contains a piece of data and a pointer (reference) to the next element in the list. We need this reference to the next element because linked lists are not stored contiguously in memory.

You can contrast this with arrays, where each element <em>is</em> stored sequentially in memory. This is why an array can contain only elements and no other references. In other programming languages, arrays are fixed in size when they are created, but this is not the case in JavaScript, where you can add and remove elements at any time. For other languages, a linked list would be more flexible because new elements can be added and stored anywhere in memory, and they would also be more efficient because they only take up memory for elements in use.

The first node in a linked list is called the <strong>head</strong>. Every node will point to the next, until you reach the end of the list, where the next element is `null`. Unless, the list contains a cycle, like in today's problem.

### Linked list cycle problem

A <strong>cycle</strong> occurs when a node points to a previous element in the list, so a node can be reached again. Basically the <strong>tail</strong> node doesn't point to `null`, but points to a node we've already seen.

Today's problem was to determine whether or not a linked list has a cycle.

Not seeing any algorithms for linked lists before, I figured I could iterate through the list and if I didn't find a pointer equal to `null`, then there was a cycle. Or, thinking the pointer would be like an array-like numerical index, I could keep a count of the nodes I had already iterated through, and assume we had a cycle if the pointer was a value less than the count.

Neither of those worked. I became a bit frustrated because I wasn't sure what the leetcode input actually looked like. I tried to console log it and got nothing, but their input looked to me like an array, which was confusing, and probably why I thought I could just iterate through it.

I started to look in the discussion and saw many users working with slow and fast pointers.

### Slow and fast pointers

Also called the Hare and Tortoise or Floyd's algorithm.

You traverse the linked list using two pointers at once. You start both pointers on the head node. The "slow" pointer will go through the list one node at a time, while the "fast" pointer will go through the list two nodes at a time. If there is a cycle, there will be a point in time where the slow and fast pointers have landed on the same node - the faster one will have looped back around and met up with the slow one.

I really liked [this explanation](https://medium.com/@dev.adrishs/linked-list-cycle-in-javascript-leetcode-141-142-54c2177c600a) with the tortoise and hare diagram.

You can check if this is the case by checking if the slow and fast pointers are equal to each other.

If there is no cycle, the pointers will never get the chance to meet up.

### The code

Leetcode has their own way of inputting a linked list data structure into JavaScript questions. I could never get a visual in the console, but it is essentially an object that looks something like this:

```js

// example from the above linked medium article

const list = {
    head: {
        data: 6
        next: {
            data: 10
            next: {
                data: 12
                next: {
                    data: 3
                    next: null
                    }
                }
            }
        }
    }
;

```

You can't iterate over this with a for loop, so these problems often use while loops and your fast and slow pointers are defined outside of it.

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function (head) {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow === fast) {
      return true;
    }
  }

  return false;
};
```

Start off by making `slow` and `fast` equal to the `head` node. They are starting off in the same place.

While your `fast` pointer exists and its `next` pointer is not null, you can move your pointers forward.

Then you can check if they are equal to the same node. If they are, you have a cycle, and can return true.

At any point you end up with `fast` or its `next` pointer being null, it means you've reached the end of the list, and there is no cycle, so you can return false.

### A different solution

Another solution I came across used a `set` to store all values we traversed. If your current node matched a node already in the set, you have a cycle, as you've seen it before.

```js
var hasCycle = function (head) {
  if (!head || !head.next) return false;

  let set = new Set();

  let nextNode = head.next;

  while (nextNode) {
    if (set.has(nextNode)) return true;

    set.add(nextNode);

    nextNode = nextNode.next;
  }

  return false;
};
```

This logic makes a little more sense in my brain, but it seems like you could potentially use a lot of memory if you added nearly every node to your set. This solution put me in the bottom % of memory usage in leetcode. It seems like the slow/fast algorithm is the way to go.
