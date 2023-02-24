---
layout: post
title: "Getting item count from DynamoDB query, with pagination"
date: 2023-02-22 20:30
tags: dynamodb database debugging aws
navigation: true
class: post-template
current: post
---

At work I had another bug that ended up having to do with database pagination in DynamoDB.

I needed to check if our database had a certain number of items saved. If it did, the program can move onto another task.

I noticed that the next task was sometimes not being completed, especially if there were a large number of items. When I checked the database manually in the AWS console, all of the correct items were there, so we should be able to move onto the next task.

The number of items being returned was supposed to be 186. I noticed that in the console, the number of items would flash 177 before displaying the correct 186. The function that is checking the number of items was also logging 177. I thought maybe DynamoDB had to do more than one fetch to get all the items, and that second fetch wasn't happening in my code.

Turns out that was correct. If the data you're querying reaches 1MB or greater, you need to query again to get the rest, so I needed to do this to get an accurate count. This meant I needed to add `LastEvaluatedKey` somewhere in my code like the last time this issue came up.

When I saw the 1MB value in the AWS docs, I started thinking that I don't actually need to return all this data if all I'm going to do is count it. These resources helped me to understand how to get the count, rather than the data, and how to paginate it.

- [AWS DynamoDB query count](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.html#Query.Count)
- [AWS DynamoDB pagination](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.Pagination.html)
- [Stackoverflow post about getting the count only](https://stackoverflow.com/questions/27316643/how-to-get-item-count-from-dynamodb)

I ended up with the code below (changed somewhat for privacy, all the concepts still apply):

```js
const getItemCount = async (itemId) => {
  try {
    let params = {
      TableName: `my-table`,
      KeyConditionExpression: "PK = :pk AND begins_with(SK, :sk)",
      ExpressionAttributeValues: {
        ":pk": itemId,
        ":sk": "item",
      },
      Select: "COUNT",
      ExclusiveStartKey: null,
    };

    let count = 0;
    let result;

    do {
      result = await dynamo.query(params).promise();

      count += result.data.Count;
      params.ExclusiveStartKey = result.data.LastEvaluatedKey;
    } while (typeof result.data.LastEvaluatedKey !== "undefined");

    return count;
  } catch (e) {
    return e;
  }
};
```

### Explanation

As seen in a previous post, we make use of a do/while loop. We query the database with the same params, but with each query we need to add a value to `ExclusiveStartKey`. That value should be the `LastEvaluatedKey` that was returned from the previous operation. `LastEvaluatedKey` is where the previous query left off, so that's where we need to start the next query. When there is no `LastEvaluatedKey`, meaning it is "undefined", we have retrieved all the results and can exit the loop.

To get the count without returning the data, we need to add `Select: "COUNT"` to our params. We increment the count each time our query returns results, so our final value is the total count.

I also added `ExclusiveStartKey: null` to the params. This is because I am using typescript, and it wouldn't let me add a new field that wasn't defined from the beginning. Having the start key as null doesn't affect the first query, and allows us to update the params object later in our code. You don't have to do this in regular javascript.

### Conclusion

It's important to keep pagination in mind when fetching large amounts of data, and understand the limitations of the database you're working with. As I learned, it can cause unforeseen bugs that are difficult to track down. It's encouraging to me to realize I'm now picking up on these patterns.
