---
layout: post
title: "Reverse a linked list in JavaScript"
date: 2023-07-21 15:30
tags: javascript coding-problems algorithms
navigation: true
class: post-template
current: post
---

Today's leetcode problem involved linked lists again. This time the task was to reverse one.

I honestly had no idea how to do this and had to look it up. Below is the code I ended up with, and an explanation.

```js
var reverseList = function (head) {
  let current = head;
  let previous = null; //because when we start there is no previous
  let next = null;

  //while current node is not null
  while (current) {
    //keep track of what our next node is
    next = current.next;

    //first time through, the new end of our list is null
    current.next = previous;

    //our new previous is the one we just looked at
    previous = current;

    //move on to the next node
    current = next;
  }

  //previous is now new head
  return previous;
};
```

To help me explain, here is a simple diagram of a linked list.

<img src="../assets/images/linkedListDiagram.svg" style="max-width: 500px;" alt="diagram of a linked list with three nodes">

This code presents an iterative approach to the problem. You could also solve it recursively.

For this method, you iterate over each node using a `while` loop, and change the pointers for each node.

#### Initialize the variables

To use the while loop, you need to set a `current` variable. The loop will run while this is not null, or until we reach the end of the list, which will have no next pointer.

We also need to keep track of the `previous` node in the list, as well as the `next` one. `previous` is set to null at the beginning, since we're on the first node, and we'll deal with the `next` value in the loop

#### Setting next and resetting current's pointer

It's crucial to save the `next` variable first thing during a new iteration. That's because the following step replaces the pointer for `current`, so we lost whatever next used to be.

Let's keep track of our variables for the linked list diagram above.

`next` now equals node `2`.

Then, update `current`'s pointer to `previous`. `current.next` now equals `null`.

#### Resetting the variables for the next iteration

For our next iteration, `previous` will be whatever node we just updated in this iteration.

So, `previous` now equals the `current` node, which in this case is node `1`

The `current` node for our next iteration is what we saved earlier in our `next` variable, which is `2`

#### End of list

Once we reach the end, we'll have `previous` equal to our last node, `3`. This is the new head. The pointers have all been reversed, so we return the new head node. It's the start of the reversed linked list.
