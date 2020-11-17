---
layout: post
title: "Making a React/Node.js app"
date: 2020-11-16 18:30
tags: react node.js
navigation: true
class: post-template
current: post
---

When I was in school we learned about fetching data from external APIs. At the time I found a news API where you could filter articles based on topic. Since I previously went to university and earned a science degree I thought it would be great to make an app for myself that gives me the latest science articles.

I had genuinely wanted to keep up with what was going on in science. After I graduated I had a subscription to the journal Nature hoping that would help me keep up. I fell hopelessly behind as each week they sent a journal packed with primary science research articles. Even after spending four years reading these types of in-depth research articles, they often took me a long time to get through due to the detailed technical language.

I figured an app would be easier. I finally got around to making it a year after finding the news API. Since I am learning React I thought this would be a good opportunity to practice my React skills.

### What I actually did

I specifically searched for tutorials on how to fetch API data with React. I modelled what I did after [this video by Brad Traversy](https://www.youtube.com/watch?v=YaioUnMw0mo), and I also went back to previous React projects/tutorials I followed as a reference. Since I wanted to get data onto cards in a React front end this was perfect.

I created the front end with my card structure and set up React Router so I could navigate between pages in my app.

#### Node.js/Express backend

I realized that I needed to hide my API key somewhere. Even though this is a low stakes project and no one was likely going to hack my app, I wanted to do it right. First I put my key in a .env file in the front-end.

After reading some posts on StackOverflow I learned that the key is still visible in the Network tab under Chrome dev tools. And I checked - it was very easy to find. At this point I decided to create a Node.js backend to keep my key safe. I needed to do this anyway to deploy my app to Heroku later on.

[This was a very helpful tutorial](https://www.youtube.com/watch?v=kJA9rDX7azM) that taught me how to make a very simple back-end and connection to React front-end.

[You can see the my server.js code on github](https://github.com/jchack1/react-science-news-public/blob/main/server.js), but to summarize: the "request" npm package was used to actually make the request to the news API. I had to parse the json data to get to my "articles" array. I made a route called "/getArticles" that would be called by Axios in the front end.

Get route:

```js
app.get("/getArticles", (req, res) => {
  request(
    "https://gnews.io/api/v4/top-headlines?topic=science&lang=en&country=ca&token=${apiKey}",
    function (error, response, body) {
      if (!error && response.statusCode === 200) {
        const parsedBody = JSON.parse(body);
        const articles = parsedBody["articles"];

        res.send({ articles });
      } else if (error) {
        console.log(error);
      }
    }
  );
});
```

And below it is being called in the front end:

```js
const url = "/getArticles";

const [items, setItems] = useState([]);
const [isLoading, setIsLoading] = useState(true);

useEffect(() => {
  let cancel;
  axios
    .get(url, {
      cancelToken: new axios.CancelToken((c) => (cancel = c)),
    })
    .then((res) => {
      setIsLoading(false);
      setItems(res.data.articles);
    });

  return () => cancel();
}, [url]);
```

There is probably a cleaner way to do the above Axios request, but if I didn't do it this way, there would be too many calls to the news API and I would use up my daily limit in a matter of minutes. This code came from an old tutorial I followed to call data from a Pokemon API.

#### Using a proxy in package.json during development

The video I linked above goes over this, but when you are working on your local machine you will have React and Node running at the same time. In the package.json you can add a line like this (port number can be any unused port):

```json
"proxy": "http://localhost:5000"
```

And set your port to 5000 in your server.js and/or .env file. React by default starts on port 3000, so you will be able to run both projects at once while developing.

I originally kept this in my package.json when I deployed, but removed it later on. I had several bugs once I deployed and the ports may or may not be related. It's possible that Heroku ignores this during deployment, but I decided to put both projects back to port 3000 just in case this was causing problems for me.

It should also be noted that when you add your proxy into your package.json, you must restart your projects for it to take effect.

#### Scripts in package.json

I had to pay special attention to my scripts so I could start my server and my React app separately during development. I used the below scripts in package.json:

```json
"scripts": {
    "start": "node server.js",
    "react-start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

I kept the "start" script related to the server. When deploying, Heroku looks for your start script to start your Node.js server. I made a special "react-start" script which I ran with "npm run react-start".

### Other issues

The .env file was very problematic. I used this in the backend to hide my API key from being public on github. This worked fine when running locally, but the "/getArticles" route would not work once deployed on Heroku. I made a last ditch effort to get it to work and added my API key directly into the url in my server.js file. I was shocked when it worked.

I need to look into this more, but it seems like there is an issue using .env files when deploying with Heroku, or perhaps deployment in general. At least this solved my problem of the API key being revealed in the Network tab, but I can't upload my exact server.js file to github. Until I figure out a better way, my server.js file on github is modified so the API key is not shown.

Also, the original news API I had decided to use - NewsAPI.org, which gave me the idea for this project in the first place - changed their rules for the free developer plan earlier in 2020. You cannot get data if the API is being called from anything other than localhost (so it doesn't work when deployed), and the API keys expire after one month. It is just meant for developers to test out the API before paying for it, which I believe is a cost of about \$400 per month.

I had already spent a long time building the project using this API and was very disappointed when I deployed and found this out. Thankfully I found GNews.io, which I fit into my app with very little extra work. If I'm being honest, I think I actually like it better than NewsAPI.org. The data seems cleaner and there are not as many articles missing images. This data looks better on my cards so I think it worked out for the best.

### Conclusion

For now this is all I can remember. It was a difficult project but I'm happy I pushed through, because I ended up with this finished product: https://react-science-news.herokuapp.com/
