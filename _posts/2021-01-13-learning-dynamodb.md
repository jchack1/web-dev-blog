---
layout: post
title: "Learning DynamoDB"
date: 2021-01-13 14:30
tags: database dynambodb nosql
navigation: true
class: post-template
current: post
---

I have recently been learning about DynamoDB, another NoSQL database.

DynamoDB is a NoSQL database offered by AWS. It has a reputation for being very fast and scalable.

### Resources that helped me

[This youtube tutorial](https://www.youtube.com/watch?v=OfZgHXsYqNE)

[This talk from Rick Houlihan](https://www.youtube.com/watch?v=jzeKPKpucS0)

[DynamoDBGuide](https://www.dynamodbguide.com/what-is-dynamo-db/)

[There are lots of links in this github repo](https://github.com/alexdebrie/awesome-dynamodb)

### Data modelling

Even though it's very flexible it's good to figure out all your access patterns before you begin to model your data.

Your database can use only one table - just one table to hold all your records.
