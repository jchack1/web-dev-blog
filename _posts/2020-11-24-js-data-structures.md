---
layout: post
title: "When to use the different JavaScript data structures"
date: 2020-11-23 18:30
tags: javascript
navigation: true
class: post-template
current: post
---

A few quick notes from my Udemy Javascript course on when you should use different types of JavaScript data structures. The course is "The Complete JavaScript Course 2020: From Zero to Expert!", which I started last year but never finished.

I recently learned of two new data structures that I have never used before. In JavaScript I was never really aware of anything past arrays and objects. The new ones I learned about were sets and maps.

A <strong>set</strong> is a collection of values you can iterate through, which contains unique values. You cannot have any duplicate values in a set, unlike arrays.

A <strong>map</strong>, not to be confused with the .map() method, is like an object in that it has key-value pairs, but keys can be any data type. A map basically returns an array of arrays, with each array containing the key and value.

### Arrays are similar to sets

Arrays and sets are both collections of values, whose values do not need to be described.

Arrays are good for when you need to allow for duplicate values, and when you need to manipulate data, since there are lots of good built-in array methods you can use.

Sets are a good way to remove duplicates from an array, and when you must have unique values. So sets compliment arrays nicely.

### Objects are similar to maps

Objects and maps are used in instances where you need to describe your values with keys.

If you need to include methods, use objects. Maps are better for simple collections of key-value pairs.

If you need your keys to be any data type other than a string, use a map.

### Screenshot from course

Below you can see a screenshot from the course, which is a nice comparison of all the options.

<img src="../assets/images/udemy-js-screenshot.jpg" style="max-width: 700px;" alt="screenshot udemy data structure comparison"/>
