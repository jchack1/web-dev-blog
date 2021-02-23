---
layout: post
title: "MERN stack - flashcard app"
date: 2021-02-22 18:30
tags: react node.js mongodb mongoose database
navigation: true
class: post-template
current: post
---

My latest project was a full stack MERN app.

I am pretty confident when I am doing front end work but I know I need more practice with back end, especially databases. At work I use AWS for all backend services, particularly Lambda and DynamoDB. For personal projects I like to use Node.js, Express, and MongoDB, as this was what I began learning in school, and I believe these are still good skills to practice.

I came across old flashcards I made while working on my science degree, so I decided to make a flashcard app.

### Backend

The first thing I decided to do was configure my database. Each type of flashcard would be given its own category. I created a model and route file for each category, and kept them in their respective folders. I created a mongoose schema and required it in the route file. I also included an npm package to find a random item from the collection, so in each category I can get a random flashcard on the front end.

```js
//model file
const mongoose = require("mongoose");
const Schema = mongoose.Schema;
const random = require("mongoose-simple-random");

const AminoAcidSchema = new Schema({
  name: String,
  code: String,
  symbol: String,
  group: String,
});

AminoAcidSchema.plugin(random);

const AminoAcid = mongoose.model("AminoAcid", AminoAcidSchema);

module.exports = AminoAcid;
```

Here I created my routes. I included a get random route, as well as a post route so I can post records to my database with postman.

I preferred to use the post route, rather than add my data directly into MongoDB Atlas, because using my app ensures I am posting to the correct collections. When I post through my app, my collections are created automatically based off the names I give my models, so the app knows to post to these collections.

I could also do all of this in the command line, but in this case my preference is to use a GUI. Either way works fine.

```js
//route file
const express = require("express");
const router = express.Router();

//getting model, needed to make queries
const AminoAcid = require("../models/aminoAcid");

//get random amino acid
router.get("/random", (req, res) => {
  AminoAcid.findOneRandom(function (err, result) {
    if (err) {
      console.log(err);
    }
    res.send(result);
  });
});

//used for testing route
router.get("/", (req, res) => {
  AminoAcid.find({ name: req.body.name }).then((aminoAcid) =>
    res.json(aminoAcid)
  );
});

//for posting amino acid data to db
router.post("/", (req, res) => {
  const newAA = new AminoAcid({
    name: req.body.name,
    code: req.body.code,
    symbol: req.body.symbol,
    group: req.body.group,
  });
  newAA.save().then((item) => res.json(item));
});

module.exports = router;
```

I then required the routes in my server.js file. The routes above could have gone into the server.js file directly, however I would have ended up with a dozen or more routes. Keeping them in a separate routes folder keeps my files more clean and organized.

```js
//server.js
// I required the routes at the top of my file
const aminoAcids = require("./routes/aminoAcids");
const functionalGroups = require("./routes/functionalGroups");
const physics = require("./routes/physics");
const biology = require("./routes/biology");

//and later on in the file I use them and assign their url
app.use("/aminoacids", aminoAcids);
app.use("/functionalgroups", functionalGroups);
app.use("/physics", physics);
app.use("/biology", biology);
```

I used postman to test my routes and database connection. I was able to get and post items into the correct collections. Even in my post route I ask the database to return the data we just posted, as a way of confirming success. I could also check in my MongoDB Atlas account to ensure they are there.

### Front end

Once again I am using react on the front end. I created a separate client folder and used create-react-app to initialize the front end app.

There are two main UIs - a home page displaying all the categories in a tile format, and a flashcard page. Each flashcard is a little different so they each get their own component, which is given its own route with react router.

The router looks like this, and the components are imported at the top of the file:

```js
//inside the App.js file
<Router>
  <div className="container justify-center">
    <Switch>
      <Route exact path="/physics" render={() => <PhysicsFlashcard />} />

      <Route exact path="/biology" render={() => <BiologyFlashcard />} />
      <Route
        exact
        path="/functionalgroups"
        render={() => <FunctionalGroupsFlashcard />}
      />
      <Route exact path="/aminoacids" render={() => <AminoAcidFlashcard />} />
      <Route exact path="/" render={() => <HomePage />} />
    </Switch>
  </div>
</Router>
```

Each flashcard component has its own state, managed by the UseState react hook. Each "flashcard" has two sides, so I keep a variable called "flipped" to store the current "side". There is also an item variable, to which we will assign a value from our API call.

In this app I made sure to have a loading variable as well, as it is good practice to show a loading state when fetching data.

```js
const [item, setItem] = useState([]);
const [isLoading, setIsLoading] = useState(true);
const [flipped, setFlipped] = useState(false);
```

### Getting data

To get the random item from the database I make an API call to the back end with axios and the UseEffect hook. A get request is made to the "random" route for the current category. The setItem function from my hook is used here to set the item variable to the data we just fetched.

