---
layout: post
title: "Implementing hash tables in JavaScript"
date: 2023-08-10 15:30
tags: algorithms data-structures coding-problems javascript
navigation: true
class: post-template
current: post
---

Hash tables are a commonly used data structure and, apparently, are commonly asked about in interviews.

Hash tables store data in key-value pairs. They should be used when you are looking for very quick look-ups. Generally, if you search a hash table for a particular key, it should be able to find it for you without having to search the entire data structure, giving look-ups O(1) time.

Inserting and deleting should also be O(1) time. They take up O(n) space.

Here are a couple links that were helpful for me to learn about hash tables in JavaScript:

- [this freeCodeCamp article](https://www.freecodecamp.org/news/javascript-hash-table-associative-array-hashing-in-js/) for a good overview and a look into a simple JavaScript implementation
- [this educative article](https://www.educative.io/blog/data-strucutres-hash-table-javascript) for learning more about collisions, and generally a good overview
- [this freeCodeCamp video](https://www.youtube.com/watch?v=F95z5Wxd9ks&t=459s) for another explanation and JavaScript implementation

In day-to-day coding with JavaScript, you shouldn't need to implement hash tables yourself, as we have `Objects` and `Maps` that do this for us. The freeCodeCamp article above explains some of the drawbacks of objects, and where maps have made improvements.

Essentially, object methods (inherited from the Object prototype) can be easily overwritten by mistake. Maps require you to use built-in `get` and `set` methods when working with the map, and inherited methods can't be overwritten.

### Implementing hash tables ourselves

There are many ways you could do this, but with the help of the above resources I came up with an implementation I like. Creating a HashTable class made the most sense to me.

Behind the scenes, a hash table is just an array of arrays. You determine the size of the array when you initially create the hashtable. I like to make this size dynamic instead of hardcoded. You could probably change the size if you wanted to later on, but this seemed simpler for now.

Here is the constructor inside the HashTable class:

```js
constructor(numBuckets) {
    this.table = new Array(numBuckets)
    this.size = 0
}
```

Of course, you'd create a new hash table like so:

```js
const hashtable = new HashTable(5); //where 5 is size of array
```

Next we need a hash function. This takes the key of the item you are adding, and converts it into a number. This number is an index in the hash table array. The way we did this here, based off the examples in the resources above, is by calculating a number based on the character codes in the key. Each character is assigned to a numeric code. We sum these values and use the modulo `%` operator to ensure our hash isn't larger than the length of the array.

```js
_hash(key) {
        let hash = 0
        for(let i = 0; i < key.length; i++){
            hash += key.charCodeAt(i)
        }
        return hash % this.table.length
    }
```

We use the hash function everywhere - it's how we translate the key into the index where our item is stored. We use this to get the item, delete it, and of course when we store it for the first time.

### Collisions

One thing that comes up with hash tables is the concept of collisions.

What happens when the hash function generates the same index for two different keys. We want to be able to overwrite keys when they are <em>the same</em>, but not when we have different keys assigned to the same index.

There are lots of ways of dealing with this. The way it's handled in the above mentioned youtube video, and here as well, is by considering each index of the hash table array a `bucket` that can store multiple items.

This complicates the code a bit, and the code's performance as well when we have multiple items in a bucket.

Check out the code for the `set` method:

```js
set(key, value){
        //get an index using hash function
        const index = this._hash(key)

        //check if anything at this index - if there isn't, add item
        if(this.table[index] === undefined){
            this.table[index] = [[key, value]]
            this.size++

        } else{
            let inserted = false

            //check everything in this bucket to see if we already have this key

            for(let i = 0; i < this.table[index].length; i++){
                if(this.table[index][i][0] === key){

                    //if we come across the same key, replace the old value with the new value
                    this.table[index][i][1] = value

                    inserted = true
                }
            }

            //if after checking everything in the bucket we haven't added the new value, push it into the bucket now
            if(inserted === false){
                this.table[index].push([key, value])
            }
        }


    }

```

Without the concept of buckets, this code would be just 3 lines (although there would perhaps be other algorithms needed to check for collisions):

```js
const index = this._hash(key);

this.table[index] = [[key, value]];
this.size++;
```

In our longer set method, you use the hash function to get the index for this key. You then check if there is anything at this index. If not, we just insert it, no big deal. We need to insert it as `[[key, value]]`, because this index is now itself an array. The key-value pair is also an array. So in this implementation, you get a very nested array.

If there is already something at this index, we have a collision. We need to check if this key already exists here, because we would want to overwrite it. Otherwise, we just push our new key-value pair into the array at this index.

The remove and get methods need to work similarly, checking if there are multiple items in the bucket, an then iterating through to get the correct one.

I will reiterate here, that whenever we do this, the time complexity is <strong>no longer O(1) but is now O(n)</strong>, since it is no longer a simple look-up/get/delete using a single key. The bucket concept makes this less efficient, which I feel defeats the purpose of using this data structure, at least if your concern is time. I'd have to look closer at the other options for handling collisions to see if they are any better.

### All the code

There may be some issues I haven't come across yet, but here is the whole HashTable implementation. I like seeing how this can work behind the scenes, but all we really need is objects and maps.

```js
class HashTable {
  constructor(numBuckets) {
    this.table = new Array(numBuckets);
    this.size = 0;
  }

  _hash(key) {
    let hash = 0;
    for (let i = 0; i < key.length; i++) {
      hash += key.charCodeAt(i);
    }
    return hash % this.table.length;
  }

  get(key) {
    const index = this._hash(key);

    //check length of the bucket - if more than one item, have to iterate through, making the operation O(n)

    if (typeof this.table[index] === "undefined") return undefined;

    if (this.table[index].length > 1) {
      for (let i = 0; i < this.table[index].length; i++) {
        if (this.table[index][i][0] === key) {
          return this.table[index][i];
        }
      }
    }

    //check that key matches
    if (this.table[index][0][0] !== key) return undefined;

    //otherwise, just return the item - O(1) operation
    return this.table[index];
  }

  set(key, value) {
    //get an index using hash function
    const index = this._hash(key);

    //check if anything at this index - if there isn't, add item
    if (this.table[index] === undefined) {
      this.table[index] = [[key, value]];
      this.size++;
    } else {
      let inserted = false;

      //check everything in this bucket to see if we already have this key

      for (let i = 0; i < this.table[index].length; i++) {
        if (this.table[index][i][0] === key) {
          //if we come across the same key, replace the old value with the new value
          this.table[index][i][1] = value;

          inserted = true;
        }
      }

      //if after checking everything in the bucket we haven't added the new value, push it into the bucket now
      if (inserted === false) {
        this.table[index].push([key, value]);
      }
    }
  }

  remove(key) {
    const index = this._hash(key);

    //if nothing here, return
    if (this.table[index] === undefined) {
      return false;
    }

    //check if this bucket has multiple items, should be able to splice here since we're not depending on the index *inside* the bucket
    if (this.table[index].length > 1) {
      for (let i = 0; i < this.table[index].length; i++) {
        if (this.table[index][i][0] === key) {
          this.table[index].splice(i, 1);
          return true;
        }
      }
    }

    //if one item here, set to undefined
    //originally wanted to use splice, but that changes the index of everything else

    this.table[index] = undefined;
    this.size--;
    return true;
  }
}
```
