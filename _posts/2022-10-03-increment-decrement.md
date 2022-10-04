---
layout: post
title: "What's the difference between 'i++' and '++i'"
date: 2022-10-02 18:30
tags: computer-science javascript
navigation: true
class: post-template
current: post
---

I have always been familiar with incrementing a variable using `i++`, but recently came across `++i`. It took me a while to truly understand the difference.

`i++` is used as a shorthand for writing `i = i + 1`. `++i` does the same. The difference between the two is in what value is returned from this operation.

```js
const returnIncrementPrefix = (i) => {
  return ++i;
};

returnIncrementPostfix(5); // returns 6
```

In the example above, we use the prefix `++i`. When 5 is used as an argument in this function, 6 is returned. Knowing that `++i` increments by one, this seems to make sense.

What happens when we use the postfix?

```js
const returnIncrementPostfix = (i) => {
  return i++;
};

returnIncrementPostfix(5); // returns 5
```

The above code returns a value of 5. At first this doesn't make sense. We are supposed to be incrementing by 1, right? Shouldn't this also equal 6?

Nope! In the first example, these steps occur:

- i is incremented by 1, giving i a value of 6
- i is returned

We are returned the value of i <em>after</em> it has been incremented.

In the second example using the postfix, i is being returned <em>before</em> it is incremented. The operation doesn't happen until after 5 has been returned to us.

It's all about the order in which the steps occur.

#### Tip for remembering

If you need help remembering which operation does which, think of it this way. If the "++" comes <em>before</em> the i, incrementation happens before it is returned. If the "++" comes <em>after</em> the i, the incrementation happens after it is returned.

Of course, this applies to other operations, including decrementation.
