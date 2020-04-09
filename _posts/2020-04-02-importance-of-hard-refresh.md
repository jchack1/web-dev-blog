---
layout: post
title:  "The importance of a hard refresh"
date:   2020-04-02 12:12
tags: dotnet javascript debugging
navigation: true
class: post-template
current: post
---

Recently I was working on a task for my practicum.  The task was to add a delete button to two of the pages and ensure it redirected to the correct page once an item was deleted.

This was fairly straight-forward, though it did take some time as I was still in the process of learning the set-up of their project. Since the delete button existed elsewhere in the application, I could copy and paste the code into my cshtml file and make a few minor adjustments. Redirecting the page was handled by a javascript file. I made a new function, which was basically a copy of an already existing function, but tweaked the code so it would redirect to a particular overview page.

I opened the application in debug mode (in Visual Studio, the F5 key). Everything worked. I was excited that I may be nearly done my task.

Then I ran the code normally (Ctrl + F5), not in debug mode.  And it didn't work - the page contents would not appear, and none of the buttons worked. 

I couldn't understand why this would happen.  I searched for answers online. Stack overflow suggested that it in normal mode, certain variables aren't being initialized, but in debug mode variables default to zero. I scanned my code to see where there could be uninitialized variables but couldn't find anything helpful.  I got onto discord and asked my web developer friends, who suggested the delete button was not properly connected to the necessary function in the javascript file. Once again I scanned my code, but there were no typos, everything was connected properly.

At this point I was becoming very frustrated, feeling like I was missing something important. That I just didn't understand some important feature of .NET, and that this was going to take ages to dig into and fix.

I tried testing it again. In Visual Studio I did a clean and build. In the browser I did a hard refresh (Ctrl + Shift + R).

...and it worked.

I was surprised and a little suspicious. I didn't think it would be that easy, but it was.  Previously, the old javascript file was saved in the cache, not my new updated one. Once it had the new file it worked like a charm.

So, all of that to say, I learned the importance of trying the simple solutions first. Restart Visual Studio. Restart your computer.  And if working with javascript, just try a hard refresh.
