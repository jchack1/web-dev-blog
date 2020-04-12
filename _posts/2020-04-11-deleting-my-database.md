---
layout: post
title:  "Last ditch effort – I deleted my database"
date:   2020-04-11 18:30
tags: dotnet database migrations
navigation: true
class: post-template
current: post
---


I have been working on my Udemy courses again, currently hoping to finish my MVC course.
Sometimes I really don’t like .NET.  I’m not sure if it’s needlessly complicated or if I just need to learn more to help myself understand. I didn’t want to give up on my course though, because I’m sure it will be useful at my next job, or somewhere down the line.

Since I was away from this course for a while I forgot where I was and some of what I had learned. I started a new section, followed all the instructions exactly, attempting to create a dropdown menu. Of course, none of the options showed up in the menu.

The first thing I did was check the code for typos. There were none. I thought this was a mistake so I checked again, and compared it to the instructor’s code on github. Nope, still correct.

Perhaps I made an error with the earlier exercises in the course. There were no review videos, and I had to use some of the code from github. This was necessary in order to have the same code as the instructor throughout the course.  If it was an error with my migrations, a topic I still don’t quite get, maybe the dropdown menu items just didn’t make it into my database.

I’m also not yet an expert in SQL, so I used the below query to check the table for my data:

```sql
SELECT TOP (1000) [Name]
FROM [MembershipTypes]
```

This showed me the rows that were in my table, and the menu items were there.

At this point I spent about 2 days trying various things to fix the problem. I tried to simply update the database, using the update-database command in package manager console. Didn’t work. I realized some of my files may have been missing from the earlier exercises, so I added the code to my solution. Also didn’t help. I updated the DbContext to what I saw on the github code. Didn’t help, at least not immediately. After lots of experimenting and searching, I was ready to start a new solution and rebuild it from scratch, which meant having to watch a few hours of video again. It would maybe help solidify what I was learning but that was not ideal.

[Somewhere on Stack Overflow](https://stackoverflow.com/questions/16035333/how-to-delete-and-recreate-from-scratch-an-existing-ef-code-first-database) someone suggested to manually delete the database to start fresh. I decided to try this, but I did not follow their instructions exactly.

First I deleted the database connections in Server Explorer. I then deleted the database files under the App_Data folder in Solution Explorer. I added a new migration using “add-migration” in package manager console, then “update-database”.
I cleaned and built my solution again, then opened the page I had originally been working on.  The dropdown was now working properly and all menu items showed up!

I can’t help but feel there is still going to be something wrong with my database, migrations, or project in general and it will come back to bite me later on. But for now I will continue on with the course and take this as a learning opportunity.
