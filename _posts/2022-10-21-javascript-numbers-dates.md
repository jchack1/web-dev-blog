---
layout: post
title: "Working with JavaScript numbers and dates"
date: 2022-10-30 18:30
tags: javascript
navigation: true
class: post-template
current: post
---

Doing math and working with dates can sometimes be challenging in programming.

I've been getting back into my JavaScript course on Udemy called "The Complete JavaScript Course 2022: From Zero to Expert", in order to level up my skills. Here are some notes about numbers, dates, and methods we can use in JavaScript to work with numbers and dates.

### Conversion, parsing

- `Number()`: converts to a number
- `+` : a kind of type coercion that you place in front of what you'd like to convert. Acts just like `Number()`
- `Number.parseInt()`: if a string begins with a number(specifically an integer), it will return the number to you. Make sure you include the radix (or base, eg base 10, or base 2 (binary)).
- `Number.parseFloat()`: if a string begins with a number(in this case, a floating point number, meaning it has a decimal), it will return the number to you, including what's after the decimal.
- `Number.isNaN()`: check if something is not a number. Remember that NaN is its own type of value, so you could even put a string into this method, and it will return false, because it is <em>not</em> NaN, it is a string.

Notes on NaN [from MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN): NaN is returned when a math operation results in something we can't represent or is undefined. It is also returned when we try to convert something into a number that can't be. - `Number("257abc")` returns NaN, since it can't be turned into a number

- `Number.isFinite()`: another, better way to check if something is a floating point number
- `Number.isInteger()`: to check if something is an integer, no decimal

### Remainders

- use `%` to get the remainder of a division operation
- good when you need to do something every nth time
- good to use when determining if a number is even or odd
  - if you divide a number by 2, and the remainder is 0, the number is even. If the remainder is 1, the number is odd

### Numeric separators

- use `_` to separate digits in a number, to make it easier to read
- for example you can use them to separate by the thousands place
  - 2,000,000 becomes 2_000_000
  - JavaScript ignores the underscores
- cannot be placed:
  - before or after a decimal point
  - at beginning or end of number
  - in a string that needs to be converted to a number - will result in NaN

### BigInt

- there is a limit to the biggest number JavaScript can represent - the max safe integer
  - 2 to the power of (53 -1)
  - anything larger is unsafe
- BigInt can store these larger numbers
- add `n` to the end of your number
- can also use BigInt function
- you can't do operations that mix regular numbers and BigInt - make sure the regular number is converted to a BigInt before doing a calculation

```js
//n
console.log(569875621352789786315346578967n);

//BigInt
console.log(BigInt(685789746533216578964321321234));
```

### Dates

- `new Date()` gives you the current date and time
- you can pass strings and numbers into the Date constructor and JavaScript will parse them
- unix timestamps (number of milliseconds since Jan 1 1970) can be passed into it as well

There are methods you can use on dates

- `getFullYear()` returns the year
- `getMonth()` - returns as a number, remember it is 0 based
- `getDate()` - returns the day of the month
- `getDay()` - returns day of the week as a number also 0 based, starting on Sunday
- `getHours()`
- `getMinutes()`
- `getSeconds()`
- `toISOString()` - converts to a string
- `getTime()` - returns unix timestamp in milliseconds
- `Date.now()` - gives us the timestamp for right now

There are also "set" methods for all of the above, so you can update a date

### Operations with dates

- you can convert a JavaScript date to a timestamp in milliseconds by converting it to a number
- if you wanted to subtract two dates, and return the number of days in between them, you'd need to divide them by a numerical value `1000 * 60 * 60 * 24` - converts milliseconds to seconds (1000), then to minutes (60), then hours (60), then days (24)
- do these kinds of conversions depending on whether you want values in days, minutes, hours, etc
- if you need to do more complicated operations, can get a library like Moment.js

### Internationalization API

#### Dates

Formatting with different languages:

- use `Intl.DateTimeFormat()` function to choose a language (locale), then `.format()` to format the date according to the chosen language
- [this link gives a list of codes](https://www.lingoes.net/en/translator/langcode.htm)
- you can [read more about this API on MDN here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat/DateTimeFormat)

```js
const today = new Date();
label.textContent - new Intl.DateTimeFormat("en-GB").format(today);
```

- you can add an "options" object into the formatter to get the date and time, and format how you choose
- numeric gives a number, long gives you a string - see above MDN link for more options

```js
const today = new Date();

const options = {
  hour: "numeric",
  minute: "numeric",
  day: "numeric",
  month: "long",
};

//pass options as an argument
label.textContent - new Intl.DateTimeFormat("en-GB", options).format(today);
```

- you can also get it from the user's browser instead of setting it manually: `const locale = navigator.language`
- this is ideal if you will have international users

#### Numbers

- use `Intl.NumberFormat().format()` to change number formatting based on location
- eg. using commas vs decimal points as separators
- can also pass in an option object, where you can format units (like mph, temperature, percent, currency)
- have to set the currency in 'options', which you can't get from locale alone

### setTimeout and setInterval

`setTimeout()`

- first argument is a callback function, and the second argument is the amount of time to pass before running the function
- pass additional arguments (that your callback function needs) after the time argument
- line of code is read, JavaScript makes note of it and counts down the time in the background, while the rest of your code runs. JavaScript does NOT wait for setTimeout to finish before executing the rest of your code
- can use `clearTimeout()` to delete the timer in certain situations. You would assign your setTimeout function to a variable, then pass that variable into `clearTimeout()`

`setInterval()`

- runs a callback function over and over, according to the timer interval you pass as an argument
- stop it using `clearInterval()`
