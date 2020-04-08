---
layout: post
title:  "New jekyll site part 2 - errors with Gulp"
date:   2020-04-04 17:31
tags: jekyll gulp debugging
navigation: true
class: post-template
current: post
---

Continuing from my last post, where I began to describe the process of making this blog.

I'm at the point of self-isolation where I'm starting to forget what day it is.  I believe I spent the past 3 days or so trying to figure out this problem, though at this point I've lost track.

I left off describing my issue of CSS not compiling the way I expected it to.  I admit I should have checked the instructions on the Jasper2 theme's github in the very beginning so I would be aware that they use gulp. I followed the instructions to install gulp, but it still didn't work. I spent many more hours fiddling around with it, then I decided to look up a tutorial on gulp in case my issue was simply not knowing enough about gulp.

I saw that gulp utilizes command line.  You can create functions in your gulpfile.js that can be executed in the command line; in my case, the gulpfile was already created. Below is an example function that was included with my theme:

``` js
gulp.task('build', ['css'], function (/* cb */) {
    return nodemonServerInit();
});
```

Above is a gulp task, named build.  To run this code I would type "gulp build" in the command line. If you name a task "default", all you need to type in command line is "gulp", and your default task is run. 

Once I learned this I was getting much closer to figuring out my problem.  However when I ran "gulp" or "gulp build", I got an error: 

```
"fs.js:27
const { Math, Object } = primordials;
                         ^

ReferenceError: primordials is not defined"
```

I searched online for a solution to this error as kind of a last ditch effort. Up until this point, I could not find any solutions at all, and there were no issues of this kind brought up by users on Jasper2's github page.  Of course, stack overflow gave me a solution, which I would never have thought of myself unless I looked it up. I believe it was the second answer on this page that helped me: https://stackoverflow.com/questions/55921442/how-to-fix-referenceerror-primordials-is-not-defined-in-node

The issue is that the node.js and gulp versions were incompatible. Remember my last post, where I wondered if I had an issue with my package versions? In a way I was right, just not in the way I expected. Node version 11.5 and higher, and gulp 3.9.1 are not compatible. This version of gulp depends on graceful-fs version 3.0.0, which is something I'm still unfamiliar with. 

To fix this I had to:
- add the below code to my package.json, and run "npm install" in command line

``` json
"scripts": {
    "preinstall": "npx npm-force-resolutions"
  },
  "resolutions": {
    "graceful-fs": "4.2.3"
  }
```


All this just to be able to tweak the CSS in my blog. 

Now, whenever I make a change to CSS, I execute the following commands:
- gulp
- bundle exec jekyll serve

(If I really want to be thorough, I will add in "bundle exec jekyll build" before I serve the site)

I believe there is a way to make gulp watch for changes, but I haven't figured out how to make that work with jekyll. For now though this works just fine, despite the extra typing.

Okay. Now I think it's time to finally make my changes to CSS, as well as research markdown in order to clean up my blog posts.  See you in the next one.

