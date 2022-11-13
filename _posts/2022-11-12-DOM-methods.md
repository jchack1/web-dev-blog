---
layout: post
title: "The DOM"
date: 2022-11-12 18:30
tags: javascript dom
navigation: true
class: post-template
current: post
---

Some more notes about the DOM, from the course I'm working on "The Complete JavaScript Course 2022: From Zero to Expert" on Udemy. This post is more about DOM methods and strategies for common use cases.

Once again this is a bit messy, since it's just a quick reference for myself on all the things we can do with the DOM.

### Selecting

- selecting the whole HTML document: `document.documentElement`
- head: `document.head`, body: `document.body`
- select all, returns a node list: `document.querySelectorAll()`

More selectors:

- getElementById()
- getElementsByTagName()
- querySelector()
- getElementsByClassName()

HTML Collection: a list of elements from the <em>live</em> webpage, will update if elements are added or deleted

- not just any type of DOM node - collections are made up of HTML elements

Node list vs HTML collection: a node list stays constant (usually) and won't update itself, but an HTML collection is live and can update

- node lists can contain any type of DOM node, but HTML collections can only contain HTML elements
- node lists can use array methods, HTML collections can't

### Creating/inserting elements

- insertAdjacentHTML()
- document.createElement() - returns DOM element, but you need to put it into the page still
- textContent and innerHTML can add content to an element
- prepend() - adds element as the first child of an element
- append() - adds element as the last child of an element
  - prepend and append can be used to move elements, since the DOM element is assigned a unique id
- cloneNode() - copies an element, giving it a unique id

### Deleting elements

- remove() - removes element from DOM

### Styling

- getComputedStyle() - pass element into this function to get all its CSS styling
- can get individual properties from this as well

```js
const thing = document.querySelector(".thing");

const color = getComputedStyle(thing).color;
```

- setProperty() - pass property and value into this to set a CSS property on an element

```js
const thing = document.querySelector(".thing");

thing.style.setProperty("backgroundColor", "red");

//but this is not always necessary
//can do this instead

thing.style.backgroundColor = "red";
```

### Attributes

- access standard HTML attributes like how we access style above
- for anything that is not standard, use getAttribute()
- also a setAttribute() method
- can get data attributes as well, accessed with "dataset" property

### Classes

- get classes with "classList"
- classList.add()
- classList.remove()
- classList.toggle()
- classList.contains()

DON'T USE: `thing.className = "newClass"` to set a class, because it overrides everything. Just use the methods above.

### Coordinates of DOM elements

- getBoundingClientRect(): gives you lots of properties of an element, including how far away it is from the left (x), right, top (y), bottom of the viewport, as welel as the element's width and height
- window.pageXOffset or scrollX: how far down the page you have scrolled
- window.pageYOffset or scrollY: how far you've scrolled horizontally, if at all
- document.documentElement.clientHeight (or clientWidth): gives the height (or width) of the viewport
- window.scrollTo(): to scroll to coordinates on the page

```js
//NEWER WAY of scrolling to element
//no calculations needed
//just in new browsers though
element.scrollIntoView({behavior: "smooth"});

//OLD WAY
//combine methods together to get the abolute coordinates of an element
//not just the coordinates relative to the top of viewport, etc

window.scrollTo(
  element.left + window.pageXOffset,
  element.top + window.pageYOffset
);

//can also add an object, to specify behavior
window.scrollTo({
  left: element.left + window.pageXOffset,
  top: element.top + window.pageYOffset,
  behavior: "smooth",
});
```

### Events

- mouseEnter(): like hovering, when the mouse is hovering over an element
- mouseLeave(): mouse is off the element
- mouseover and mouseout are similar, but mouseenter triggers only when the mouse enters that specific element. mouseover is triggered when the mouse enters that element OR its childredn

How events work - using click events as example

- element is clicked, and an event is generated at the root of document (top of dom tree)
- capturing phase: event passes down to the target element, through every parent element on its way
- target phase begins once the event gets to the target element - then whatever is supposed to happen on click, happens
- bubbling phase - event travels all the way back up to the root of the document, again through every parent element
  - its as if the same event is happening on all the parent elements too
  - so if we also had that event listener on one of the parent elements, the thing would happen there too
- this is basically events "progagating"
- you can stop the propagation of an event before it bubbles back up to the parent elements, using e.stopPropagation() - but you don't want to rely on this unless you really need to

By default: event is handled by target element and in bubbling phase, BUT: you can set up some events to happen in capturing phase (add `true` to your event listener code)

Also, there are exceptions, where there are no capturing or bubbling phases

- mouseover vs mouseenter: note that mouseover events bubble, but mouseenter events DO NOT, since mouseover is triggered by hovering over itself and its children

### Smooth scroll into view

- on the a tag, put the id of the element you'd like to scroll to in the href
- in the javascript, add an event listener
- use e.preventDefault() so we aren't just using the HTML to jump down the page, want our javascript to make it smooth
- get the id of the element but using this.getAttribute("href")
- select that element using the id, then add scrollIntoView({behavior: smooth})

Example below of adding events to multiple nav links - but, forEach is not very efficient. You could instead add the function to a common parent element, only one time.

```js
document.querySelectorAll(".nav-link").forEach((x) => {
  x.addEventListener("click", (e) => {
    e.preventDefault();
    const id = this.getAttribute("href");
    document.querySelector(id).scrollIntoView({behavior: "smooth"});
  });
});
```

### Event delegation

- add event listener to common parent
- determine what element originated the event, using e.target
  - check if target contains the correct class of the element you're looking for
  - if so, using e.target, you can get the id like in the example above, basically do the same code

### DOM traversing

- "walking through" the DOM
- can select elements, and get child elements or use querySelector() on it

```js
const h2 = document.querySelector("h2");

//can select child elements with a particular class
//only elements with this class that are child elements of this parent will be selected
const elements = h2.querySelectorAll(".my-class");

//can also select parent elements

const parent = h2.parentNode;
const anotherWay = h2.parentElement;

const closest = h2.closest(".header"); //closest parent with a particular class

// can also select direct siblings

const previous = h2.previousSibling;
const next = h2.nextSibling;
```

### Passing arguments into handler functions

- event handlers can only accept 1 argument, but there are times when you may need to pass in others
- you can use the `bind` method, as well as the `this` keyword

```js

//opacity example, similar to the one in the course
//let say you want to change the opacity of something based on a hover event

const handleHover = (e) => {
  //only need to pass in the event, using bind in the event handle allows us to use "this" here
  if(e.target.classList.contains("nav-link")){
   const link = e.target
   const siblings = link.closest(".nav").querySelectorAll(".nav-link")

   siblings.forEach(x => {
    if x !== link x.style.opacity = this
   })
  }

}

//bind allows you to add extra arguments, and you can use "this" in your callback
element.addEventListener("mouseover", handleHover.bind(0.5))
element.addEventListener("mouseout", handleHover.bind(1))
```

### Intersection Observer API

- user observer to observe a specific target
- I've had some trouble getting this to work the way I want it to, but it's very powerful
- can use it to do something when an element comes into view on the screen

```js
//this is called whenever the target element intersects the root element we define in the options object
const obsCallback = (entries, observer) => {
  entries.forEach((entry) => {});
};

const obsOptions = {
  root: null, //set to null when you want your target to just come into the viewport
  threshold: 0.1, //how much of the section needs to be intersecting the viewport
};

const observer = new IntersectionObserver(obsCallback, obsOptions);
observer.observe(myElement);
```
