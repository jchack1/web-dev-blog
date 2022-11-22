---
layout: post
title: "Deploying full stack MERN apps with Render"
date: 2022-11-21 20:30
tags: deployment devops node.js cors
navigation: true
class: post-template
current: post
---

Due to the recent changes with Heroku removing its free tier, I decided to move a few of my portfolio apps over to Render.

While I understand that companies want to make money, I can't justify spending money, even a small amount, every month just to host demo portfolio apps that probably no one looks at except me.

This isn't a tutorial. I just wanted to mention a few things I ran into while deploying my apps that could help someone (or me) in the future.

I came across Render when looking for a platform to deploy Node.js/Express/MongoDB apps. This [freeCodeCamp article](https://www.freecodecamp.org/news/how-to-deploy-nodejs-application-with-render/) provided a great walkthrough of what to do to deploy the back end of the app. [This blog post](https://dev.to/gregpetropoulos/render-deployment-free-tier-of-mern-app-52mk) was also helpful, since it explained how to set up a full stack app.

### You need a front end and back end site

One thing that was nice about Heroku was I could upload the front and back end in one project. It was easy for the front end to make requests to the back end using simple routes.

For my first app in Render, I set up a "Web Service" and connected my GitHub repo. This worked very well and was up and running quickly. When I opened the link, however, I just got "not found". All I was doing by visiting the link was making a request to the root of the app "/". I added "/getArticles" to the url, which is a route in my app, and then received the data I was expecting. Great! Except all I was getting was raw JSON and there was no front end. This had to be done separately.

So, I set up a "Static Site", pointing to the front end code in my repo. I realized that I would need to make requests to the same url I had just used in the browser. This was relatively simple to do. I just added an env variable in Render, and referred to it in my front end code.

### CORS

Of course, because I now had different urls for the front end and back end, I was now running into CORS issues. This is because the default behavior of CORS is to not allow requests to go through if they are being sent from a different origin than the app receiving the request.

This was fixed by installing a CORS package in the back end, giving permission to the front end url.

```js
//inside the server.js file

const cors = require("cors");

//real link is an env variable in Render
app.use(
  cors({
    origin: process.env.ORIGIN,
  })
);
```

I spent much longer on this than necessary. I kept getting the same CORS error over and over.

This was because I added an extra "/" on the end of my url, like this: `https://juliachack-dev-blog.netlify.app/`, when the real origin had no "/" on the end `https://juliachack-dev-blog.netlify.app`. A silly mistake, but it was the reason my app and the origin "didn't match".

### Conclusion

It seems like a bit too much work to have to set up two different deployments for one full stack app, and it's possible I'm missing something. Maybe there is another way that I didn't see. Otherwise, maybe one day it will require only one deployment. Overall it was a good experience setting up my apps on Render.
