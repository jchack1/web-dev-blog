---
layout: post
title:  "500 server error and the csproj file"
date:   2020-05-22 12:30
tags: csharp dotnet 
navigation: true
class: post-template
current: post
---

Here is what I learned when we got a 500 server error while testing my code at work.

I was tasked to create a button that would copy rows of a list to another position in the list. We changed the functionality later on, but originally I created a button that opened a modal, which required user input to carry out the copy behavior. I created new javascript and cshtml files for the modal.

I am still learning the ins and outs of Sourcetree, our version control software, but I knew that I had selected these files and pushed them to our main development branch. When the button was clicked during testing, a modal popped up, but its content was "500 server error". I couldn't understand why this would be, since I knew I pushed the modal files.

Back in Visual Studio, I noticed that the new modal files were no longer showing up in solution explorer, but I could see them in file explorer on my PC. Plus I had the correct button functionality locally on my machine. It turns out there was one file that I never pushed, and that was the csproj file.

I [learned that MSBuild](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file), the engine that builds your solutions in .NET, uses the csproj file.  This file tells MSBuild that we are building a C# project and gives instructions for how to build it, like database server settings, platform requirements, and what files to include, among other things.

The problem was with the "include" code, which looks like this:
```csharp
/*snipit from microsoft docs*/
<Content Include="Content\Custom.css" />
<Content Include="CreateDatabase.sql" />
<Content Include="DropDatabase.sql" />
```

 I never technically included the new files in the project, so the application didn't know to look for them. Here are the steps taken to fix this:

- if you can't see your files in solution explorer, go to Project -> Show All Files
- files should be visible now in solution explorer; right click -> "Include in Project"
- you will see them included in .csproj file
- commit this file in Sourcetree and push

No more 500 error!
