---
layout: post
title: "Algorithms: Boyer-Moore"
date: 2023-07-07 22:30
tags: coding-problems algorithms
navigation: true
class: post-template
current: post
---

Today I learned about a new algorithm while working on a leetcode coding problem.

The problem was to find the majority element in an array of numbers (`nums`) of length `n` - meaning, return the element that appears half the time or more (`n / 2`). Your input is guaranteed to have a majority element.

### My first attempt

I figured I could loop through all the elements of the `nums` array, and for each one, filter the array to contain elements equal to the current value. If the length of the filtered array is the same or more than the majority (`n / 2`), return that element.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
  for (const num of nums) {
    const justNum = nums.filter((x) => x === num);

    if (justNum.length >= nums.length / 2) {
      return num;
    }
  }
};
```

This worked for most of the test cases, but with a very long array, I got a timeout error.

### The solution

I looked at some of the comments in the discussion, and they mentioned that this problem related to the Moore's voting algorithm (or Boyer Moore algorithm).

[I looked it up](https://iq.opengenus.org/boyer-moore-majority-vote-algorithm/).

Usually you perform two passes over your elements. During your first pass you:

- initialize a variable for your `element`, and another for `count`. Set `count` to 0
- for each element `x`, do the following checks
- if `count` = 0, assign `element = x` and increment `count` by 1
- else if: if `element` = x, increment `count`
- else: decrement `count` by 1

The `element` value you are left with should occur the most often out of all elements.

Then you would do a second pass over your elements, to check if `element` in fact makes up the majority of your elements:

- initialize a variable for `count`, set to 0
- for each element, increment `count` if it is the same as the `element` from earlier
- if in the end, `count` is greater than or equal to `n/2`, this is the majority element

### My final solution

I figured I only needed the first pass for my solution, because we were guaranteed a majority element. The second pass only confirmed we had one, so it was not necessary in this case. This time it passed all tests.

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function (nums) {
  let count = 0;
  let elem;

  for (const num of nums) {
    if (count === 0) {
      elem = num;
      count++;
    } else if (elem === num) {
      count++;
    } else {
      count--;
    }
  }

  return elem;
};
```
