---
layout: post
title:  "Beginning to learn .NET"
date:   2020-02-17 22:30
tags: dotnet MVC
navigation: true
class: post-template
current: post
---

I am currently in the middle of the practicum component of my program. I am entering week 3 of 8, and to be honest I still have a lot of learning to do in order to be useful here.  I have already learned a lot of things that were not taught in my program, which I guess is the point of working in a real office. It hasn't been easy and I suspect it will continue that way.

The source of all my trouble is the fact that until now I never learned C# or .NET, and we spent maybe half and hour in class on MVC.  I consider myself to be more skilled at front end development, but my job is almost entirely back end. At least so far. Having knowledge of ASP.NET and MVC applications beforehand would have been extremely helpful. I believe the most helpful information will be anything to do with the structure of MVC applications, so I can understand how to navigate my company's project. One challenge I have found is that tutorials and articles relating to MVC assume a basic to intermediate knowledge of C# already. And the C# tutorials start out at a level that is too beginner for me, given that I already know how to add, subtract, use logical operators and conditionals... I'm on a bit of a time crunch to learn MVC and .NET. And yet my progress feels too slow given that my practicum is only 2 months long.

Anyway, I'll get into some of the things I've learned so far.  My work notebook is filling up very quickly, but perhaps I'll just compile what I've learned through my studies at home.

There are many, many, many definitions online of the model, view and controller. On a [code analogies blog](https://blog.codeanalogies.com/2016/05/02/model-view-controller-mvc-explained-through-ordering-drinks-at-the-bar/) the author related it to ordering a drink at a bar. Your order is the request, the bartender (and the drink recipe they memorized) is the controller, the ingredients are the model, and the finished drink is the view. Actually very helpful. 

Here are more notes and definitions I've been able to compile from various courses and articles, including my Udemy course on ASP.NET MVC Applications:

Model:
- the application data and behavior
- is independent of UI
- independent of data persistence 

View:
- the HTML displayed to the user

Controller:
- handles the HTTP request
- used to define and group a set of actions together - see more on actions below
- any instantiable class ending in "Controller"

(bonus)Router:
- selects which controller will be used, based on the the request that comes in

Also a little bit about Entity Framework, which I have learned is important but can't seem to find plain English definitions of:
- need a persistence framework to access the database in an app
- helps to load objects from a database, and save to a database
- need this to use dbcontext in your files

Yeah, I don't know. We'll revist later, maybe with my work notes. Onward!

Creating a new controller:
- very important fact! When you create a new controller in Visual Studio, you will automatically get a new folder created in Views with the same name
- when it needs to return a view, the controller will search for the view in a folder with the same name!

Client side stuff/CSS:
- ASP.NET uses Bootstrap for CSS.  This is pretty obvious if you have used Bootstrap, when you see the ASP.NET default page... It's fine.  It's good if you don't have time/don't like/aren't good at front end.
- You can use bootswatch.com to get Bootstrap templates to use in your application if you don't like the default.
- Go to bootswatch and download the bootstrap.css file for your preferred theme. You can rename if you like. Place in your Content folder of your MVC project
- You will need to update your Bundle config file if you want to use your new theme, and the file name is different. Go to App_Start -> BundleConfig.cs.  Scroll and look for Content/bootstrap.css, and update the path name if necessary
- FYI, the BundleConfig.cs file is where you define client-side bundles and assets.


Action and ActionResult
- An action is a method in a controller responsible for handling incoming requests
- Routing maps the requests to our actions
- An ActionResult is the output of our actions
- Action Parameters are the inputs for our actions
- Any public method on a controller type is an action
- Actions are basically the final destination of our requests in an MVC app

To rename a parameter:
- Hit F2
- Type new name
- All references are now updated automatically in Visual Studio

Nullable: to make a parameter nullable means it's ok for it to have no value passed it - it's ok for it to be "null"
- for int and double data types, put "?" afterwards
- for example: int? = ...
- don't need to put "?" after strings, because apparently they are already nullable in C#


Ok, that's all for this post. I will write more as I continue with my courses, and when I remember to bring home my work notebook. 


