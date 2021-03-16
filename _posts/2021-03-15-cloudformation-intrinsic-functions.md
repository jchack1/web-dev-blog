---
layout: post
title: "AWS Cloudformation - intrinsic functions"
date: 2021-03-15 14:30
tags: aws
navigation: true
class: post-template
current: post
---

AWS Cloudformation is a really great AWS service for creating "infrastructure as code".

If you have ever dabbled in AWS it's possible that you have been creating your resources in the browser in the AWS console. I will say I enjoy creating my resources this way because I am a visual person and sometimes enjoy using a GUI. However, when working on real-world projects that involve other developers and AWS accounts and environments, it would become tedious to manually create these over and over again in the console. And it would be awful to have to re-create them in the case that they are accidentally deleted.

Cloudformation allows you to create template files in YAML or JSON (I use YAML, I think this is most common) to declaratively create your AWS resources. The templates can be uploaded in the AWS console, or in the command line. For work I have my AWS credentials saved in a file on my computer, and I am able to deploy my templates from the command line. Despite what I said about enjoying using a GUI, once I got used to pushing code from the command line, that became my preference. It's just so quick and easy once you get up and running. I imagine I'll feel this way about Cloudformation with more experience.

No surprise here but I am taking a course in AWS as well - Ultimate AWS Certified Developer Associate 2021 on Udemy. Some of the info here comes from that course as I work my way through the cloudformation section, as well as the AWS documentation.

### Intrinsic functions

As I was diving deeper into AWS Cloudformation I started to come across a lot of weird things I had never seen before while working with YAML. Things like !Ref, fn::GetAtt, etc. These are called <strong>intrinsic functions</strong> and essentially allow you to make references to resouces and parameters inside your cloudformation template.

First, a couple definitions:

<strong>Resources:</strong> This is a required section in your template, which is where you declare the AWS resources that you want included in your stack. A resource is an AWS entity/service you can work with, like an EC2 instance, an SNS topic, or an S3 bucket.

<strong>Parameters:</strong> This is an optional section in your template. Parameters in a cloudformation template are like parameters you pass into a function. You define specific values for your parameters which can be referenced later on in your template. If you are referencing these parameters many times in your template, but you need to make a change, it is easier to change them once at the top of your file than have to update them several times throughout your code.

When you see Fn::Something or !Something in the code you are dealing with an intrinsic function.

Fn::Ref

- in YAML code is !Ref
- most common
- can be used to reference a parameter - gives you the exact <strong>value</strong> of the parameter
- can be used to reference a resource - give you the <strong>physical ID</strong> of the resource

Fn::GetAtt

- sometimes you need more than just the ID of a resource
- attach to any resource, check docs to see the <em>attributes that are exposed</em> by each type of resource
- in YAML code it looks like this: !GetAtt MyInstance.AvailabilityZone
- so, use !GetAtt, with the resource name, and the attribute you want

Fn::FindInMap

- used similar to GetAtt, but to get a value out of a "Mapping" you made in your template
- a "mapping" is where you hardcode values in your template that, for example, depend on what environment you're in
- maybe you are using a different image to create an EC2 instance based on what region you're in, or whether you are in qa, dev, or prod environment. Hardcode these into your template and reference your map later on
- in your code use like so: !FindInMap [MapName, TopLevelKey, SecondLevelKey]

Fn::ImportValue

- to get values exported from another cloudformation template in its "Outputs" Section
- your exports should have unique names across the region you're working in
- used in YAML code: !ImportValue ExportedResourceName

Fn::Join

- to join a list of values with a delimiter (a delimiter is a character that separates values - could be a comma, colon, etc)
- !Join [delimiter, [list of values]]
- puts the delimiter in between each value in the list

Fn::Sub

- to substitute a particular value with other values you specify
- !Sub
