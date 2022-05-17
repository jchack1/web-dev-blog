---
layout: post
title: "Things I learned about the DOM and vanilla JavaScript from cloning the chrome dinosaur game"
date: 2022-05-16 18:30
tags: javascript css dom
navigation: true
class: post-template
current: post
---

I learned some new things about vanilla javascript while following a [tutorial for creating the chrome dino game](https://www.youtube.com/watch?v=47eXVRJKdkU).

### Data attributes

Data attributes are used to store extra information on an HTML element. They always start with "data-" and are added to an HTML element like any other attribute. They are meant to be used for data that will not be shown on the screen, or picked up by a screen reader, just for data associated with a particular element.

```html
<div id="score" data-score></div>
```

The data attributes for an element are collectively called a "dataset". You can access the dataset using javascript like this:

```js
const scoreDiv = document.querySelector("#score");

const scoreDataset = scoreDiv.dataset;
```

Data attributes are great for selecting DOM elements in our javascript code. The easiest way to access elements with data attributes is to use the query selector method, adding your data attribute inside square brackets:

```js
const scoreDiv = document.querySelector("[data-score]");
```

### CSS variables and calc() method

When I started learning CSS there weren't a lot of ways to use variables in your stylesheets - you had to use SASS, which would compile into CSS. There are now more options built into CSS to use variables and functions, including the calc() method.

This method is fairly straight-forward. You pass some values into calc(), and the value returned from it becomes the value of your CSS property.

```css
.my-class {
  --bottom: 0;
  position: absolute;
  bottom: calc(var(--bottom) * 1%);
}
```

In this example we are also passing a variable into calc.

Variables, or custom properties, are useful for values that repeat often in your stylesheet. We also used them in the dino game code when we needed to manipulate a value using javascript.

Variables are declared in your stylesheet with a double hyphen `"--"` preceding the variable name, and are accessed using the var() function.

In the dino game code we often selected our elements using data attributes and changed their custom CSS properties in our javascript code. This is how we were able to make the game interactive.

### CSS getPropertyValue() and setProperty()

These are built in functions that allow you to get the value of a CSS property, and set or change the value of a CSS property.

They are used on your elements once they are selected in your javascript code.

We also needed to use a window method called <strong>getComputedStyle()</strong>, which returns an object containing all the CSS properties for an element.

```js
const scoreDiv = document.querySelector("[data-score]");

const myValue = getComputedStyle(scoreDiv).getPropertyValue("margin");
```

The object returned from getComputedStyle() is read only, so when you want to update a value, you use setProperty() on the element's style property like this:

```js
//pass in the property and its new value
scoreDiv.style.setProperty("margin", "20px");
```

### getBoundingClientRect()

This one isn't new to me, but I had never used it in a game before. This function returns an object with information on the size of an element and its position within the viewport.

In the dino game we get these values for both the dino and the cacti. Whenever any of the position values are the same, that means a collision has occurred, and the game is over.

### Conclusion

I don't often work with vanilla javascript anymore since I spend most of my time using React. It's important to understand the basics and it's been really helpful to fill some gaps in my knowledge regarding working with the DOM, so I can make my code more interactive.
