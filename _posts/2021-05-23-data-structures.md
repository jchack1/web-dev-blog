---
layout: post
title: "Learning data structures - performance and basic structures"
date: 2021-05-23 14:30
tags: computer-science data-structures
navigation: true
class: post-template
current: post
---

It felt like it was about time to dive deeper into data structures.

This is one of those topics that I knew I needed to learn in more depth eventually, but didn't get around to until now. I was working on a difficult programming problem where I had to convert some data from one structure to another for my front end to be able to use it. I was frustrated because I felt like I had a gap in my knowledge - how do I create and work with these data structures efficiently? Through a lot of trial and error I figured it out, but it likely would have been easier if I had more knowledge of the specific structures I was using - in this case, objects and maps in Javascript.

### What is a data structure?

Turns out my idea of data structures as a concept wasn't too far off - the [free code camp video I'm using to help me learn](https://www.youtube.com/watch?v=zg9ih6SVACc&list=WL&index=7) defined a data structure as something that stores data and allows a user to manipulate that data.

Things you might want to do with a data structure include:

- accessing the data
- searching the data
- inserting a new item
- deleting an item

### Big-O Notation

This helps us to determine how efficient our data structures are. It measures the complexity of an algorithm.

I found [this article that helps explain this concept for beginners](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation).

You essentially score a data structure based on how it performs if you increase the size of the data input into your program.

You give a score to the data structure for how well it can accomplish the tasks mentioned above (access, search, insert, delete). When you are choosing how to store your data, you should choose the data structure that is most efficient at the thing you are mainly using it for.

Performance of a function is based on the <strong>number of operations</strong> it takes to complete its task.

We measure performance by using equations that take in the size of the data set. This equation returns the number of operations. These are called Time Complexity Equations. We don't actually measure time, however, as that can vary depending on the machine you are using to run your program.

#### Most common Time Complexity Equations, from highest to lowest efficiency

n = the size of the data set

The first three are pretty good:

<strong>O(1)</strong>: the best possible score is 1. This means that the task can be completed with one operation. It always takes the same amount of time regardless of the size of the data set it is working with.

<strong>O(log n)</strong>: still very fast; the larger the data set, the more efficient it is. Follows a logarithmic curve (the inverse of an exponential curve, if like me you don't remember much about logarithmic curves from school).

- example algorithm that uses this: binary search

<strong>O(n)</strong>: the number of operations needed is proportional to the number of items in the data set. This would be represented by a linear graph.

The next three are not so good:

<strong>O(n log n)</strong>: not quite linear, performance gets slightly worse as the size of the data set increases

<strong>O(n^2) and O(2^n)</strong>: gets exponentially worse as the size increases

- n^2 can happen with nested loops
- you could end up with n^3, n^4 and larger if you have even more code nested with in your nests

### Basic data structures - arrays and array lists

Arrays and array lists are random-access data structures. This means you can access any element inside them directly without depending on any of the other elements.

#### Arrays

- can be used to store anything, usually similar values
- items in an array are called elements
- indexes are used to identify the elements in the array, starting at index 0

Parallel arrays: multiple arrays have corresponding values, like employees and their salaries

There was a point when I started to become confused while watching the freeCodeCamp video. They say that an array can store data of only one type. This isn't true in Javascript, as it is an un-typed language. You can also add and remove items in a Javascript array, so the size is not always fixed; in the video, fixed-size was an important aspect of the array data structure. If my understanding of data storage is correct, this means that you cannot reserve continuous spaces in memory for all your array elements (in Javascript). This could affect how the data is accessed and therefore its performance.

Moving on...

You can have two-dimensional arrays:

- at each index of the first array, there is another array
- looks like a spreadsheet
- use two indexes to access each item, first for column and second for row

BigO efficiency for arrays:

- accessing: O(1)
- searching, deleting, inserting: O(n)

#### Array lists

This began to clear up my confusion about Javascript arrays and size. An array list has the same properties as an array, except it is dynamic and can grow. Exactly what I'm used to with Javascript. So, in Javascript, is an "array" actually an "array list"? That is something I need to dive into further.

- array lists usually use arrays in the background. Some languages, like Python (and possibly Javascript?) mix properties of arrays and array lists.
- has built-in methods. Javascript has methods like pop, unshift, and length, to name a few.
- in memory, array list items are stored as references to other places in memory - the actual element values are stored elsewhere in memory

BigO efficiency for array lists:

- accessing: O(1)
- searching, inserting, deleting: O(n)

For both arrays and array lists, searching, inserting, and deleting will require us to go through each item to complete the task (you could also find the correct element on the first try, but for BigO notation we look at the worst case scenario).

Use arrays if:

- you won't be changing the size or modifying the data

Otherwise, array lists are more powerful and a better choice. As we can see above, the efficiency in terms of BigO are the same.

### Conclusion

That's all for the basics. The next posts will get into the intermediate and advanced data structures.
