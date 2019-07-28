# Create a REST API : Project Setup with Express

In this tutorial we will create a new NodeJS project with express, body-parser and nodemon and get our server.js file ready to build a REST API. This is part 1 of a series of tutorials on building a REST API in Node with express and PostgreSQL.

Part 1 - Project Setup with Express
Part 2 - PostgreSQL with KnexJS Setup
Part 3 - User Registration and Validation
Part 4 - Send Emails with Amazon SES
Part 5 - Verify Users with Tokens

## Required

Before installing, download and install Node.js. Node.js 0.10 or higher is required.

## Project Setup
1. Create a new node project
Create a new node project (We called ours 'simple-api' but you can call yours anything you'd like):

## From your command line / terminal

```
mkdir simple-api
cd simple-api
npm init
```
npm will ask you a series of unimportant questions about naming your package and version numbers. Just hit 'Enter' until you get to `'entry point'` and type in `server.js` instead of the default `index.js`. Create a `server.js file` now in your projects root directory.

## 2. Install some packages
For this part of the tutorial series we will need express, body-parser and nodemon.

Express - A web framework for Node.js which makes working on web applications easier by providing a nice coder-friendly layer on the Node.js HTTP server. Check out the express website for more details.

body-parser - Middleware for Express that extracts the body of an incoming request stream into a more usable form, `req.body`. You can read more about it here

nodemon - Automatically restarts node applications when changes are made to a directory. Read more about it here .

Use the following command to install these express and body-parser:

```
npm i express body-parser
```
***Tip*** : As of version 5.0.0 of npm, you don't need to add ***--save*** at the end of an npm install command to have packages added to your package.json file. npm does this automatically now!

and install nodemon globally so you can use it as a command line tool:
```
npm i nodemon -g
```
## 3. Don't forget Git
I am assuming you are familiar with Git so this is just a friendly reminder that this would be a good time to create a `.gitignore` file (with 'node_module' and package-lock.json') and do a git commit.

## 4. Server.js
Moving on to the `server.js` file where we will customize the configuration of our project.

```
// simplecode-api/server.js
// Include express
const express = require("express");

// This line simply puts Express in a variable called 'app'
const app = express(); // Include body-parser
const bodyParser = require("body-parser");

// Include users.js file from the api directory (We will add this in the next step)
const userRoutes = require("./api/routes/users");

// Configure body-parser settings//
// urlencoded is for bodies that have UTF-8 encoding.
// If {extended: false} you cannot use nested objects.
// e.g. nested obj = {person:{name: adam}}
app.use(bodyParser.urlencoded({ extended: true }));

// Parse json with body-parser
app.use(bodyParser.json());

// Setup your api routes with express
app.use("/v1/users", userRoutes);

// Server will listen to whatever is in the environment variable 'port'
// or 3000 if nothing is specified
const port = process.env.PORT || 3000;

// express returns an HTTP server
app.listen(port, () => console.log("[Server] online " + new Date()));
```

Notes:

***app.use***

Used in express to set up the middleware layers. Various options can be added after `app.use()` i.e. ```app.use([path,] callback [,callback…])```. The usage of each option has been explicitly mentioned in the comments above each line of code.

***v1/users***

The purpose of using `/v1/` in our routes is to make versioning of the api possible without a lot of hassle. So, for version 2 of a route you could just add `/v2/users`. There are other more elegant ways to perform versioning, but for the purposes of learning how to make an api, this should work for now.

## 5. User Routes

Let's setup our users routes file. Create a directory called `api` in your project root with another directory called 'routes' inside that. Create a `users.js` file in the `routes` directory. In the `users.js` file we will include the express router middleware for our routing.
```
// simeplcode-api/api/routes/users.js
// Include express
const express = require("express");


// Include express router middleware
const router = express.Router();

// Add a 'get' method to express router for our test route
router.get("/", function(req, res) {
  res.send({ msg: "Hello World" });
});

// Exports the router object
module.exports = router;
```
If you remember, this file is loaded by server.js and inserted at the end of the `v1/users` route. So in this test route we are adding '/' to the end of our users route which will respond to any requests to `localhost:3000/v1/users/`. We'll see this in action in the next few steps

## 6. Start your server
In your projects root directory in a command line or terminal enter:
```
nodemon server.js
```
and you should see the following:
```
[nodemon] 1.18.6
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `node server.js`
[Server] online Thu Dec 08 2018 09:57:07 GMT-0500 (Eastern Standard Time
```
Looking good! Your server is up and running!

## 7. Test your server
With your server running, open up a web browser and go to `localhost:3000/v1/users` (Remember this route we create earlier?). You should see the following:
```
    {
        "msg": "Hello World"
    }
```
## 8. Testing with Postman

Before we move on to part 2 of this series, it might be useful to download and install the Postman App. Once you have postman open, select the GET method, type in `localhost:3000/v1/users/` in the address bar and click 'Send'. You should see the same “Hello World” message as you did in your browser.