---
layout: post
title: "Binary math - addition problem"
date: 2023-07-02 22:30
tags: binary computer-science javascript coding-problems
navigation: true
class: post-template
current: post
---

Lately I've been working on some practice coding problems, and I needed to re-learn how to do math with binary numbers.

The problem was to simply add two binary numbers - the inputs, `a` and `b` were strings, and the output needed to be a string as well.

There are better explantions about binary and math out there, these are just some notes in my own words to help me in the future. Crash Course on youtube has a great series on computer science, and they [have an excellent video explaining binary here](https://www.youtube.com/watch?v=1GSjbWt0c9M).

### How binary works

Every day we deal with the decimal number system, which has 10 digits we can work with: 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9. We also call this base 10. Binary has only two digits, 0 and 1. We can call it base 2.

When we are solving a simple addition problem on paper, we start with the 1's place, working from right to left. We add the numbers in the 1's place, and if the sum is 10 or greater, we have to carry a value. We add that value to the numbers in the next column, the 10's place, and continue from there. Each column increases by a factor of 10, since we are in base 10.

We can use the same method with the binary system, it's just the value of each column is different. Since we are in base 2, each column increases by a factor of 2. Instead of 1, 10, 100, 1000... we have, again from right to left, 1, 2, 4, 8, 16, 32, and so on. You add the columns in the same way.

### Adding

Let's say you are adding the following binary numbers:

111
+10

The right-most column is the 1's column. 1 + 0 is 1, so your first value is 1. There is nothing to carry.

Onto the next column, we have 1 and 1. This adds to 2 (10 in binary), so our value we write down is 0 and we carry the 1 to the next column (remember, we only have 0 and 1 to represent our values). Then for our next calculation, we add 1 plus our carried over 1 to get 2 (10 in binary). There are no more columns, so we write down 10.

This gives us 1001.

### Converting to base 10

In the example above, our result spans the 8, 4, 2, and 1 columns.

Because we have a 1 in the 1 column, and a 1 in the 8 column, we can add those together to get a base 10 value of 9.

### My initial solution

I figured I would just loop through each column of the strings I was given, trying to replicate the on-paper addition process. I ended up with this:

```js
//a and b are strings
//returns a string
var addBinary = function (a, b) {
  let binaryStr = "";
  let carryValue = 0;

  const longestStringLength = a.length > b.length ? a.length : b.length;

  //as many iterations as the length of the longest string - that's how many places our binary number has
  for (let i = 0; i < longestStringLength; i++) {
    //iterate over string from right to left - it's highest index to lowest
    const calc =
      (Number(a[a.length - 1 - i]) || 0) +
      (Number(b[b.length - 1 - i]) || 0) +
      carryValue;

    //add calc to the beginning of binaryStr, carry value if needed
    if (calc > 1) {
      binaryStr = (calc - 2).toString() + binaryStr;
      carryValue = 1;
    }

    if (calc <= 1) {
      binaryStr = calc.toString() + binaryStr;
      carryValue = 0;
    }
  }

  //account for a carried over value from last opertion, if there is one, otherwise return
  if (carryValue === 1) {
    return "1" + binaryStr;
  }

  return binaryStr;
};
```

### A better (or shorter) solution

I looked at some other solutions and found that JavaScript has prefixes to convert other number systems to base 10. In this case, we add "0b" to the beginning of our strings, convert to numbers, add together, and return a string.

```js
var addBinary = function (a, b) {
  const calc = BigInt("0b" + a) + BigInt("0b" + b);

  return calc.toString(2);
};
```

`BigInt` was used here to handle potential large inputs. Also, to return a string in binary rather than base 10, we add a radix to the `toString()` method. 2 means base 2 in this case.

### Prefixes

There are prefixes for other number systems too:

- `0o` for octal
- `0x` for hexadecimal
- `e` for exponents

The letter after the leading zero can be uppercase or lowercase.

### toString() radix

`toString()`, be definition, returns a string value that represents a number. It is meant to convert a number to a string, so it makes sense that you can convert between different number systems at the same time. Including a radix is optional, and defaults to 10, but it can be any value between 2 and 36.
