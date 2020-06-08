---
layout: post
title:  "'There is already an object named ___ in the database' and 'Column names in each table must be unique'"
date:   2020-06-08 12:30
tags: database sql dotnet migrations
navigation: true
class: post-template
current: post
---

There were migrations performed on our application at work this week, which meant my database was out of date and the application would not load on my local machine. 

I'll do my best to explain without using any data/table names/etc from my work application.

In package-manager console in Visual Studio I ran "update-database", and received the following error when it tried to run the migrations on my database: 

#### "There is already an object named ___ in the database"

Right away this time I asked a coworker if she also had that error. She told me I should drop the above-mentioned table, which I will refer to as Table1, and gave me more instructions. I will outline the process below.

- opened SQL Server Management Studio 
- Databases 
- *mydatabasename 
- Tables

There was another important related table, which I'll call Table2. In here I had to go to the Keys section and delete all the keys related to Table1.

Then,
- go back to Table1 
- right click 
- Script table as 
- DELETE to 
- New query window

In the new query window I deleted the line that said,
```
WHERE <Search Conditions>
```
then executed the query. This deletes the contents. You need to do this BEFORE you drop the table.

Dropping the table:
- right click Table1 again
- Script table as 
- DROP to 
- New query window
- execute this query, which drops Table1

I went to update database again, but I got a new error:

#### "Column names in each table must be unique. Column name ___ in Table2 is specified more than once"

These were <em>column names</em> that were related to Table1. I already got rid of the <em>keys</em> that were related to Table1, but not columns. So I deleted those as well, went back to Visual Studio and tried update-database again.

This time it worked with no error and I was able to open my application.


 




