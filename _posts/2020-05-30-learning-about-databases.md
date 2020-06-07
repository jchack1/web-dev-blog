---
layout: post
title:  "Learning about databases in Udemy course"
date:   2020-05-30 18:30
tags: database node.js mongodb mongoose
navigation: true
class: post-template
current: post
---

I often feel I need to drill this information into my head a few times before I really get it, so here are some more notes with basic definitions surrounding databases. 

These notes are from the database section of my Web Developer Bootcamp Udemy course, and also include some of my own notes about connecting to MongoDB Atlas.

### Database

A collection of information or data that you access using an interface - not just any file with information. An interface here means that you use code to interact with the database.

For example, SQL is an interface to some databases.

### Relational(SQL) vs. non-relational(No-SQL) databases

Relational databases are "tabular" databases and are considered flat. You have to define what your tables look like first, and the relationships between the tables. You define relationships by creating a new table, called a join table. Relational databases are not very flexible if you want to make changes later. 

For non-relational databases you don't have to define these patterns ahead of time, so they are more flexible. There are no tables. They can have nested data so are therefore not flat.

### MongoDB and Mongoose

We are using MongoDB in the Udemy course, and we used it in my certificate program as well. That 's because it is currently the most popular database to use with Node.js. In class we used MongoDB Atlas, which is a cloud-based system for storing your data with MongoDB. You can also set up MongoDB on your machine and interact with it using the command line. There are many articles on how to set this up, and I didn't want to go through all those steps since I already have a good system in place with Atlas. You can see the [MongoDB docs](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/) and this [freeCodeCamp article](https://www.freecodecamp.org/news/learn-mongodb-a4ce205e7739/) for set up.

We are also using the Mongoose library to model our javascript objects. Mongoose uses schemas, like below:

```js
//create your schema
const campgroundSchema = new mongoose.Schema({
    name: String,
    image: String
});

//use your schema to model what data a Campground object will include - needs to start with a capital letter 
const Campground = mongoose.model("Campground", campgroundSchema);

//create a new campground
Campground.create(
    {
        name: "Granite Hill", 
        image: "https://i.picsum.photos/id/654/200/300.jpg"}, function(err, campground){
            if(err){
                console.log(err);
            }else{
                console.log("Newly created campground");
                console.log(campground);
            }
        });

```

Even though you are defining name and image in your schema, it is not set in stone; you can modify it later, unlike tables in relational databases.

The following is what I use to connect to mongoose and my database:

```js
//connecting to mongoose
mongoose.connect(process.env.DB_CONNECTION, {
    useUnifiedTopology:true, useNewUrlParser: true
});

//define db
const db = mongoose.connection;

//error handling and letting us know if we are connected to our database
db.on("error", console.error.bind(console, "Error connecting to mongoose"));
db.once("open", function(){
      console.log("Connected to the database");
  })
```

The part with "process.env.DB_CONNECTION" is how I connect to my database in the cloud. In my code I also have a .env file, which is included in my .gitignore and NOT uploaded to github, because it contains confidential information like my username and password.

Putting your username and password directly in your code is of course very insecure. You need to install "dotenv" in your project, and the content of your .env file may look like this:

```js
DB_CONNECTION="mongodb+srv://username:password@cluster0-abc.mongodb.net/mycollection?retryWrites=true&w=majority"
PORT=3000
```

DB_CONNECTION is your connection string. Here, "mycollection" is the name of the collection you are connecting to, and you can see where you include your username and password. Change the collection name for each new project.  Mongo will create the new collection automatically, and you can check that it was made when you log into MongoDB Atlas and go to your collections. PORT was included here as well to define the port we want to use as port 3000.

Here is an example connection string template from the [MongoDB docs](https://docs.mongodb.com/manual/reference/connection-string/):

```
mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]

```

### Useful commands

Let's say you have a database called "cat_app" with a collection called "cats". You can interact with it using these commands in the mongo console.

- help
//actually prints all the commands you can use

- show dbs
//will print names of your databases

- use cat_app
//switches you to the cat_app database, prints out a message to confirm

- show collections
//prints the collections in cat_app

- db.cats.find()
//prints out all the cats in the cats collection
//can include data inside find() like find({breed: "whatever"}) to filter the data, finding all the cats of a particular breed

- db.cats.insert({name: "Fluffy"})
//inserts data into the cats collection

- db.cats.update()
//update data of a cat

- db.cats.remove({breed: "whatever"})
//remove the cats of a particular breed













