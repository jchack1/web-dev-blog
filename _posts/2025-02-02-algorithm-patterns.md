---
layout: post
title: "Problem solving patterns"
date: 2025-02-03 16:30
tags: computer-science problem-solving coding-problems
navigation: true
class: post-template
current: post
published: false
---

Today I am moving onto the next section of the course, which is discussing common problem solving patterns.

### Frequency counters

Using an object, or set, to gather values or frequencies of values (O(n) time). You are creating an object with information about the content of an array or string. Each key in the object is a value in the array, and its value is how frequently it occurs. You can then compare the objects.

It's all about building a dictionary to track values in at least one of the inputs, and is often used to avoid nested loops (O(n^2) situations).

Steps:

1. Check for edge cases or sitations where you can return early - strings/arrays different lengths, nulls, undefined
2. Initialize lookup object, and loop over the first input, filling the lookup object. You could end up with {a:1, b:3, c:2}
3. Loop over the second input, comparing it to the look-up object. Each time you encounter the same value in the lookup object, decrement that value in the lookup by 1. If the value does not exist in the lookup object, or the value is 0, return false.

```js
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;

  const lookup = {};
  for (value of firstArr) {
    lookup[value * value] = (lookup[value * value] || 0) + 1;
  }
  for (secondValue of secondArr) {
    if (!lookup[secondValue]) return false;
    lookup[secondValue] -= 1;
  }
  return true;
}
```

### Multiple pointers

Create pointers, or values, that correspond to an index/position, then move to the beginning, end, or middle towards each other.

Could be arrays, strings, linked lists.

Usually looking for a pair of values, using two references (variables). Each reference starts in one places, and moves through the data structure based on a certain condition.
