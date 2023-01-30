---
layout: post
title: "Writing pipe and compose functions - advanced JavaScript"
date: 2023-01-30 12:30
tags: javascript
navigation: true
class: post-template
current: post
---

I recently went down a rabbit hole while learning about pipe and compose functions in JavaScript. This is a fairly advanced topic and needs a lot of prerequisite knowledge to grasp.

I was testing my knowledge by going over technical JavaScript questions on https://30secondsofinterviews.org/ when I came across this one: “Create a function pipe that performs left-to-right function composition by returning a function that accepts one argument.
“

The term “pipe” was vaguely familiar, as was “function composition”, but I honestly had no idea what this question was asking me for. I started by googling “left-to-right function composition”.

This led to several tutorials about writing <strong>pipe</strong> and <strong>compose</strong> functions. I saw a lot of `reduce` and `reduceRight` functions being used. I hadn’t seen reduceRight before. These pipe and compose examples used an arrow-function syntax I didn’t quite understand either, and it looked something like this:

`const multiply = x => y => x * y`

What’s with this extra parameter?

Seeing it all written out like this wasn’t making sense, so I found myself watching a tutorial on youtube about pipe and compose. The instructor used the above syntax in his video and called this a <strong>curried function</strong>, and directed viewers towards a video on that topic. That video made reference to <strong>pure functions</strong> and <strong>functional programming</strong> and said these were important topics to understand curried functions.

All of that to say there was a lot to learn before even being able to tackle the original question. I’ll go over everything I learned briefly below, because I know I’ll need to come back to this one day.

Here’s a quick list to summarize:

- pure functions and functional programming
- `reduce` and `reduceRight`
- curried functions
- pipe/compose and function composition

### Pure functions and functional programming

These topics are related to each other.

Pure functions are those that will produce the same result given the same arguments, and will not affect anything in the global scope (no side effects). They won’t change the state of anything outside of it. They won’t mutate variables elsewhere in the program. To achieve this, you generally need to keep your functions as small as you can, using simple, reusable blocks of code.

Functional programming builds software using pure functions. It is declarative rather than imperative.

Wait… what does that mean?

#### Imperative vs Declarative programming

<strong>Imperative</strong> programming focuses on <em>how</em> the program works, the process of getting your result. <strong>Declarative</strong> programming focuses on <em>what</em> you are getting out of the program. Imperative is about the steps involved, declarative is about the end result. Imperative uses more control flow in its code.

We could write a whole article on that, maybe a whole course, but let’s move on.

My understanding of functional programming is that we create a lot of simpler functions that can accomplish one main task, and we use those functions together throughout our program to do more complex things. It’s like making a larger program with smaller building blocks of code. Your program will look less like instructions to the computer with lots of control flow, and more like a series of expressions, of functions being called.

Again, to be more accurate we could do an even deeper dive into this topic, this is just a general idea that was helpful for me.

### Reduce and reduce right

Reduce is a difficult method to understand. Basically what it does is takes in an array, and for each item in the array, it runs some code, and adds the result to an accumulator - “reducing” it down to another value.

The go to example of this always seems to be a “sum” function

```js
const array = [1, 2, 3];

const sum = array.reduce((accumulator, x) => accumulator + x, 0);
```

In this example, we perform reduce on the array. Reduce takes in an accumulator, the thing you’re boiling it all down to, and the array item (x). For each array item we run this code: `accumulator + x`, and that value is returned from that iteration. On the first iteration, we have to initialize the value to something. In this case, since we are just adding some numbers together, we set it to 0. In the first iteration, the accumulator is 0, so we are doing “0 + 1”, and returning a value of 1. The value of the accumulator is now 1. We keep doing this until we have gone through each item in the array.

Notice that reduce goes through the array in order from <strong>left to right</strong>. We start with the left-most value, at the 0 index.

In reduceRight, we do the opposite. We start with the last value in the array. As far as I can tell that’s the only way they are different.

Reduce is used for “pipe” functions and reduceRight is used for “compose” functions, which we’ll see later.

### Curried functions.

This can get a bit complicated so I’ll summarize as best as I can. This video and this article were very helpful for me to figure this out.

