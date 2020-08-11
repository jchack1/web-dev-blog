---
layout: post
title:  "Dealing with memory leaks"
date:   2020-08-10 18:30
tags: javascript debugging memory
navigation: true
class: post-template
current: post
---

While working on a particularly difficult task at work I ended up learning a lot about memory leaks.

I am actually still working on this task.  To summarize, I needed to create a button that copies items in a table. We are using the javascript library Knockout.js.  

Knockout.js came out in 2010 before other similar libraries like React had come onto the scene. It was very promising at the time and gained a lot of interest from developers because it was able to observe the UI for changes from the user. 

I am still getting to know Knockout and React, but so far I have found my experience with React to be a more pleasant one. React just makes more sense to me in how the library is used and structured, and it is more optimized for performance.  Knockout, on the other hand, is not fully optimized in this regard and there are very specific, advanced fixes required to solve the problems you may face.  

I am not meaning to bash Knockout, and I am sure there is more for me to learn to improve my experience with it, but I do find it unnecessarily frustrating at times.

So, back to my specific issue.

I figured out the original logic for my button early on when I was assigned this task. To be honest even this took me a while; deciphering other peoples' (uncommented!) code is what proved to be the most challenging aspect at that point in time. It was basically two nested loops - the outer loop determines where in the table the <em>set</em> of copies will go, and the inner loop inserts the <em>individual</em> copies into the correct place in the table.  This worked great on small tables, let's say smaller than 20 items. But once I started using it on tables that contained 100s of items, the browser crashed. It ran out of memory.

My classmates from my web dev program suggested a while back, for a different task, that I get to know the chrome developer tools. I decided to go more in depth and start learning how to use the performance profiler and the memory tab. I also started doing more research into memory leaks in general and this is what I learned.

### Javascript memory management

Javascript automatically manages memory, allocating slots of memory when you declare variables. 

<strong>Garbage collection(GC)</strong> is automatic memory management, which monitors how memory is being allocated, and reclaims memory that is no longer being used. Javascript uses GC, where other languages like C and C++ do not have this built in. You have to allocate memory manually when using these languages.

But just because javascript has garbage collection doesn't mean you will never encounter issues with memory.

Javascript and GC algorithms keep track of references.  
  -  Reference counting: if an object has no other objects referencing it, it is no longer needed. It is sent to the garbage.
  -  Circular references: if you have references referring to each other. They can never be garbage collected. Even if the values are deleted, the references still exist in memory.
  -  Mark and sweep algorithm: instead of looking at which objects are no longer needed, objects that are "no longer reachable" are garbage collected. It starts from the javascript root and works its way outward, finding all the references from there. If something cannot be reached in this case, it is garbage collected.

If you look at your memory graph in dev tools, when analyzing performance, you may see a saw-tooth pattern. This can sometimes indicate a memory leak, because the javascript heap keeps increasing.

<strong>Heap:</strong> the pool of memory used to satisfy requests for memory

### How my issue was solved

I spent a lot of time just learning about memory and was pretty stuck.  Of course, the examples from the dev tools instructions are pretty simple, but the application I'm working on is complex. I sent some of my performance profiles and heap snapshots to one of the senior devs. He said that it looked like too many <em>subscriptions</em> are being made, and few are being <em>disposed</em> of.

In knockout, you can <strong>subscribe</strong> to elements in your UI so you will be notified of any changes. Subscriptions can also be <strong>disposed</strong> of when they are no longer needed.  It looked like I had to manually make these fixes, while they are most often taken care of automatically in the background. 

What was happening was, every time we were making a new copy in the table, a new subscription was being made, and this was taking up space in memory. This is because we were adding new copies to an observable array, not a plain javascript array.

To fix this, we:
-  copied the observable array with all the original data into a new regular javascript array
-  added the new copies to the regular array
-  replaced the content of the observable array with the contents of the regular array; this is the stage when the subscription occured

So, we had only one subscription event, rather than a potential 100 or 1000. The speed improved dramatically.

There are still a number of bugs to work out with this task, but I learned a lot about memory management just from working on these issues.


