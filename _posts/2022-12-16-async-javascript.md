---
layout: post
title: "Notes about asynchronous JavaScript"
date: 2023-01-16 16:30
tags: javascript
navigation: true
class: post-template
current: post
---

I work with async JavaScript a lot, but I wonder if I know everything about it that I need to.

I'm working through my Udemy course again ("The Complete JavaScript Course 2023: From Zero to Expert") and will share some notes below.

### AJAX and XML

AJAX: Asynchronous JavaScript and XML

- [MDN page](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX)
- make requests to servers and updates to the UI without having to refresh the page

The old way: XML. We used to use XML, but JSON is now most common. It's easier to use JSON data in JavaScript code, plus it's smaller in size than XML.

- you can get any type of data, not just XML - [MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

How that works:

```js
const request = new XMLHttpRequest();

request.open("GET", url); //whatever url you are fetching from
request.send();

//use callback function to do something once we get the data
request.addEventListener("load", function () {
  const data = JSON.parse(this.repsonseText);
});
```

### Promises

The above code works fine when you only need to make one request. However, what if you need to make multiple requests? What if they need to be made in a particular order, or subsequent requests rely on earlier ones? In this situation you end up with callbacks inside of callbacks inside of callbacks - callback hell. The code becomes unmanagable at this point.

Since ES6 you can avoid this by using promises:

```js
//no need for event listeners or callbacks
const request = fetch(url); //whatever url you are fetching from
```

This code returns a promise. Promises are used as placeholders for the result of the async request. The fetch API used above builds (creates) the promise for us to consume.

Promise lifecycle/states:

- pending: waiting for the value to be returned
- settled: async task finished
  - fulfilled: a value was returned to us
  - rejected: some error happened

We can more easily chain promises using the ES6 `.then` method.

To handle errors/rejected promises, we use the `.catch` method. It will catch errors that occur anywhere in the `.then` promise chain.

There is also a `.finally` method, that is run no matter what happens with the promise.

```js
//when the promise is settled, use .then and .catch
const request = fetch(url)
  .then((response) => {
    //need to parse the data
    //this is also a promise, so we need another .then
    return response.json();
  })
  .then((data) => {
    //do something with the data
  })
  .catch((err) => {
    //do something with the error
  })
  .finally(() => {
    //do something no matter what
  });
```

You can also handle specific errors with:

```js
throw New Error()
```

`.catch` method will catch these errors too.

### The event loop

How does the JavaScript runtime work?

- JS engine includes the heap (where things are stored in memory) and the call stack (where the code is executed)
- JS is single threaded - meaning it handles one operation at a time. JavaScript doesn't multitask.
- runtime also includes various web APIs, like DOM, fetch, event, geolocation, etc. These are not a part of the language, just provided to the JS engine to use
- Callback queue: a data structure that holds callback functions that are attached to events. When the call stack is empty, the <strong>event loop </strong> takes callbacks from the queue and adds them to the call stack.
  - when an event is emitted, it is placed in the callback queue
  - the first item in the queue is taken by the event loop and placed on the call stack
- this is how we can have asynchronous behavior in JavaScript
- Microtasks queue: for promises. Acts similarly to callback queue, but actually has priority over it.

Concurrency model: how JS handles multiple tasks in a <strong>non-blocking</strong> (not waiting for other operations to finish running) way. The question is, how can this actually happen if the language is single threaded?

Some async tasks run in the Web APIs environment, not on the call stack. For example, loading an image, fetching data, etc. So it's not happening on our thread of execution.

### Creating a promise

Use the promise constructor. It takes in an executor function with two parameters: resolve and reject.

```js
const myPromise = new Promise(function (resolve, reject) {
  if (Math.random() > 0.5) {
    resolve("You win!");
  } else {
    reject("You lose!");
  }
});

myPromise.then((res) => console.log(res)).catch((err) => console.log(err));
```

Promisifying: to convert callback behavior into promise behavior. We can then consume the promise like above instead of using callbacks.

### Async/await

Place `async` in front of function keyword. The function will return a promise. `await` statements are placed inside the function.

`await` stops code execution and waits until the promise is returned. But, because of the `async` keyword, this is happening inside an asynchronous function, and this execution is happening in the background. The call stack is not being blocked while waiting for this promise. Everything is happening behind the scenes, it's just this syntax makes it look synchronous. Promises are still being used.

Now you can assign resolved values from promises to variables, like we'll see below.

```js
const myFetchFunction = async function () {
  //below, res is the resolved value of the promise
  const res = await fetch(`https://mylink.com`);

  const data = await res.json();

  return data;
};

const data = await myFetchFunction();
```

`await` keyword can currently only be used inside of async functions. You could wrap your code inside of an IIFE to get around this if needed.

### Try/catch

Use try/catch blocks for error handling. This is comparable to the `.then` and `.catch` syntax, but when we use async/await, there is nothing to chain the `.catch` onto.

Wrap your code in a try block. If an error occurs in the try block, the catch block code is executed.

```js
const myFetchFunction = async function () {
  try {
    const res = await fetch(`https://mylink.com`);

    //error gets caught in the catch block
    if (!res) throw new Error("Problem getting data");

    const data = await res.json();

    return data;
  } catch (e) {
    return e;
  }
};

const data = await myFetchFunction();
```

### Awaiting multiple promises at once

What do you do when you are waiting for several promises at once, but you don't need them to arrive in order? If you are making several requests, one after the other in your code, they will be fetched one after the other.

You can use `Promise.all()` to make sure they're fetched concurrently. Place all your promises in an array:

```js
const data = await Promise.all([
  getJSON(`https://mylink.com/1`),
  getJSON(`https://mylink.com/2`),
  getJSON(`https://mylink.com/3`),
]);
```

Now, these requests will be running at the same time, instead of synchronously.

The only problem is if one fails, the whole operation fails.

#### Some other options

`Promise.race()`: resolved when the first one is resolved

`Promise.allSettled()`: you get the results of all promises, even if they are rejected

`Promise.any()`: similar to Promise.race, but ignores promises that reject
