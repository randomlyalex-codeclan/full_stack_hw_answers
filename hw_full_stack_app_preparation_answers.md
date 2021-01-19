# Homework: Full Stack Games Hub App / Answers

For me to first understand i've written below what happens when a user adds a game.  I'm struggling to put this into a diagram however, probably because (as usual) i've gone into too much detail, sorry? lol

**In the cosideration of addGame:**
Front end server via Webpack (?), serves the page to the client.  Ie the form.
User interacts with the website, ie a form.
The event (could be a button but in this case the form submit) triggers a method, that in this case takes the data from the form (via Vue bindings not traditional/legacy way). Puts the data into an object called "game" in our example and calls the GamesService ()which is a vanilla Javascript file) as a promise (?) 

GameService is tied to the backend api server with the baseURL set as a constant.

postGame (called by the addGame method) with a HTTP POST request, on the baseURL, sending the object over http as a "stringified" message.  stringify takes a true javascript object and create a json string, the exact opposite of how .json() takes the json string and creates object data for us.

The server side / back-end client running express.js deals with this POST http listening on port 3000 and app.use is binding /api/games with the Router gamesRouter.  In this case takes the Post Req at /api/games gets routed, and the server performs an InsertOne with the json payload on the connected database client.  To which it has a connection established in the server.js (using the MongoDB driver that has been imported) under localhost port 27017. Note again none of the three clients need to be local in these cases.  The database responds to the InsertOne command appropriately, replies with success/failure, and this in turn is passed back as a response to the backend client express.js and this response is passed back to GameServices, which takes that and emits it to the purblic eventBus under 'game-added', so the other component(s) just the one in our case get the reply and update accordingly.  Notably GamesGrid.vue will show the newly updated list of Games by pushing the reply onto the games array.

## Answers

1. What is responsible for defining the routes of the games resource?
   **node package express.js in our case but generally the back-end server side of things**

2. What do you notice about the folder structure?  Whats the client responsible for? Whats the server responsible for?
   
   **The client is responsible for the user input and interaction, and the connection to the back-end via the back-end API sending requests and listening for responses.**
   
   **The server is reponsible for listening and replying on the API, routing and a connection and queries to the database****

3. What are the the responsibilities of server.js?
   
   **Routing, dealing with requests to the right places, and querying the DB which is on another connection.**

4. What are the responsibilities of the `gamesRouter`?
   
   **gamesRouter is the code responsible for the routing of requests to /api/games**

5. What process does the the client (front-end) use to communicate with the server?
   **via an API an example can be seen in GamesService.js where it is directly using 'http://localhost:3000/api/games/'**

6. What optional second argument does the `fetch` method take? And what is it used for in this application? Hint: See [Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) on the MDN docs
   
   **By default a fetch is GET, but you can specifiy other methods, in the form of an object, including telling the recipient what data to expect, and what will be in the body.  I find calling this fetch() in this case counter productive.... But i can't write a whole language or framework!**

7. Which of the games API routes does the front-end application consume (i.e. make requests to)?
   
   **I can only see /api/games/**

8. What are we using the [MongoDB Driver](http://mongodb.github.io/node-mongodb-native/) for?:
   
   **This is establishing a connection and communication between the database (MongoDB in our case) and the server back end client. **

## Extension

Why do we need to use [`ObjectId`](https://mongodb.github.io/node-mongodb-native/api-bson-generated/objectid.html) from the MongoDB driver?

**My understand is that it is the conversion between the built in id operator within Javascript to the _id operator that is used within MongoDB?  So when something is loaded into an object from MongoDB into memory as an obect, the _.id attribute is populated not directly, but converted into a native id of javascript that can then be used to reference on delete or update etc.  **
