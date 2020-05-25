---
layout: post
title:  "Reviewing API calls"
date:   2020-05-18 16:30
tags: javascript node.js api
navigation: true
class: post-template
current: post
---

Today in my Web Developer course I am reviewing how to use APIs.

This was a topic I really struggled with the first time around while in school.  I just couldn't wrap my head around it. I think I struggled because javascript was still so new to me, and I could not seem to figure out how to get the data I needed from an API call.

This time around it's making a bit more sense, so here are some important points I picked up from the API section of this course, and a little explanation of the process of completing these exercises.  

For this exercise we needed to get the sunset time for Hawaii from a weather API. The course uses the yahoo weather API, but it has since been retired. I decided to use OpenWeather API, which I believe some of my classmates used for their API assignments back in the fall. 

To get started you need to sign up, and an API key will be generated for you. You can generate more API keys as needed. The docs explain how to make an API call. I decided to make my API call using the city ID, and all city IDs can be found in a downloadable JSON file.  I chose Lahaina, Maui.  The format for calling the API by city ID is as follows:

```
api.openweathermap.org/data/2.5/weather?id={city id}&appid={your api key}
```

As you can see, you plug in the city ID and your API key.  My URL looked like this (note I have included a fake API key for privacy):

```
https://api.openweathermap.org/data/2.5/weather?id=5849996&appid=12345abcde
```

My course used the "request" package to handle requests in Node.js, however that package has since been deprecated. I used it for the exercise because it still works, though for a real project I would try to find something that is currently being updated and maintained. 

To test if you're receiving data, the code looked like this:

```js
request("https://api.openweathermap.org/data/2.5/weather?id=5849996&appid=12345abcde", function(error, response, body){
    if(!error && response.statusCode === 200){
        console.log(body);
    }
});
```

However, the data you get back in the console is actually a string. We can prove this by printing "typeof body" to the console. We need to access our data from a javascript object, not a string. All you need to do is use "JSON.parse(body)":

```js
request("https://api.openweathermap.org/data/2.5/weather?id=5849996&appid=12345abcde", function(error, response, body){
    if(!error && response.statusCode === 200){
        var data = JSON.parse(body);
        console.log(data);
    }
});
```

Now, to get my sunset data. To know the structure of the JSON object I made the API request in the browser. In chrome I have an extension called JSON Viewer, which makes the JSON easier to read. This was the part of the JSON data I needed, to know how my sunset time was nested:

```js
"sys": {
    "type": 1,
    "id": 7875,
    "country": "US",
    "sunrise": 1590335183,
    "sunset": 1590382873
  },
```

So I just need to get the data, in this case using the square bracket notation []: 

```js
request("https://api.openweathermap.org/data/2.5/weather?id=5849996&appid=12345abcde", function(error, response, body){
    if(!error && response.statusCode === 200){
        var data = JSON.parse(body);
        var sunset = data["sys"]["sunset"];

        console.log(sunset);

    }
});
```

However, this just prints a long string of numbers, not a date. Checking the docs I can see that, when using a JSON object, it gives the time in unix format, which is basically the number of seconds that have passed since 00:00:00 UTC on 1 January 1970. The simplest seeming suggestion to parse this format on stack overflow was to use Moment.js.  

Once I figured out how to get an hour:minute format, I printed it to the console. But the time didn't look right. If I am looking for the sunset in Lahaina, in May, it most likely wouldn't be 22:58 - nearly 11pm! -  which is what my program told me. When I looked it up, it was in fact 18:59. This four hour difference is actually the time difference between Lahaina and my own timezone, MDT. Somehow I am getting the time that the sun sets in Lahaina, in MDT.  I had to dig in a bit more to find out why this was happening. Is it coming from my API, or from Moment.js?

I ended up spending a few days away from this exercise, and came back with a fresher mind. After a couple searches I found that you need to include the city's timezone hardcoded. Stack overflow suggested a method called utcOffset(). Hawaii's timezone is GMT-10, so I plugged that into the method. You can also see how unix time was handled:

```js
request("https://api.openweathermap.org/data/2.5/weather?id=5849996&appid=12345abcde", function(error, response, body){
    if(!error && response.statusCode === 200){
        var data = JSON.parse(body);
        var sunsetUnix = data["sys"]["sunset"];
        
        var sunset = moment.unix(sunsetUnix).utcOffset(-10);
        
        console.log(sunset.format("HH:mm"));
    }
});

```

Finally, this gave me the correct sunset time. 

I was glad I managed to figure this out, and that it wasn't as difficult as I was expecting. I was almost ready to just move on with my course not having figured it out. Maybe it wouldn't have mattered since this timezone issue doesn't really have much to do with APIs. However, sticking with it until it was solved reminded me of two things:

- the importance of stepping away from the code when you are stuck; it gives your mind a rest and allows your subconscious to continue looking for creative ways to solve the problem
- that (at least for my own personal projects) I can solve any problem if I set my mind to it



