---
layout: post
title: "Problem solving strategy for code problems"
date: 2025-01-29 16:30
tags: computer-science problem-solving coding-problems
navigation: true
class: post-template
current: post
---

I'm continuing to work on the JavaScript Algorithms and Data Structures course on Udemy. Here are some notes outlining the recommended problem solving strategy.

### What is an algorithm?

Essentially, a set of steps to accomplish a task.

### Step 1: Understanding the problem

Ask clarifying questions

- can I restate in my own words?
- what are the inputs?
- what are the outputs?
- can the outputs be determined from the inputs? do I have enough info already?
- how should I label the important data?

### Step 2: Look at concrete examples

Can help you understand the problem, and whether your solution will work as you expect

Some real life examples:

- user stories
- unit tests

How to do this:

- write down simple examples with the inputs and outputs
- then some more complex examples
- now you have it and can look back on it as you work
- think about edge cases, examples with empty inputs or invalid inputs

For example: write a function which takes in a string and returns counts of each character in the string

A couple simple examples: `charCount("aaaa") // {a: 4}`
`charCount("hello") // {h:1 e:1, l:2, o:1}`

A question that comes to mind: do we want to have all the letters in the alphabet, and just increment the ones used? Like: {a: 0, b: 0, c: 0, ...}. Or, only include the letters in the string. Would need to clarify this.

More complex: `charCount("my phone number is 1234567890)` - now we think, what do we do about spaces? What about numbers? Would we want this to return an error?

Or `charCount("Hello")` - are we storing uppercase and lowercase? Or converting uppercase to lowercase? Again, would need to clarify.

What about empty strings and null values, like `charCount("")` or `chartCount(null)`- how do we handle it?

This shows us how looking at different inputs can help understand how we need to solve the problem. Start with what seems simple, then the edge cases.

### Step 3: Break it down

Write down the steps of the problem. Can use comments as your guide. Also, if you were in a code interview, you can ask them if you're on the right track and get a hint.

Helps you think about the code before you write it, and catch any issues before you start.

### Step 4: Solve or simplify

Solve the problem, and if you can't, solve a simpler problem. Ignore what is giving you a hard time for a while, and focus on what you understand, then come back to it later. Often you'll begin to understand the more difficult part as you go and you can come back to it.

In addition, if it were during an interview, you're not stuck at the beginning and running out of time with nothing to show for it. At least you solved the rest of the problem.

### Step 5: Look back and refactor

Go line by line, talk about what you don't like, how easy it is to understand.

Ask these questions. If you were in an interview, ask them out loud

- can you check the result?
- can you derive the result differently?
- google if there is a faster way of doing it
- can you understand it at a glace? how intuitive and readable is it?
- can you use this solution for another problem? reusability in your code is a good thing
- can you improve the performance?
- any other ways to refactor? does it follow the style guides of the company? is it formatted well?
- how do others solve this problem? Even ask an interviewer this