- [https://www.youtube.com/watch?v=I4MebkHvj8g](https://www.youtube.com/watch?v=I4MebkHvj8g)
- [https://medium.com/dailyjs/why-the-fudge-should-i-use-currying-84e4000c8743](https://medium.com/dailyjs/why-the-fudge-should-i-use-currying-84e4000c8743)

If I try to describe it in my own words, currying functions is when you create a function that takes in multiple parameters separately, and these are used in separate steps of your function.

Most functions I’ve seen up until now take in all arguments at once, then run some code, returning one final result.

This doesn’t happen for curried functions. Within your function, the first argument is used in some code that returns another function. The function that is returned can use the second argument input into the function. And so on. If you were to write it all out line by line, you would end up with a lot of nested functions.

If you only give the function one parameter, the function can’t complete fully. The result returned is just another function, that can take another parameter to produce a result. This is called a <strong>partial application</strong>

There are some good use cases for this, explained in the article. For example, you could use the first part of the function to do some expensive calculation or fetch, save that to a variable, then use the resulting partial application with different parameters.

This probably doesn’t make a lot of sense without examples so please see the resources above.

For now I’ll try to explain it with the multiplication example above.

`const multiply = x => y => x * y`

To completely use this function, you might try something like `const myNumber = multiply(2)(3) //6`

You can place the arguments directly after each other, invoking it immediately

But let’s go step by step. What does `multiply(2)` give us?

`const times2 = multiply(2) // y => 2 * y`

It returns a function where 2 is multiplied by our y parameter

The next step is passing in the y value

`const myNumber = times2(3) //6`

Let’s say we know that we might need to multiply a lot of different values by 2. We can partially apply the curried function with 2 as the first parameter. We are then left with a function that can multiply any value we give it by 2.

Also, not that this is very important, but “curried/currying” has nothing to do with the food. It comes from the logistician Haskell Curry.

### Pipe and compose

Another video by the same youtuber was helpful: [https://www.youtube.com/watch?v=kclGXphtmVg](https://www.youtube.com/watch?v=kclGXphtmVg)

Now we can put everything we’ve learned together to create pipe and compose functions.

Pipe and compose functions both take in several simple pure functions, and “compose” them together to create a more complex function. This more complex function would ideally have just one parameter.

Both are curried functions that take in arguments at separate steps. The first argument is an array consisting of the pure functions. The second argument is the value of the single parameter the function will be acting upon.

For pipe, the `reduce` function is used to accumulate the array of pure functions from left to right. For compose, the `reduceRight` function is used to accumulate the pure functions from right to left. That’s all that is meant by left to right and right to left in this context.

Here’s how you would write the pipe function:

` const pipe = (...fns) => val => fns.reduce((acc, fn) => fn(acc), val)`

And here are our pure functions and what we want our resulting function to look like:

```js
const square = (v) => v * v;
const double = (v) => v * 2;
const addOne = (v) => v + 1;

const result = pipe(square, double, addOne);
```

`…fns` is your array of pure functions. It will look like: `[square, double, addOne]`

`val` is the argument that will be passed into the resulting function.

`acc` is the accumulator, and `fn` is the current pure function from our array that reduce is iterating upon.

The accumulator is initially set to `val`.

That means the first iteration, going left to right in our array, will look like: `square(val)`. `acc` is now equal to `square(val)`

`acc` is the now the argument for our next function, which is `double`. We get `double(square(val))`. That’s the new value of the accumulator.

Next we use `addOne(acc)`, which ends up looking like `addOne(double(square(val)))`

#### let’s look at those steps again but with `val = 3`

1. square(3) = 9
2. double(9) = 18
3. addOne(18) = 19

If we want to use a compose pattern instead of pipe, we would reverse the order of the pure functions in the array, and use `reduceRight` instead of reduce.

### Conclusion

There is of course a lot more complexity here than I’ve explained, and it’s possible my explanations are not 100% accurate. I personally found it difficult to understand the concepts when they were written with such technical language, and without the steps being explained. You can find all of that online already, so I didn’t want to just repeat that information. Definitely check out some other explanations in articles and videos for more detail.
