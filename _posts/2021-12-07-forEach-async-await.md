---
layout: post
title: "Debugging - forEach with async/await"
date: 2021-12-07 18:30
tags: javascript debugging
navigation: true
class: post-template
current: post
---

This week I learned that you do not use forEach loops with async/await.

I was working on a CRUD microservice at work. It uses AWS Lambda functions with Node.js. I was having trouble with the update and delete functions in the app.

We use async and await to deal with promises when interacting with the database. While investigating the updating issue, I noticed that most of the time, my "put" operations were not going through. No error was being returned - the puts were just not happening. Not a lot to go on when debugging. The code also seemed to stop running when it hit the function that was supposed to return the promise.

What's interesting, though, is that <em>sometimes</em> the operations would actually go through. I'd notice that my item had updated and the rest of the code had run. But I hadn't made any changes to the code. This suggested to me that it may be an issue with the way I implemented async and await.

### What the code originally looked like

I had no reason before to think that a forEach loop couldn't be used with async/await. I figured that if you awaited something in the body of the forEach callback, it would work as long as you placed an async in front of your callback, kind of like this:

```js
const newTeamName = "Team B";
const employees = getEmployeesByTeamName({teamName: "Team A"});

employees.forEach(async (employee) => {
  await updateEmployeeTeam({teamName: newTeamName, employeeId: employee.id});
});
```

### What the problem is

Here are a couple articles that helped me understand, as well as the MDN doc for forEach:

- [https://zellwk.com/blog/async-await-in-loops/](https://zellwk.com/blog/async-await-in-loops/)
- [https://www.hacksparrow.com/javascript/foreach-in-promise-async-function.html](https://www.hacksparrow.com/javascript/foreach-in-promise-async-function.html)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

Essentially, forEach came before async/await and is not built for it. Async/await doesn't work with callback based loops. You aren't actually awaiting the callback. The code just keeps running and doesn't wait for promises to complete.

For my work scenario, most of the time I could not get the data back from the database before the forEach loop moved on the to next line of code. I assume that for the times that my code <em>did</em> work, it was just by chance that I got the data quickly enough that execution had not moved onto the next line yet. Of course, this was a very rare event.

### What to do instead

Just use a for loop. The code will pause and wait for your promise synchronously, as you'd expect. In my case I used a for...of loop, like below:

```js
const newTeamName = "Team B";
const employees = getEmployeesByTeamName({teamName: "Team A"});

for (const employee of employees) {
  await updateEmployeeTeam({teamName: newTeamName, employeeId: employee.id});
}
```
