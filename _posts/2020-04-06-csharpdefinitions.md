---
layout: post
title:  "C# definitions cheatsheet"
date:   2020-04-06 17:50
tags: csharp
navigation: true
class: post-template
current: post
---

Here is some info I compiled as I worked my way through [Codecademy's C# course](https://www.codecademy.com/catalog/language/c-sharp). 

I chose the codecademy course because I could work on it at my office during my practicum. My work computer wouldn't play audio for some reason, so video tutorials weren't an option while I was there. I also found the Microsoft documentation to be not very beginner friendly in its language, and it was the same with many other article style tutorials. The codecademy course was very beginner friendly and interactive so it held my attention better than just reading.

That wasn't an ad or anything, just wanted to share something I found genuinely helpful. Moving on to the definitions...


<strong>Class:</strong> custom data type, defines info and methods <br>
<strong>Object:</strong> each <em>instance</em> of a particular class

We can make many instances of a class, each with unique values of their own

<strong>Fields:</strong> pieces of data, like size and name
- basically like variables
- these start with a lowercase letter eg. name
- fields can have different values in each object/instance

Class <strong>member</strong>: a general term for the building blocks of a class

Some default values in C#: 
- string: null
- int: 0
- bool: false


<strong>Properties:</strong> control access to a field
- another kind of class member
- use the { get; } and { set; } methods
- start with an uppercase letter eg. Name
- inside the set method you can include validation

<strong>Access modifiers:</strong> defines how a type or member can be accessed
- public: can be accessed by ANY class
- private: can only be accessed by code in the SAME class
- protected: can be accessed by the current class and any class that inherits from it
- override/virtual: override an inherited method. Use "override" in subclass, "virtual" in base class
- abstract: use when there is no implementation in base class, but it must be in the subclass

<strong>Methods:</strong> actions an object can perform
- if familiar with JavaScript, they are like functions
- most belong to a class
- define how an instance of the class behaves

<strong>Constructor:</strong> type of method that sets values of fields when you create a new instance. See below for an example of a constructor:

```csharp
//define a constructor in your class
//again, codecademy example
public Forest(string name, string biome)
{
    //whatever values you input as parameters are assigned to the below properties
    Name = name;
    Biome = biome;
    Age = 0; //age starts at 0 for all new forests
}
```

Use the constructor with the <strong>"new"</strong> keyword to make a new instance, as per below:
```csharp
Forest f = new Forest();
//example from codecademy
```
In the above example, "Forest" is the type, "f" is the name of the new object/instance you are making, Forest(); is the constructor that makes the new instance.


<strong>this</strong>: refers to the <em>current instance</em>
- it's good to use this and be more explicit in your code; less chance for misinterpretation

<strong>Static:</strong> information that is related to a class, but is not an instance of the class
- something that applies to all instances and there should only be one value for the whole class
- a static member is accessed from the class, not an instance

Example:
```csharp
Console.WriteLine(Forest.Definition);
```

<strong>Overload:</strong> two or more methods that have the same name, but different parameters

<strong>Interfaces:</strong> sets of properties, methods, and other members that tell us how a class can be used
- helps check that we are using our types correctly
- helps minimize bugs
- doesn't specify how they work, just that a class MUST have them
- guarantee certain functionality across multiple classes
- all start with "I"
- the class must implement properties and methods in the interface
- cannot specify constructors or fields

```csharp
class Sedan : IAutomobile
{
    //the interface says the sedan must have a "Honk" method, so we must include one in the sedan class
    //must be "public"
    //interface doesn't care what "Honk" actually does 
}
```

<strong>Inheritance:</strong> a subclass/derived class inherits members of a superclass/base class

```csharp
class Sedan : Vehicle
//subclass Sedan inherits from base class Vehicle
//must use ":"
```

Both of the below are variables:
- <strong>Reference types:</strong> refer to a place in memory
- <strong>Value types:</strong> hold actual data

Classes are reference types. When we create an instance of an object, and store it in a variable, it is a reference to the original object.

It may seem obvious when written this way, but a "reference" is not an object, it is a <em>reference to</em> an object.

```csharp
Dissertation diss1 = new Dissertation();
//from codecademy
//diss1 is a reference to the object Dissertation
```

That's all I have for now. As I continue going through my notes I will add to this post.