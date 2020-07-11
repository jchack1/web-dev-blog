---
layout: post
title:  "Learning SQL"
date:   2020-07-01 18:30
tags: sql database
navigation: true
class: post-template
current: post
---

Another set of notes while I am doing a course, this time in SQL.

This time I am following [this freeCodeCamp SQL tutorial on youtube](https://www.youtube.com/watch?v=HXV3zeQKqGY&list=WL&index=111&t=1s).

### Tables

Columns: a type of information like name, email, password, etc

Row: a unique entry in the database
- e.g. in a "student" table, a row would be a unique student including their ID #, name, major, email, etc

Primary key: an attribute that uniquely defines a row in the database
- can be anything - a number, string, etc - but it has to uniquely identify a row

Whenever you want to store data:
- define a table with all your columns
- insert specific pieces of information into the table

Surrogate key: doesn't map to anything in the real world
- a type of primary key
- e.g. a random number assigned to an employee in an "employee" table
- used to represent the employee in the database but doesn't really mean anything

Natural key: has a purpose in the real world, not just in the database
- e.g. a social insurance number

Foreign key: an attribute we can store in a table that links us to another table in the database
- basically the primary key of a row in another table
- helps us define relationships between the tables

Composite key: needs two attributes/columns
- both together are needed to identify a particular row
- for if you have columns with repeated values - combine with another column to give a unique row
- another type of primary key


### SQL basics

- query language used to interact with a relational database management system (RDBMS)
- RDBMS examples: MySQL, Oracle, Microsoft SQL Server

SQL used for:
- CRUD - creating, reading, updating, deleting information
- manage databases
- create databases
- design and create tables
- admin tasks, like managing users and security
- define database schemas

Query: set of instructions given to the RDBMS, in SQL, to tell it what information you want it to retrieve
- goal is to get exactly the data you need - from potentially very large data sets 


### Installing MySQL and PopSQL

This is the install process for MySQL in this course:
- go to [dev.mysql.com](https://dev.mysql.com/downloads/mysql/) and download the community version of MySQL; I downloaded the MSI file rather than the zip
- open the installer
- chose custom install
- from available products I chose MySQL Server 8.0.20 - x64 and MySQL Shell 8.0.20 - x64
- on the installation tab click execute
- when those finish installing, on the type and networking screen, choose Standalone MySQL Server/Classic MySQL Replication
- keep all the other default settings
- enter a password, no need to create any more user accounts unless you want to
- on apply configuration page click execute
- then the shell will be installed

To create a database, open MySQL 8.0 Command Line Client, and enter the password you set earlier. This connects you to MySQL server on your machine.

Then type "create database {database-name};"

We also installed a text editor for SQL called PopSQL. It helps to visualize what's going on but you can type all the same commands into the command line client. Just go to popsql.com and download. You need to create an account. Once installed create a new connection to the database. Fill in the following:

- Type: MySQL
- Nickname: whatever you want
- Hostname: localhost
- Port: 3306
- Database: whatever you named your database in the command line 
- Username/password: whatever you used when you set up MySQL. if you just used the default username during setup, enter "root"
- Click connect


### Common data types

- INT - whole #
- DECIMAL(10, 4) - decimal #, () specifying number of digits
    - in this case, it means this decimal number has 10 digits total with 4 coming after the decimal
- VARCHAR(1) - string, number of characters in ()
- BLOB - binary large object, stores large amounts of binary data, like image files 
- DATE - formatted as YYYY-MM-DD
- TIMESTAMP - formatted as YYYY-MM-DD HH:MM:SS

### Creating tables/columns

First step when working with RDBMS you have to create tables.

Convention is to write SQL in all caps, to distinguish it from other text in our queries.

End SQL commands in semicolons.

In the below example, you are naming the columns as student_id, name, and major, declaring their data types, and setting the primary key to student_id.

```sql
CREATE TABLE student (
    student_id INT PRIMARY KEY,
    name VARCHAR(20),
    major VARCHAR(20)
);
```

Below, in PopSql, this shows us the student table:

```sql
DESCRIBE student;
```

To delete a table:

```sql
DROP TABLE student;
```

Altering a table:
```sql
/*to add a column*/
ALTER TABLE student ADD gpa DECIMAL(3, 2);

/*to delete a column*/
ALTER TABLE student DROP COLUMN gpa;

```

### Inserting data


Inside () put data you want to store:

```sql

INSERT INTO student VALUES(1, 'Jack', 'Biology');

/*to see the data in your table*/
SELECT * FROM student;

/*if you don't know all the values; the one you leave out will just show as NULL in the column*/
INSERT INTO student(student_id, name) VALUES(1, 'Jack');



```

### Update data

Let's say you want to change all instances of "Biology" major to "Bio":

```sql
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';
```
If you do the below query, not using the WHERE statement, it will apply to all columns:

```sql
UPDATE student
SET major = 'undecided';
```

### Deleting data

To delete one student from the student table,

```sql
DELETE FROM student
WHERE name = 'Tom' AND major = 'undecided';
```

To delete everything from the table, giving an empty table:

```sql
DELETE FROM student;
```
