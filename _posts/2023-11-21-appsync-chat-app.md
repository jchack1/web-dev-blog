---
layout: post
title: "Developing a chat app using AWS AppSync"
date: 2023-11-21 16:30
tags: aws serverless appsync
navigation: true
class: post-template
current: post
---

I recently created a chat app at work using AWS AppSync, and then decided to re-create it on my own time.

There were some new features rolled out in AppSync since I started my work project, so I wanted to try them out. It also gave me the opportunity to learn more about setting up authentication with Amplify and Cognito, which I had never tackled before, as authentication had been set up at work by another developer.

Everything below has also been included on my portfolio site.

### Why I chose to use AppSync

To create a chat app you need to ensure messages appear on the screen in real time. There needs to be a bi-directional relationship between the front-end and database, and the front-end must display messages immediately upon their creation without refreshing the page. One option for this could have been to use Apollo Client WebSocket Link.

I attempted this with my original work project. The front-end set-up was simple, but I quickly realized that setting up a web-socket on a serverless app built with AWS Lambda would be challenging. This required manual set-up of the web-socket, creating a new connection each time a user logged in, and storing the connection information in the database. It was much simpler to use AppSync, which removed all of this manual work.

### The nitty-gritty of my AppSync experience

AppSync allows you to create a GraphQL API that connects to your backend datasources. Resolvers are the functions in your API that handle incoming requests. For my projects, resolvers generally will receive a request, get something from or put something into a DynamoDB database, and return some data to the front-end. A few times I used Lambda functions, connected to DynamoDB, as my datasource, which I will elaborate on later.

AppSync initially used VTL (Velocity Template Language), which is a Java-based language, to write mapping template code (basically equivalent to resolvers). When I began the original project, AWS just began to roll out JavaScript-based resolvers in a limited capacity. Since it was such a new functionality, and all the AppSync tutorials by my favorite devs on social media were using VTL, I decided to use VTL as well. It was time consuming for me to learn, and debugging in AppSync was often difficult, but eventually I figured it out and delivered a working app to my team.

When it came time for me to create my own project, JavaScript could be used with any type of resolver. I assumed this would make my life easier and coding this project would be faster. This was not the case. JavaScript is not run on a browser or Node.js runtime, but on a special `APPSYNC_JS` runtime, and there are limits on what JavaScript features may be used. For example, `try/catch` statements are not supported, making error handling challenging; you need to use their built-in functions. `async/await` and promises are also not supported. While I understand the logic behind this - awaiting promises is too time-consuming and your AppSync API is supposed to be fast - this presented challenges for my multi-step code.

Pipeline resolvers are made for this scenario. This could have worked, except there were some cases when I needed to query multiple items from the database using a global secondary index (GSI). There is a built-in AppSync function called `BatchGet` which I tried to use, however it will only allow you to get items using the primary key, no secondary indexes, so it didn’t work for me.

At this point I could have re-worked my data structures, but instead I put my JavaScript code into Lambda functions and used them as the datasource. I was able to use the JavaScript functionality I was familiar with and write the exact DynamoDB queries I needed. For my purposes this worked fine, and I never noticed any performance issues.

### Take-aways and future updates

Overall I enjoyed learning AppSync. It’s a big time-saver to not have to manually set up web-sockets. Writing the resolvers was the most challenging aspect, since there seemed to be a lot of restrictions on what could be included in your code. I often received errors that said “there is a problem with the code” but nothing more, so debugging was difficult.

Working with GraphQL is a great experience since it helps me think of my code in terms of inputs and outputs, and it always makes me want to work more with types, specifically using TypeScript. I would have written the backend for this chat app in TypeScript, but I struggled with importing my own types into the code. Again, I got the “there is a problem with the code” error, so it wasn’t clear what exactly the problem was.

It seems as if AppSync wants you to use it to auto-generate all your types, schemas, and datasources, when I want to write it all myself. In the future I’d like to revisit this; I’d like to see if the developers make this process easier, or dive deeper into the docs to find out if I missed any key details for TypeScript implementation. Perhaps in this environment of AI and cloud services, I need to better embrace the ability of these kinds of services to auto-generate my code and infrastructure for me, if I want to produce software quickly.

### Helpful resources

- [Foobar Serverless github repo and accompanying youtube video](https://github.com/mavi888/web-client-appsync-test/tree/master)
- [serverless-appsync-plugin docs](https://github.com/sid88in/serverless-appsync-plugin/tree/master/doc)
- [AWS AppSync developer guide](https://docs.aws.amazon.com/appsync/latest/devguide/what-is-appsync.html)
