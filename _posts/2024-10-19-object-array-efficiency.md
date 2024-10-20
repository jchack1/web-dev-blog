---
layout: post
title: "Big O For JavaScript Objects and Arrays"
date: 2024-10-19 18:30
tags: computer-science data-structures javascript
navigation: true
class: post-template
current: post
---

Quick post to summarize different operations on objects and arrays.

This content came from a lesson in JavaScript Algorithms and Data Structures Masterclass on Udemy.

### Objects

Objects are unordered collections of key-value pairs. If you have the key, you can retrive the value immediately without searching the entire object. This is constant time, O(1).

It's the same thing for inserting new items. Order doesn't come into play, you're just adding a new item into the object, so it's constant time.

`Object.keys()` and `Object.entries()`: since this converts an object to an array, so you can iterate over it, it is O(n) time. That's because you have to go through every item in the object in order to create the new array.

Objects are great for when order doesn't matter and you just need to look things up.

### Arrays

Arrays are more useful for when you need order and/or fast access. If you know the index of the item you're looking for, you don't have to search the whole array - you can retrieve it right away, making it constant time O(1).

Searching the entire array is O(n), because we are considering the worst case scenario - that the item you're looking for it at the very end, and you have to go through all the other items first.

Insert or removing an item depends on <em>where</em> in the array you are adding/removing from.

If you are adding to the end (`push()`) or removing from the end (`pop()`), you don't have to touch any other items in the array. These are constant time O(1).

However, if you are adding (`unshift()`) or removing (`shift()`) from the beginning of the array, you now have to re-index every other item in that array.

Let's say you added one item to the beginning. It is now index 0. The next item in the array, that was previously index 0, now has to be re-indexed to 1. And so on. Since you are updating every item in the new array, time complexity is O(n).

In general, any built in JavaScript method that acts on (potentially) every item of an array is O(n), including:

- `splice()`
- `slice()`
- `concat()`
- `filter()`
- `map()`
- `forEach()`

`sort()` is O(n log(n)) - because there are comparisons and other complex operations going on for each element in the array:
