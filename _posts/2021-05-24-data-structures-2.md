---
layout: post
title: "Learning data structures - intermediate structures"
date: 2021-06-10 14:30
tags: computer-science data-structures
navigation: true
class: post-template
current: post
---

Now we are getting into some intermediate topics in data structures.

Again I am following along with the [freeCodeCamp video](https://www.youtube.com/watch?v=zg9ih6SVACc&t=3551s) on data structures as I learn.

We are now going to be going over sequential access data structures. Unlike random access data structures, you can access items only in a certain order, which means you are depending on other items in the data structure. You can't get items instantly like you can with arrays.

### Stacks

Stacks follow the "last in first out" principle - the last item you add is the first item to be removed.

The video has a great analogy for this where they envision a stack of books. If you wanted to grab a book from the middle of the stack, you first have to remove all the other books on top, in order, otherwise the whole thing will collapse.

I also thought of this like a stack of plates. The last one you put on top of the stack is the first one you will use.

Some stack methods:

- push: adds to top of stack
- pop: removes from top of stack
- peek: get the value of the top item, without removing it
- contains: to find something within the stack

Interestingly, pop and push are the same names as common Javascript array methods. The contains method reminds me of the Javascript array method "includes". This is definitely making me think I need to find a Javascript specific data structures resource.

BigO efficiency:

- accessing, searching: O(n)
- inserting, deleting: O(1), because you just do push or pop once

When would you use stacks?

- for recursion - when functions call themselves repeatedly
- a back button in the browser - add your pages/actions to a stack

### Queues

Follows a first in first out principle. Just like a line-up in real life. The first person in the line is the first person served.

Some queue methods:

- enqueue: add to the end/tail of the queue
- dequeue: remove from the start/head of the queue
- peek: tells us the value at the head of the queue
- contains: if our queue contains are specific element

BigO efficiency:

- accessing, searching: O(n)
- inserting, deleting: O(1), because you just do enqueue or dequeue once, same as the stack

I actually use queues fairly often when working with AWS, particularly SQS queues. Events can be added to a queue, and processed by, for example, a Lambda function in the order they are received, when the function is ready for the next one.

### Linked lists

I was very interested in learning what exactly a linked list is. I hear about them all the time but don't actually know what they do.

- sequential
- each element is called a node, and a node contains data and a reference to the next node
- nodes are objects and can have multiple values
- the reference is what <em>links</em> the elements together into a list
- the last node reference points towards a null value
- can add or remove items anywhere in the list, unlike stacks and queues, but this means you also have to change the references
- can only go forwards, not backwards

Linked list methods:

- add to head of list: need to add a reference to the former head node,
- remove from head: just set reference to null
- add to middle: reference of new node needs to be to the next item in the list, and the item before must have its reference updated to that of the new node
- remove from middle: set the reference of the node before to point to the node after the one you're removing, then update the reference of the one you're removing to null
- add to the end: make the tail node point to the new node
- remove from the end: set the node before the tail node to null

BigO efficiency:

- accessing, searching: O(n) - still need to go through each node before we get to the one we want, using the pointers to get us there
- inserting, deleting: can be O(n) or O(1), depending on if you're adding to the beginning/end or in the middle

So how are these actually used? In situations where one thing points to another

- music playlists
- photo albums
- can work with queues and stacks behind the scenes

### Doubly linked lists

Regular linked lists could only point forwards, but doubly linked lists can point forwards and backwards.

You don't just have a pointer to the next node, you also have a pointer to the previous node.

Like a linked list, there is a head and tail node. The first pointer of the head node points to a null value, as well as the second pointer of the tail node.

BigO efficiency: the same as for linked lists

Uses - basically whenever you want to go back and forth between things:

- browser caches
- undo/redo
- open recent functionality

### Dictionaries

These are also called associated arrays and maps. So in Javascript, a dictionary is referred to as a map (not the same as the map function).

Dictionaries store data in key-value pairs. Keys can be any primitive data type. Keys must be unique and can only have one value assigned to them. This makes it easier and faster to look up values later. Values do not have to be unique, however.

#### Hash tables

During the lesson on dictionaries the instructor went over hash tables. Yet another term I've heard a lot about and never understood.

Hash tables help you to store keys from a dictionary while cutting down on nil/empty values being taken up in memory. A hash function is what assigns keys to locations in memory so they can be retrieved easily later.

Dictionaries are built on hash tables.

Ok, back to dictionaries.

BigO efficiency: in the case of dictionaries, we can't just use the worst case scenario, things get pretty complicated due to hash tables.

- for the average efficiency, all operations are O(1). That is because they are key-value pairs, so it's quick to look up a value based on its key. You don't have to spend a lot of time searching through your data structure to perform your operation.

Dictionaries are overall very useful data structures that can be used in many situations.

### Conclusion

That's it for the intermediate data structures. The deeper I go into all of this, the more I want to find information on how these all apply to Javascript specifically. I'll finish up the rest of the data structures in this course, and find more information on data structures in Javascript.
