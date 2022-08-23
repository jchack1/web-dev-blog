---
layout: post
title: "DynamoDB scans and pagination"
date: 2022-08-22 18:30
tags: dynamodb database debugging aws
navigation: true
class: post-template
current: post
---

Today I was working on a task that involved archiving some old data in DynamoDB. I didn't want to permanently delete my data, so I needed to retrieve the data, create a new record in the archive database, and delete the record from the original database.

These records were never meant to be queried all at once. Normally in the app you would query these records for one user at a time. My PK and SK were not set up to get all of these records as one batch, which meant I needed to scan the database and filter out all of these records in my retrieval step, rather than doing a more efficient query.

I wrote the code and invoked it in my lambda console. I invoked it multiple times and noticed in my logs that I was retrieving and archiving new records each time. It wasn't supposed to work like this - I should only need to invoke this function once, and all of the records are archived. My scan wasn't getting all of the records.

One [stack overflow page](https://stackoverflow.com/questions/66337345/amazon-dynamodb-scan-is-not-scanning-complete-table) brought to my attention a value that is returned from DynamoDB scans and queries called "LastEvaluatedKey", which is logged to the console when there are more records left to be retrieved. This is how DynamoDB paginates large amounts of data.

According to the [AWS developer guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html), this happens after a maximum of 1MB of data has been returned. If there is no "LastEvaluatedKey" logged to the console, there are no more results.

### How to fix this

I still needed to get all the data. If there are still records left over, you need to do another scan and tell it where the previous scan left off. You do this by making the value "ExclusiveStartKey" in your current scan equal to the previous scan's "LastEvaluatedKey". You do this until there are no more records left to retrieve, which means your last scan hasn't returned a value for "LastEvaluatedKey".

```js
//not the exact same code for privacy reasons

const scanDatabase = async () => {
  const params = {
    TableName: "myTable",
    FilterExpression: "contains(PK, :pk) AND begins_with(SK, :sk)",
    ExpressionAttributeValues: {
      ":pk": "team-stats",
      ":sk": "1",
    },
  };

  let scanResults = [];
  let items;

  do {
    items = await dynamo.scan(params).promise();
    items.Items.forEach((item) => scanResults.push(item));
    params.ExclusiveStartKey = items.LastEvaluatedKey;
  } while (typeof items.LastEvaluatedKey !== "undefined");

  return scanResults;
};
```

This could have been a use case for a recursive function, but I based my code off one of the answers on the stack overflow page which used a do-while loop. It seemed like a simple, readable solution, and performed reasonably well as my archive function eneded up taking only a couple minutes to complete.

This helped me to learn a little more about pagination, but also brought up a few questions for me. Are there any other cases in my codebase where the data being requested is more than 1MB? Are we missing data and not realizing it? I will have to check some of our other queries and see if "LastEvaluatedKey" is being returned.
