---
layout: post
title: "Another way to delete element from a string"
date: 2023-07-08 15:30
tags: javascript coding-problems
navigation: true
class: post-template
current: post
---

When I have solved a problem on leetcode I like to look at other users' solutions to learn other ways of solving the problem.

Today I learned a different way of deleting elements from a string.

Often when I am working with strings and I need to delete parts of a string, I will convert to an array using `split("")` and then delete at a specific index using `splice(index, 1)`. Splice mutates arrays in place, so it can be helpful if I need to keep track of the value after several deletions.

This time the coding problem was to check if one string was an anagram of another (meaning, all letters in string A are used in string B exactly once, and no other letters are added).

Someone's solution used `replaceAll(x, "")`. In this case, you need to know exactly what value(s) you want to delete, rather than an index, and as the name suggests it will replace all the values that match your first argument. Replacing a value with an empty string `""` effectively deletes it. Somehow that's something I've never really thought about before, even knowing that empty strings are falsy values.

My solution involved splitting, sorting, and rejoining each string, and then comparing the results, which still does the job:

```js
var isAnagram = function (a, b) {
  if (a.split("").sort().join("") === b.split("").sort().join("")) return true;

  return false;
};
```

However, the other person's solution saves a lot of steps, using only a while loop and the `replaceAll()` method.

Basically, while `a` and `b` are the same length, get the first letter of `a`, and remove it from both `a` and `b` wherever it is encountered. If the strings continue to be the same length after this, repeat. If they are ever not the same length, that means they don't have all the same letters, so they are not anagrams. Otherwise, once both strings reach a length of 0, you have determined all the letters in each string matched, so you have an anagram.

There were a lot of possible solutions, but I liked this one for its simplicity.
