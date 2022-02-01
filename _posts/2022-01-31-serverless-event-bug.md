---
layout: post
title: "Debugging serverless - Eventbridge rule already exists"
date: 2022-01-31 18:30
tags: aws debugging serverless
navigation: true
class: post-template
current: post
---

I recently came across a bug while deploying a serverless microservice using github actions. I may have seen this error before at some point, but I never wrote down the solution, so here is how I solved it this time.

This is the error message, with the real function and service names changed:

<em>CREATE_FAILED: AccountsqadeleteAccountrule1EventBridgeRule (AWS::Events::Rule)
myservice-events-qa|accounts-qa-deleteAccount-rule-1 already exists</em>

This is a message from Cloudformation telling us that our resource, in this case an event rule, couldn't be created.

The strange part is that I only encountered this issue when I deployed to an AWS account via github actions (to my QA account), not when I deployed to my dev account from my local machine. There had to be a difference between my dev and QA environments.

This also only happened with services that had Lambda functions triggered by Eventbridge. Makes sense given that the error message has to do with event rules, though sometimes there can be other explanations for these kinds of problems.

### Problem solving

One of the steps involved in github actions deployment is to install serverless on the virtual machine github uses to deploy. The logs told me the environment looked like this:

` linux, node 12.22.9, framework 3.0.1, plugin 6.0.0, SDK 4.3.0`

Notice "framework: 3.0.1". I remembered that my local serverless version was still at version 2, and I had not updated it since originally installing it. I confirmed my local version was 2.60.0.

Maybe there were some breaking changes since the new version came out. But when did version 3 even come out? If it's a recent change, maybe that's the cause of my problems.

I did a quick search and found out that version 3 came out a day or two before I started getting my error. In fact, there was [an article from January 27th](https://www.serverless.com/blog/serverless-framework-v3-is-live) about the release of version 3, saying it is now live.

To be sure this was the problem, I checked the last successful deployment of one of my problem services from a few days earlier, and during the build process serverless@2.72.2 was installed.

### Solution

The solution to this specific Eventbridge problem was just a few lines of code in my serverless.yml file:

```yml
provider:
  eventBridge:
    useCloudFormation: false
```

This is because Eventbridge resources will now be deployed using native Cloudformation instead of Lambda, [which you can read about here](https://www.serverless.com/framework/docs/deprecations#aws-eventbridge-lambda-event-triggers). The solution above won't work forever, so I'll need to do some maintenance on all our serverless projects to bring them up-to-date.
