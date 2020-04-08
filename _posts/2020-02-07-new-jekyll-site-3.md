---
layout: post
title:  "New jekyll site part 3 - errors with deployment"
date:   2020-04-04 16:03
tags: jekyll debugging deployment
navigation: true
class: post-template
current: post
---

I made my changes to CSS without any more problems and I cleaned up my markdown files, as I mentioned last time. I figured now would be as good a time as any to deploy. I can keep adding posts and making small adjustments once deployed.

Of course there have been errors with this as well. As expected.

I created my github repo and pushed my code using command line. I used the following commands and this worked without errors:
- git init
- git add .
- git commit -m "first commit"
- git remote add origin https://github.com/jchack1/julia-blog2.git
- git push -u origin master

I decided to deploy with Netlify, which is my go to for any deployment. It's usually pretty straightforward, [and you can find the instructions for deploying Jekyll with Netlify here.](https://www.netlify.com/blog/2015/10/28/a-step-by-step-guide-jekyll-3.0-on-netlify/)  I selected my github repo and clicked deploy. After a few minutes I got an error, saying the build failed.

That is because Netlify was looking for the _site folder.  I checked my files and I saw that I did not have this.  By now I'm getting pretty frustrated with this theme because it feels like there was a lot missing and unexplained. Why isn't there a _site folder? 

I went to the [Jekyll docs](https://jekyllrb.com/docs/step-by-step/01-setup/) looking for any information about the _site folder.  I saw that when you use the command "jekyll build", Jekyll builds the site and outputs it to a directory called _site, which is viewable in the browser at localhost:4000. I have used the "jekyll build" command many times so far, so I was unsure why it's not in my files.  And why I was able to serve my site to localhost:4000.  I decided to see what would happen if I make a new Jekyll folder from scratch and run "jekyll build". In my previous sites I always had a _site folder and never gave any thought to how it was created.

This time I decided to just follow my old notes rather than just copy-paste from my theme. I went into the Jekyll folder I already have on my machine and opened command line. I used the following commands:
- jekyll new julia-blog3
- cd julia-blog3
- bundle exec jekyll build

I watched the files in the folders and there it was. Once I used the jekyll build command, my _site folder appeared. Perhaps this was my mistake from the beginning. I never intiallized my folder properly. 

I decided to check both folders now, to see if I could either update my blog scripts to create the _site file, or if I'd need to copy everything over into my new folder. Whatever I tried in my old folder did not work, so I ended up copying everything over into my new folder. I set up gulp, which was really quick this time since I knew what I was doing. When I executed "bundle exec jekyll build" I now ended up with a _site folder, and it was filled with the proper files.

Now I will upload this new folder to github and deploy. It should work now that I have a proper _site folder.

I just have to remember to always "bundle exec jekyll build" when I make changes. But only after I execute "gulp". Hopefull I will quickly get used to this workflow. 