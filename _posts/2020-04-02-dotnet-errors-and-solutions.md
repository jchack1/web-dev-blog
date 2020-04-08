---
layout: post
title:  "A few .NET errors and solutions"
date:   2020-04-02 1:11
tags: .NET debugging csharp SQL
navigation: true
class: post-template
current: post
---

Here are a few errors I received while working on a .NET application, and how I solved them.

### "Unable to determine composite primary key ordering for type... Use the ColumnAttribute or the HasKey method..." 
<br>
This occured because I tried to assign more than one [Key] in a C# class file, as per below:

``` csharp
[Key]
public int whatever { get; set; }

[Key]
public int anotherOne { get; set; }
```

The microsoft docs actually helped me here: [Microsoft docs on data annotation](https://docs.microsoft.com/en-ca/ef/ef6/modeling/code-first/data-annotations?redirectedfrom=MSDN#composite-keys)

Basically, if you have more than one primary key, you need to give them an order using the column attribute, like below:

``` csharp
[Key]
[Column(Order=1)]
public int whatever { get; set; }

[Key]
[Column(Order=2)]
public int anotherOne { get; set; }
```

Note that the order doesn't have to start at zero like an index, the order is just relative.


### "This operation requires a connection to master database"
<br>

There was an issue connecting to my database, and this was fixed by adding "Integrated Security=True;" to the connection string in my Web.config file.  This is what stack overflow suggested you do when you're connecting to the database with Windows Authentication, rather than a separate username and password. 


### "Cannot insert null value into column id"
<br>

The application uses a database of students. If you create a new student in the application it should add to the students table in the database. The problem was the student id number wasn't being assigned.

Back to Stack Overflow. Assuming you want the id to automatically increment, this is what you do.

In SQL Server Management Studio:
- open table in Design
- select your column, go to Column Properties
- check Identity Specification
- set "IsIdentity" to "yes"
- set "IdentityIncrement" to "1", so it increments by 1 each time you add a new student

