---
layout: post
title:  "Making a new jekyll site - this one, actually"
date:   2020-04-02 16:50
tags: jekyll
navigation: true
class: post-template
current: post
---

Well, I'm going to do that thing again where I blog my experience of making a blog. At least I can feel like I am writing something for a reader, a pretend social interaction during week 3 of social distancing, though I suspect no one will read this except myself.

I'm already part way through making this blog, as you may have gathered from my first post on this site.  My first blog was pretty straight-forward, despite jekyll being new to me. I assumed this time around would be simpler, but now that I am really diving into the functionality of my chosen theme Jasper2 from Ghost, I'm realizing there is actually a lot I can do with this platform. Which means there is a lot to learn.  But at this stage in my web development journey, I realize there is always more to learn. 

As an aside, after building several websites from scratch with HTML, CSS, and vanilla JavaScript, using a theme like this feels a bit like cheating. I don't have to fully understand what the code does for it to work and give a beautiful result. But I guess I'm not doing school projects anymore; there are no requirements other than those I place on myself.

I already screwed up my home page by playing around with the index.html code, so I'd like to make sure I know what I'm doing when I start making new changes.

Next is adding my own logo. I remember from last time I need to stay well away from the _site folder. You can check out my old blog to learn more about that.  This time I will go straight to assets, plus I'll have a look at the index page and the layouts to see where the reference the logo. I already know that it is not mentioned in the navigation include.

It looks like the original logo is in assets -> images -> blog-icon.png.  I replaced the logo in this folder with my own and now it shows up on the site. However, it is much smaller than I'd like. I adjusted the max-height property for the site-logo class in screen.css, but this did nothing to change the size of the logo. The browser still displays the CSS with the original size.

I then found out that jasper2 requires the use of gulp to be able to change the CSS. I installed gulp globally and tried again. The CSS changes still did not show up. At this point I'm wondering if the version of gulp that I have is not compatible with my gemfile or gulpfile. 

In my theme you should be able to edit assets/css, and then the gulp.js file automatically updates the css file in assets/built. 

It took my a while to figure out what was going on, so I'll continue in the next post.

