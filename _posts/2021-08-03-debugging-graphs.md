---
layout: post
title: "Debugging front-end graph libraries - recharts"
date: 2021-08-06 14:30
tags: debugging react
navigation: true
class: post-template
current: post
---

My company does a lot of data analytics, which means on the front end we work with a lot of graphs. Today I'll talk about some difficulties I had debugging a new dashboard on our platform, which was built using recharts graphs.

### The task

We needed a set of graphs to represent some new metrics. Without giving away what the graph is actually communicating, I'll say the x-axis went from "good" on the left side, to "bad" on the far right. We wanted a gradient going from left to right, changing from a "good" colour to a "bad" colour, to represent this concept and help users visually see the problems they are facing.

### The problem

I experimented with an Area chart in the recharts sandbox. The gradient was quick to figure out and I ended up with a nice blue-purple-orange gradient.

To create a gradient with recharts, you need to put the code under "defs":

```html
<defs>
  <linearGradient id="uv" x1="0" y1="0" x2="1" y2="0">
    <stop offset="5%" stopColor="#008eff" stopOpacity="{0.9}" />
    <stop offset="40%" stopColor="#833f97" stopOpacity="{0.9}" />
    <stop offset="95%" stopColor="#ff4800" stopOpacity="{0.9}" />
  </linearGradient>
</defs>
```

You use the "id" field inside the linearGradient tag inside the Area tag property "fill":

```js
<Area type="monotone" dataKey="percent" stroke="#8884d8" fill="url(#uv)" />
```

This looked great, so I copied it into my code and tried it out in the proper dashboard in our app.

<strong>The problem was, in the app, there was no gradient - the area fill was just gray.</strong>

### Things I tried

First I tried inputing other values into the "fill" property, including colour names like "red" or "blue". These colours worked.

I wasn't sure if using gradients was a recently added feature in recharts, so I checked what version of recharts we had. Our package.json had an old beta version of recharts, while the sandbox had the newest version. I updated our version and re-installed. Upon starting the app I saw the same problem - no gradient.

Perhaps it wasn't just the recharts version - we had an old version of react, react-scripts, and react-dom. I went through the process of trying to update these to a new version, but did not complete it, as I had a lot of conflicting versions of packages. I just re-installed the original versions, with the exception of the updated recharts, as the new version didn't affect our app negatively.

These graphs were nested inside many components, so I tried taking the AreaChart code and pasting it at the top of the code for our main overview page. Unexpectedly, the graph had a gradient! This told me there wasn't anything wrong with the versions of my packages at this point, and the problem was within my app itself.

Back on the actual dashboard page, I tried putting the graph in different places. Strangely, the gradient showed up when the graph was placed higher up on the page, but not when it was further down the page. I placed my graph at the very top of the dashboard, and I noticed that while this gradient worked, one of the old graphs below was losing its gradient.

### Solution

I was trying to put two graphs with gradients on the same page. This should work fine, except the linear gradient ids for both graphs were the same - neither I nor the previous developer had changed the id names when we copied the code from recharts.

```html
//below, id="uv". both gradients had this same id

<linearGradient id="uv" x1="0" y1="0" x2="1" y2="0">
  <stop offset="5%" stopColor="#008eff" stopOpacity="{0.9}" />
  <stop offset="40%" stopColor="#833f97" stopOpacity="{0.9}" />
  <stop offset="95%" stopColor="#ff4800" stopOpacity="{0.9}" />
</linearGradient>
```

Both graphs were trying to use a linear gradient with id="uv". This caused the bug, since ids are supposed to be unique, and the graph lower on the page didn't know which gradient it needed.

This was easily fixed by updating the ids to unique values.

I kind of wish I didn't have to go through all that work before figuring out this simple issue, but at least in the future when I see a similar problem I'll have a better idea of where to look first.
