---
layout: post
title: "Learning Jest - some basics"
date: 2024-10-17 16:30
tags: javascript testing
navigation: true
class: post-template
current: post
---

I'm starting to get into testing with Jest for a work project.

We landed on using Jest and React Testing Library with our React project, as it works nicely (so I've heard) with apps set up with create-react-app.

I realize there are new build tools like Vite (with testing library Vitest), and that create-react-app is not getting new updates these days, but this is what works best with our current project set-up.

So, here are some notes on the basics I learned about Jest. This follows what was taught in the Learn React Testing course on Codecademy.

### Setting up your tests

Jest is meant primarily for unit testing. For our project we are planning to use it to test utility functions and other smaller pieces of code.

You can keep the tests in the same folder with the file it is testing, or have a separate directory, called `__tests__`. I like this option so all the tests are in one place and not cluttering the rest of the code. You can copy the file structure of the project in the tests folder to make it easy to navigate.

When creating a test file, name it the same thing as the file you are testing. Files must end with .test.js, .spec.js, or be located in the `__tests__/` directory.

For example, if the function you're testing is `getVehicleSpeed.js`, the test file would be called `getVehicleSpeed.test.js`

In your terminal, running the command `npm test` runs your tests in watch mode, so the tests will run every time you save. When the tests run, they print the results to the console. There are different flags you can use with this command, like `npm test -- --coverage` which gives a detailed breakdown of how much of your code is covered by tests.

### Unit testing

Now getting a bit more into the implementation...

For unit testing, we are testing individual functions. In Jest you can use the `it()` or `test()` function - they do the same thing.

`it()` takes three arguments:

1. a string describing the expected result of the test
2. a callback function with testing logic
3. an optional timeout

### How to set up the test file

Import the functions you are testing into your test file.

In this file we use <strong>assertions</strong> which define the expected behavior of the function.

An example of this is the built-in `expect()` function. Whatever we pass into `expect()` is an expression we want to test.

Assertions are ususally used with matcher methods like `toBe()`, into which we pass the expected result

In the callback function of `it()`, follow this pattern: Arrange, Act, Assert

<strong>Arrange:</strong> setting up your variables and conditions

<strong>Act:</strong> invoke the function you are testing, using variables from the "arrange" stage as input. Save the result of this function to a variable

<strong>Assert:</strong> check if we got the expected result by using methods like `expect()` and `toEqual()`

#### Matcher methods

Matcher methods are ways of testing our assertions, where we put the result we're expecting

Common ones include:

- toBeDefined
- toEqual - does deep equality checks
- toBe - compares primitive values
- toBeTruthy
- not - gives oppositve result
- toContain - check that an item is in an array

Example:

```js
import add from "./add";

it("adds 1 and 2 to equal 3", () => {
  const a = 1;
  const b = 2;

  const result = add(a, b);

  const expected = 3;

  expect(result).toBe(expected);
});
```

### API calls/async tests

When using asychronous code with callbacks, don't add assertions to callback functions. They don't work. Jest doesn't know that it should wait for a test to fail before moving on to the next test.

We can use `done()` as a parameter of our `it()` function. This is called after the expect() assertion. Jest knows to wait until `done()` is called before ending the test. So basically, for async calls, add `done()` after the function call. It is good to do this inside a try/catch, so we can catch any errors, and pass them to `done(error)`. If we didn't have this, done would never be reached, so the test would eventually fail due to a timeout error.

Example:

```js
const fetchUserData = require("./api");

it("fetches user data asynchronously", (done) => {
  const userId = 1;

  try {
    fetchUserData(userId, (data) => {
      expect(data).toEqual({id: userId, name: "John Doe"});
      done();
    });
  } catch (error) {
    done(error);
  }
});
```

If using async/await, don't need to use `done()` - just use async/await for code that returns promises:

```js
const fetchUserData = require("./api");

test("fetches user data asynchronously", async () => {
  const userId = 1;

  const data = await fetchUserData(userId);
  expect(data).toEqual({id: userId, name: "John Doe"});
});
```

### Mocking api calls

This is for testing the functions that call the api, not testing the api itself.

We bypass the api and return values instead. In our tests, we create a mock function and use it in place of our real function.

This is done in case there are issues getting the data from the api. I think we are assuming that there is a lot of code involved in our function, and the api call is a small part of it that we don't want blocking our tests from working properly.

Steps for creating the mock function:

1. Create a directory **mocks** - test will look for it here
2. Create a file with the same name as the function being mocked
3. Create a module, use jest.fn() to create the function we want to mock
4. Export module

The mock function looks like this:

```js
const httpRequest = jest.fn(() => {
  return Promise.resolve({
    status: ``,

    data: [],
  });
});

export default httpRequest;
```

Steps to use a mock function in your test file:

1. import the real function into the test file
2. wrap `jest.mock()` around the file path of the real file

```js
//import the actual function
import httpRequest from "./utils/http-request";

//before the it() function, we tell the test file we want to use our mock function, instead of the one we imported
jest.mock("./utils/http-request");

//then inside your it(), call the function by the name it was imported as
it("testing http request", () => {
  ///some code here

  httpRequest.mockResolvedValueOnce(resolvedValue);

  //more code below
});
```

Truthfully, I am a bit confused by we need to place a mocks folder near the function we are testing in the code. I'd like to keep everything in the `__tests__` directory. I found [this stackoverflow post](https://stackoverflow.com/questions/51303189/how-to-move-mocks-folder-in-jest-to-test) that could help work around this, but haven't looked into it yet.

Overall, it seems pretty straight-forward. We'll see how well it works once we actually get started.
