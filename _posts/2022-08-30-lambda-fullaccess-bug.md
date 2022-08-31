---
layout: post
title: "Debugging serverless: 'AWSLambdaFullAccess does not exist'"
date: 2022-08-30 18:30
tags: debugging aws serverless
navigation: true
class: post-template
current: post
---

I don't write down the solutions to bugs often enough, so here is how I solved a serverless bug today.

Maintenance tasks are never quite as simple as they appear at first glance. Last year I updated all our lambda functions at work from Node 12 (and lower) to Node 14 - or so I thought. It turns out there were a few I missed, mostly in projects that are not worked on often.

A year is a long time to not deploy a serverless app into AWS. I updated the runtime to Node 14, and attempted to deploy. There were several errors, one after the other, that I managed to fix. The last error I received had me stuck for a while.

` CREATE_FAILED: CodeDeployServiceRole (AWS::IAM::Role) Policy arn:aws:iam::aws:policy/AWSLambdaFullAccess does not exist or is not attachable`

While there was some CodeDeploy code in the app, there was never any line of code where we explicitly set "AWSLambdaFullAccess". I tried commenting out the CodeDeploy code under the IAM role section:

```yaml
- Effect: Allow
  Action:
    - codedeploy:*
  Resource:
    - "*"
```

But that didn't do anything.

After some googling, I saw that the AWSLambdaFullAccess policy had been deprecated, and AWSLambda_FullAccess was its replacement. I vaguely remembered seeing this in an AWS email sometime in the past year. The problem must be connected to this, somehow.

I checked the serverless-generated cloudformation files in my .serverless folder. There was code for the creation of a AWSLambdaFullAccess policy. But none of our own code was telling it to do that. I thought maybe I had an old version of serverless, but I recently updated my local environment to version 3, and my serverless.yml file didn't specify a framework version, so that couldn't be it.

After lots of trial and error, and more googling, I went back to a github issues page where someone else reported the same issue. At first glace I didn't see a solution for my problem, but I scrolled a little further and found something helpful.

### Solution

One commenter said their issue was that they had an old version of a plugin "serverless-plugin-canary-deployments". We had that plugin, too. I thought I had checked the plugin versions and found they were the same across our different microservices, and these microservices never gave the same error. Regardless, I updated that npm package to the latest version and deployed. This time it worked!

It's possible to receive this same error and have an entirely different issue causing the problem, but this worked for me. Perhaps we can generalize this and say that if you are getting an error related to a deprecation, have a look at the versions of your dependencies.
