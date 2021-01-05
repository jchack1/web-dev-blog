---
layout: post
title: "Some JavaScript array methods, how they work, when to use them"
date: 2020-12-14 19:30
tags: javascript
navigation: true
class: post-template
current: post
---

Here are a few JavaScript array methods that I recently learned about in my JavaScript course. Some are relatively new additions to JavaScript and some are not, but I have learned about them all in a little more depth.

### some (vs includes)

The "some" method tests if <strong>any</strong> item inside an array meets a certain condition. If any item meets your condition it returns "true".

```js
const numbers = [1, 3, 12, 17, 4];

numbers.some((num) => num > 10); //returns true, since 12 and 17 are greater than 10
```

This is similar to the "includes" method, except "includes" tests for equality. It will test if any element in your array is equal to whatever value you use inside the method.

```js
const numbers = [1, 3, 12, 17, 4];

numbers.includes(10); // returns false since there is no value of 10 in the array
```

### every

The "every" method tests if <strong>every</strong> item in an array matches a specific condition.

```js
const numbers = [1, 3, 12, 17, 4];

numbers.every((num) => num > 10); //returns false, not every item in the array is greater than 10
```

### flat

Let's say you have a nested array - an array of arrays. And you would like all the values in one array. The "flat" method flattens the nested arrays into just one array with all the items on the same level. HOWEVER, it only goes one level deep when used on it's own.

```js
const numbers = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];

numbers.flat(); //returns [1,2,3,4,5,6,7,8,9]

//for nesting more than one level deep, add a value
//into the flat method that defines the number of levels you want to go
const numbers2 = [
  [[1, 2], 3],
  [4, 5],
];

numbers.flat(2); //returns [1,2,3,4,5]
```

### flatMap

The flatMap method just combines the "flat" and "map" methods together. It works the same way as the map method - calls a funtion on each item in an array and returns a new array - and additionally flattens the result.

It also only goes one level deep, however, and you can't change that. If you need to flatten multiple levels you should use the flat method.

### sort

The sort method sorts based on strings. Even if an item is a number, it is converted to a string, then sorted. Numbers don't actually end up being sorted from lowest to highests as you would expect. You need to add a callback function into your sort method in order to sort numbers.

Below is an example similar to what was used in my course

```js
const numbers = [5, -100, 600, -440, 200, 50, -250];

//if A should be before B, return < 0  (can be any negative number) -> keep order
//if B should be before A, return > 0  (can be any positive number) -> switch order

numbers.sort((a, b) => {
  if (a > b) return 1;
  if (b > a) return -1;
});

//returns [-440, -250, -100, 5, 50, 200, 600]

//the same thing can also be acheived this way:
numbers.sort((a, b) => a - b);
```

sort compares two values at a time. In this case we want to sort in ascending order, or lowest to highest. We want A to be less than B. If A is greater than B, 1 is returned, and we switch the order of the values. If B is greater than A, -1 is returned, and we keep the current order of the values. We loop over the array until it is sorted according to our conditions.

You can also see a shortened method included.

If you want ascending order, use a - b. If you want descending order, use b - a.

### fill

Mutates the entire array. It fills an empty array with values, or replaces values of an already existing/full array. Like slice, you can specify where it begins and ends filling the array

```js
const arr = new Array(5); //creates an array with 5 empty places

arr.fill(1); //fills the array with 1s, returns [1,1,1,1,1]

//another option
arr.fill(1, 3, 5); //returns [_,_,_,1,1]
```

The first parameter is the <strong>value</strong> you want to fill the array with. The second parameter is the index you want it to start filling at, and the third parameter is the index where it should stop.

### Array.from

from is used on the Array constructor. It takes in the length of array you would like, and a function similar to map. The params you can use are the current and index values.

```js
const arr2 = Array.from({ length: 5 }, (cur, i) => i + 1);
//returns [1,2,3,4,5]

const diceRolls = Array.from({ length: 100 }, () =>
  Math.floor(Math.random() * Math.floor(6) + 1)
);
//returns array of 100 numbers between 1 and 6
```

You can use this method to convert something that is not an array into an array.

### When to use each type of array

This comes from the summary lecture of my JavaScript course

Methods that mutate the original array:

- push
- unshift
- pop
- shift
- splice
- reverse
- sort
- fill

Methods that create a new array based off an original:

- map
- filter
- slice
- concat
- flat
- flatMap

To get an index:

- indexOf (based on a value)
- findIndex (based on a condition)

To get array element:

- find

Does an array include something? Returns true or false

- some
- every
- includes

Make array into a string:

- join

Boil something down to one value:

- reduce

To simply loop over an array without producing a new value/array:

- forEach
