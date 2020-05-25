---
layout: post
title:  "Notes on jQuery"
date:   2020-04-20 21:30
tags: javascript jquery dom
navigation: true
class: post-template
current: post
---

I have seen jQuery all over the place, including at work, but I have never taken the time to learn it in depth.  I figured I needed to do this soon, and there happens to be a section in my current Web Developer Bootcamp Udemy course. Here are some of my notes.

jQuery is a javascript library that helps us manipulate the DOM.

Pros:
- shorter code, easier to access dom
- cross-browser support
- ajax
- strong community

Cons:
- everything you can do with jQuery you can use without it
- including lots of extra methods even if you are only using a couple, could have performance issues

Two ways to include jQuery in your app:
- download code for jQuery, link to it in html file
    - there's a full 9000 line file, and also a minified file
- include CDN in html file
    - linking to jQuery file online rather than downloading it to your machine
    - slightly slower performance than having it locally

Make sure your jQuery libraries are included <strong>before</strong> your own script files in your HTML. If you are depending on them in your own javascript file, they must be loaded first, otherwise your file can't find the jQuery methods it is supposed to use.

### Selecting elements

Use the $ function to select things:  $()

This is like using document.querySelectorAll() in vanilla JS. Inside the function use a CSS selector. This function returns all the elements that match the selector. e.g. $(".someClass");

### Important methods

For styling: $(selector).css(property, value)

val(): to get value from an input, can also set the value
- e.g. set the input's value to an empty string to clear the input field

text(): gets text content in set of matched elements, and can set text content of an element
- like textContent in vanilla javascript

attr(): get the value of an attribute for first matched element, set the value for all matched elements
- e.g. to set,  $(#myPhoto).attr("alt", "Photo of my dog");
- can also set several attributes at once using a javascript object

html(): is like innerHTML in vanilla javascript, gets HTML content of first matched element or sets HTML content of all matched elements

Below are like the classList properties in vanilla javascript:

addClass(): add a class or classes to matched elements

removeClass(): remove a class or classes to matched elements

toggleClass(): add a class if the matched element doesn't have it, removes a class if the element has it

remove(): removes element from the DOM

### Important jQuery events

click(): add a click listener to an element or colection of elements
- callback function goes in the parentheses

keypress(): fires in between key being pressed down and coming back up
- every key has its own code, access it with keyword "which"

```js
$("input").keypress(function(event){
    if(event.which === 13){
        console.log("you hit enter");
    }
});
```


keydown(): fired when key is pressed down

keyup(): fired when a pressed key is released

on(): most used jQuery event method
- similar to vanilla js addEventListener, 
- used most of the time
- include the type of event, and a callback function
- adds listeners for all potential items that aren't there when the page loads
    - "click", on the other hand, only creates listeners for things that are on the page when it loads

```js
$("button").on("mouseenter", function(){
    $(this).css("font-weight", "bold" );
});
```

### "this" in jQuery

In jQuery you need to wrap "this" in a jQuery selector, so it knows we're using a jQuery object, like so:  $(this)

If you use just plain this, like you would in vanilla javascript, it will not work.


### Common jQuery effects

fadeOut(): current opacity to transparent, can specify the speed
- elements are hidden, not deleted
- can include a callback function, because we often want something to happen <em>after</em> the the fade has completed. If you include this in a separate line of code after fadeOut, it will execute right away, possibly before the fade is even complete

fadeIn(): similar to fadeOut
- element should start out as display none; it gets changed to display block when you use the function
- also include callback functions inside, for same reason as above fadeOut()

fadeToggle(): will know if it needs to fade in or out depending if element is currently displayed

slideDown(), slideUp(), slideToggle(): height of an element is animated as up or down
- also have optional callback and ability to specify timing

### Parent elements

#### Event bubbling

If you have a click event on a particular element, it will bubble up into parent elements, triggering any click events on the parents.

You can tell it not to bubble by adding the following to your click event:
- add "event" (or whatever keyword you choose) as an argument to your callback function
- add a line inside your callback function: event.stopPropagation();

```js
$("span").click(function(event){
    alert("clicked on a span");
    event.stopPropagation();
});
```

#### Removing parent elements

Quite simple, as per below:

```js
$("span").click(function(event){
    $(this).parent().remove();
    event.stopPropagation();
});
```



