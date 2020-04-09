---
layout: post
title:  "Entity Framework, workflows, databases, and migrations"
date:   2020-03-18 17:50
tags: dotnet MVC database migrations EntityFramework
navigation: true
class: post-template
current: post
---

When I began my Udemy course on .NET MVC, as well as my web development practicum, there were a lot of terms I was unfamiliar with - things that perhaps seem very simple to more advanced developers.  I was constantly having to google new terms I came across, and often the language used in the articles I read was not beginner enough for me.  In this post I will simply share some definitions and bits of information I found helpful when I was just starting to learn about .NET.

### Entity Framework: a tool we use to access a database
- it's an object relational mapper (ORM)
- it maps data from a relational database to objects in our app
- allows you to work at a higher level of abstraction compared to manual mapping
- involves:
    - opening a connection to the databae
    - executing a command
    - reading the data
    - closing the connection

#### Relational vs Non-relational databases

As an aside, it might be helpful to define these as well.

[This Medium article helped me quite a bit](https://medium.com/@zhenwu93/relational-vs-non-relational-databases-8336870da8bc)<br>
[As well as this PluralSite article](https://www.pluralsight.com/blog/software-development/relational-non-relational-databases)

Relational databases: data stored in tables of rows and columns, and relationships between tables are established using primary and foreign keys. Uses SQL. You have to spend time in the beginning intentionally designing the structure of your database.
- good for complex queries

Non-relational databases: (aka NoSQL) doesn't use tables, doesn't need a lot of structure. Data stored in collections of JSON documents.
- good for storing large amounts of data, and for flexibility

### DbContext: the connection to the database
- a class provided by Entity Framework
- a DbContext class file has one or more DbSet
- DbSet represent tables in the database
- can use LINQ to query DbSet, and at runtime Entity Framework translates LINQ queries to SQL queries

See the DbContext class example below. Each DbSet corresponds to a table in the database.

```csharp
public class D2LDbContext : DbContext
    {
        public DbSet<Student> Students { get; set; }
        //<Student> is a type parameter, Students is the property name in plural form
        //the whole line is a property of D2LDbContext
        
        public DbSet<Teacher> Teachers { get; set; } 
        
        public DbSet<Course> Courses { get; set; }

        public DbSet<StudentCourse> StudentCourse { get; set; }

        public DbSet<TeacherCourse> TeacherCourse { get; set; }
    }
```

You use a DbContext object to access tables, and you use DbSet to modify table data.

Important takeaway: DbContext is a database connection, and DbSet represents a table in the database

Entity Framework opens the connection to the database, reads the data, maps it to objects, and adds them to DbSet. Entity Framework also keeps track of changes, like when we add, delete, or modify.  When we ask to persist these changes, Entity Framework automatically generates SQL statements and executes them on the database.

At this point during my reading, I asked myself what "persistence" actually means. I had to read a few different sources to really get it.

#### Persistence
- (techopedia) in terms of data, persistence means an object should not be erased until it is ready to be deleted

Persistent data definitions:
- does not change and is not accessed frequently
- master data that's stable
- exists from once instance to another
- stored in an actual format and stays there, like a hard drive; by contrast in memory, when you close the file the data is gone
- can retrieve the data again and again
- the ability of the object state to be saved to a database, so the state of the object "persists" whether the app is running or not (stack overflow def.)


### Entity Framework workflows

Two main workflows for Entity Framework: database first and code first

First, definitions: 
- domain class: models your data, used to map data from a database to an in-memory object
- "versioning" a database: sharing all versions of a database that are necessary for other team members to get a project up and running

<strong>Database first:</strong> design database tables first, then have Entity Framework create the corresponding domain classes

<strong>Code first:</strong> create your domain classes then have Entity Framework generate the database tables for us

Advantages of a code first workflow:
- faster to code, more productive
- full versioning of database - can migrate to any version of the database at any point in time

### Database migrations

Database migrations track changes to your database schema. A database migration is moving your data from one platform to another. 

Database schema: the structure/organization of a database, how it is contructed
- a description of the data
- defines attributes of a database like tables, columns, properties
- structure that represents the logical view of the database

#### Here are the steps to a code first migration in Visual Studio

Go to: Tools -> NuGet Package Manager -> Package Manager Console

Type in console:
```
>enable-migrations
```

This generates a Migrations folder in your solution.

Then in console:
```
>add-migration "YourMigrationName"
```

This generates a new C# class in the Migrations folder

Finally,
```
>update-database
```

Now if you go to App_Data in your solution, you have a new database file.