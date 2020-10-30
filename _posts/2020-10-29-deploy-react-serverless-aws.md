---
layout: post
title: "Deploying simple React app with Serverless / AWS "
date: 2020-10-29 18:30
tags: react aws serverless
navigation: true
class: post-template
current: post
published: false
---

I am learning Serverless and AWS for work. Here is what I did to deploy a basic app with Serverless.

This is assuming you already have and AWS account, and have installed serverless, the aws sdk, and set up your aws credentials on your machine.

In this case I already had a project made, which I set up using `create-react-app`. You can also set up a new serverless app by creating a new folder and running `serverless create --template aws-nodejs`. (assuming you are using Node.js in the backend)

To deploy static pages to an S3 bucket on AWS, I installed serverless-finch in my project using `npm i --save-dev serverless-finch`.

When deploying a React app it is important to run the command `npm run build`, so you get a build folder in your project directory. This is where all your static files end up including your index.html, which S3 needs to find in order to deploy your site.

Setting up a new serverless app will give you a handler.js (if using node.js), and a serverless.yml file. serverless.yml is where you configure everything. Serverless uses this file to work with AWS basically to deploy your app.

The following is what I have in my serverless.yml file:

```yml
service: react-practice

provider:
  name: aws
  stage: ${opt:stage, 'dev'}
  runtime: nodejs12.x
  region: us-west-2

functions:
  hello:
    handler: handler.hello
    #    The following are a few example events you can configure
    #    NOTE: Please make sure to change your handler code to work with those events
    #    Check the event documentation for details
    events:
      - http:
          path: hello
          method: get

plugins:
  - serverless-finch

custom:
  client:
    bucketName: julia-practice-react1
    distributionFolder: /build
    errorDocument: index.html
```

Note that under "plugins" I added "serverless-finch".

Under "custom" and "client" we configure S3.

bucketName the bucket where S3 will place our files. It must be a globally unique name, meaning no one else in the world have have a bucket with this name.

distributionFolder is where S3 should look for our files. For React we use "/build", since that's where our index.html is.

S3 also requires you have an errorDocument specified. Since this is just a simple app I didn't make one and just left it as index.html.

### Deployment

For backend stuff, especially for Lambda functions, run `sls deploy --aws-profile={whatever name you set up in your aws credentials file}`

To deploy the client side, into your S3 bucket, run this command `sls client deploy --aws-profile={whatever name you set up in your aws credentials file}`

Your screen should look something like this if successful. You will see a link you can go to and view the site.
<img src="../assets/images/sls-cli.jpg" style="max-width: 500px;" alt="serverless cli successful"/>

### Errors

I got an error reading "Serverless Error: Access Denied". This seemed strange because I never got this error for the backend deploy, so I thought it must have to do with S3. I fixed this by making my bucket public, but I don't think this is very secure so I need to look into this further.

Another error said that I was not in the correct region. This was because my bucket and my serverless.yml file "region" did not match. I just had to quickly update my .yml file and there error was solved. I could now see my site.
