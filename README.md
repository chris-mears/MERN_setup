# MERN_setup

1. Set up Git repo on github and local(create on github with .gitignore file for node)
2. npm init -y
3. npm install express dotenv body-parser mongoose
4. touch server.js
5. set up server file:
```javascript
require("dotenv").config();
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const app = express();
mongoose.Promise = global.Promise;
mongoose.connect(process.env.MONGODB_URI, {useMongoClient: true}); 

const connection = mongoose.connection;
connection.on('connected', () => {
console.log('Mongoose Connected Successfully');
});

// If the connection throws an error
connection.on('error', (err) => {
console.log('Mongoose default connection error: ' + err);
});

app.use(bodyParser.json());
app.get('/', (req,res) => {
res.send('Hello world!')
})

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
console.log("Magic happening on port " + PORT);
})
```
6. Commit
7. create-react-app client
8. Update express port other than 3001
9. npm install concurrently //From root folder
10. npm run build //From client folder
11. add "client/build/" to outer .gitignore
12. add to server: 
```javascript
    app.use(express.static(__dirname + '/client/build/'));
    ...
    app.get('/', (req,res) => {
		res.sendFile(__dirname + '/client/build/index.html')
	      })
```
13. touch .env with MONGODB_URI=mongodb://localhost/(DB Name) make sure gitignore has .env in it
14. set up engines and post install, update "main" to "server.js" in package.json //In root folder
```javascript
     "engines": {
   "node": "8.9.0"
 },
 "scripts": {
    "start": "node server.js",
    "dev": "concurrently \"node server.js\" \"cd client && npm start\"",
    "test": "echo \"Error: no test specified\" && exit 1",
    "postinstall": "cd client && npm i && npm run build"
  },
```
15. deploy to heroku: 
    1. heroku create //app-name-optional-here
    2. heroku addons:create mongolab:sandbox
    3. git add -A
    4. git commit -m "commit message"
    5. git push heroku master
16.  npm i react-router-dom axios styled-components (from client folder)
17.  add proxy to client package.json //right under "private"
```javascript
"proxy": "http://localhost:3001", 
```
//3001 should be whatever port you have the express app on. Will need to restart server for proxy to work
