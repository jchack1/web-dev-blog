---
layout: post
title: "Learning AWS - basics"
date: 2020-11-23 18:30
tags: aws
navigation: true
class: post-template
current: post
published: false
---

I am starting a course in AWS on Udemy. Here are some notes from the early parts of the course.

AWS regions: clusters of data centers

When choosing a region try to choose one that is close to you. If you choose one that is too far away, things may take longer to load. If a service you are looking for is not available in your region simply choose a different region.

AWS availability zones (AZ): usually 3 AZs per region, sometimes 6

- each AZ includes one or more data centers
- separated geographically but still connected on the network

There are global and regional services

- EC2 is regional, so if you change your region, you'd have to set up another EC2 service
- IAM is global, so you don't have to select a region when you use it - your IAM settings work across everything

Global Infrastructure page - gives you information about regions, AZs, what services are offered in each region

### IAM

IAM = identity and access management

AWS security is here, consists of users, roles, groups. IAM is the center of all your services.

- Root account is used to create users
- Groups are made up of users, can be organized by teams, what the users do, etc.
- Roles are for internal AWS usage. Roles are given to machines, whereas users are real people
- Policies control what users, groups, and roles can do and are written in JSON