```js
const url = "/aminoacids/random";

useEffect(() => {
  let cancel;
  axios
    .get(url, {
      cancelToken: new axios.CancelToken((c) => (cancel = c)),
    })
    .then((res) => {
      setIsLoading(false);
      setItem(res.data);
    });

  return () => cancel();
}, [url]);
```

There is a next button on the flipped side of the flashcard. When clicked, we get another random item from the database. The card is then "flipped over", so the user sees the front end of the card, rather than the answer, for the next flashcard.

```js
const getAnother = () => {
  let cancel;

  axios
    .get(url, {
      cancelToken: new axios.CancelToken((c) => (cancel = c)),
    })
    .then((res) => {
      setIsLoading(false);
      setItem(res.data);
    });
  setFlipped(!flipped); //reset the card
  return () => cancel();
};
```

#### Images

Ideally I would have liked to store the images - which I created in Adobe Illustrator - in my database. I researched this but it seemed overly complicated for what I was trying to do. It seemed simpler to store my images in an assets folder on the front end.

Image names match the name of the item in the database. I used a template literal so I can use the fetched item's name to get the corresponding image from my assets folder.

```js
<img src={`/assets/functionalGroupImg/${item.name}.svg`} alt="molecule" />
```

Getting images from an assets folder was a bit challenging. I struggled to get the path correct. After a bit of research I found that if you keep your assets in the "public" folder, all you need to do is begin your path with "/assets" and react will find what you're looking for.

If I needed only one static image, I could just import it at the top of my file. It would have been easier for me to deal with the paths in this case, as you type the relative path, rather than relying on react to figure out the path for you.

### Styling

I have been using styled-components and tailwind at work, so I used them in this app for extra practice. Tailwind gives a nice clean, consistent feel. I also liked including styled-components as it gave me more control over my styling, and lets me create and style my components directly in the file where I am using them without needing to rely on separate stylesheets.

It goes against everything I learned in school about "separation of concerns", but I can appreciate its usefulness. I'm sure a more in-depth discussion can be had on this topic, but that's for another time.

### Other issues and conclusions

As expected, the database/back end work was more difficult for me than the front end.

Getting a random item from the database was difficult without the npm package I ended up using. I always feel like there should be simple solutions to these <em>seemingly</em> simple problems, so I'm happy when I can find an easy solution rather than re-inventing the wheel. Sometimes stack overflow contributors can make things overly complicated.

It was difficult for me to properly get and post items to the database in my routes, as I am still learning the syntax of mongodb. In the future I would like to take some time to learn mongodb in more depth through a tutorial/course, and make a full CRUD app where I am also updating and deleting items.

Deployment was also more challenging than I expected, even though deployments to Heroku never go according to plan for me. I had never created an app before where I had a separate client folder to get my static pages from, where I was also using a database. There were issues with typos in my server.js file and my package.json. In the end I used the following:

```js
//server.js code
//for heroku deployment
app.use(express.static(__dirname));
app.use(express.static(path.join(__dirname, "client/build")));
app.get("/*", function (req, res) {
  res.sendFile(path.join(__dirname, "client/build", "index.html"));
});
```

\_\_dirname is where you currently are in your app - in this case, the server.js file. The path you create to get your static files is relative to where you currently are.

Also important, I did not have "path" installed in my project, so even if my path was correct, it wasn't seen by the app. I made sure to require this in my server.js and install in my project.

I made sure to save it in my package.json, and update my scripts for deployment. Heroku needs a start script to start your server. For react, it also needs a build script. I had to make sure to change directories into my client folder to properly create a react build. Using a command called "heroku-postbuild" was helpful here.

```js
{
  "name": "mernflashcards",
  "version": "1.0.0",
  "description": "science flashcard app",
  "main": "server.js",
  "engines": {
    "node": "12.13.0"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "npm install && node server.js",
    "heroku-postbuild": "cd client && npm install && npm run build"
  },
  "author": "julia h",
  "license": "ISC",
  "proxy": "http://localhost:5000",
  "dependencies": {
    "body-parser": "^1.19.0",
    "dotenv": "^8.2.0",
    "express": "^4.17.1",
    "mongodb": "^3.6.3",
    "mongoose": "^5.11.11",
    "mongoose-simple-random": "^0.4.1",
    "path": "^0.12.7"
  }
}
```

Once again I had issues with my .env file. I stored my mongodb connection string here, so it worked great locally, but not when deployed. I still don't fully understand how to solve this issue. It didn't cause any problems in a similar app I made in school, only recently. I believe there is a way to set up environment variables in Heroku itself. For now I added my URI in a config file, referenced by server.js, which I did not upload to github.

Overall I had fun with this project. It helped me practice database skills, tailwind, and styled-components, and I got to revisit some science concepts from university while creating the flashcard graphics and questions.

You can view the app [here](https://react-science-flashcards.herokuapp.com/)
