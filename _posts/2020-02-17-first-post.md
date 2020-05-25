---
layout: post
title:  "Welcome to my new web dev blog!"
date:   2020-02-17 00:30
tags: jekyll 
navigation: true
class: post-template
current: post
---

Hello!  This is the first post in my new blog. My intention for this blog is to create a place for me to document everything that I am learning as a new web developer. 

I started my certificate program in September 2019, and I want to be able to look back on all I have learned, and have a centralized space for notes. Here I will make notes of the things I'm learning, the challenges I face, and how I solved any issues that arise. I came up with the idea when I created my first Jekyll blog for an assignment in my CMS class. I needed to create blog posts, so I decided to live-blog my experience using Jekyll. I ended up having a lot of fun with this, and I had notes for myself for the future.

Today's blog will talk about the steps I took to do the initial set up for this blog.

To get started, I:
- navigated into old jekyll directory on my machine
- executed command jekyll new julia-blog
- cd julia-blog
- bundle exec jekyll serve 
- navigated in browser to localhost:4000

This is pretty straight forward and you can find these instructions in the Jekyll docs.

Next I wanted to install I theme. In my blog from school, I wrote that I did this by:

- finding a theme and downloading the files from github
- copying over the files that I didn't already have in my directory from creating the new blog
- bundle install
- bundle exec jekyll serve

(At this point I'd like to leave a note to self: ensure I categorize these posts into topics for easier navigation later)

When I did this, I got a page with the theme working. But I got the 404 page. Now I am looking in the config file to see if I can find out why. This config file came from my theme, which is Jasper 2.  The baseurl was set to "/jasper2", which doesn't exist in my root folder, so I changed it to "/", which the config file instructed you to do. 

- jekyll serve

This gave me an error. The error message suggested the following (something to do with the gem file, I will look into this later), which I executed:

-bundle exec jekyll serve

This worked, but it took me to my boring index page with no styling. I noticed that the 404.html page that came with my theme had layout: default in the front matter, so I added this to my index. The site regenerated, and now I have styling.