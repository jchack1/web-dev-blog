---
layout: post
title: "How to clone a JavaScript object"
date: 2022-10-18 18:30
tags: javascript
navigation: true
class: post-template
current: post
---

There are several different methods that can be used to clone an object in javascript.

### The spread operator

The spread operator `...` was an ES6 addition and it's very useful for cloning. It creates a <em>shallow copy</em> of the object.

```js
const obj = {
  a: "apple",
  b: "orange",
  c: "peach",
};

const clone = {...obj};
```

### Object.assign()

Also an addition from ES6 is `Object.assign()`, where properties from a source object are copied to a target object. This also creates a shallow copy of the original object.

```js
const obj = {
  a: "apple",
  b: "orange",
  c: "peach",
};

const clone = Object.assign({}, obj); //first argument is the target
```

### Shallow vs deep copies

The difference between shallow and deep copies is that shallow copies share the same references as the original object, but a deep copy has entirely new references. This will often not make a differnce to your code, but it can be important in some cases.

1. When you need the original object's values to remain the same, and you are going to be changing the values of the cloned object.
2. When you have a nested object.

### Making a deep copy

To create a deep copy we can use `JSON.parse()` and `JSON.stringify()` together.

```js
const obj = {
  a: "apple",
  b: "orange",
  c: "peach",
};

const clone = JSON.parse(JSON.stringify(obj));
```

It's important to note that this doesn't work in all cases, because there are some javascript objects that cannot go through `JSON.stringify()`, like functions and DOM objects. You would need to use shallow copies in this case. You can read more about this in the [MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Deep_copy).
