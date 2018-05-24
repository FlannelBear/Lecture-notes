# "Express Get" Notes

## Other Notes

- Be careful with updates, such as mac updates, node updates, etc.

## General Set up

- Git init *Initialized a git repo (the order in which you git init and npm init doesn't really matter)*
- Git remote add (url) *Connects local repo to github repo*
- npm init *Initialized a Node Package Manager and creates a package.json file*
- create .gitignore file containing ".DS_Store" and "node-modules" *excludes the listed files from all git commits*
- npm install express *installs the express "module" to your server, automatically lists it in your package.json.  When we start from scratch we must specify the modules we want to install, in this case we want express*
- create "server" folder
- put server.js file in the "server" folder
- nest a "public" folder inside the "server" folder
- create an index.html page inside the "public" folder

### Folder tree example
```
Server Folder{
    Public Folder{
        Index.html
    }
    Server.js
}
```   

## Server Set up

### Raw Code

```
const express = require('express');
const app = express();

app.use( express.static('server/public'));

const port = 5000;

app.list(port, () => {
    console.log('Listening to port', port);
});
```
### Code breakdown

- Require in Express
``` 
const express = require('express');
```
- Create the app with express
```
const app = express();
```
*the module "express" can be called as a function to "create" your application*

- Tell the app to use the "public" folder for handling requests.  The public folder will contain everything we want the client side to show, and everything we want the client to have access to.
```
app.use( express.static('server/public'));
```
*This line of code tells the app to use the public folder for handling client side requests.  This means that the client can only access what is in the public folder*

- Create a "port"
```
const port = 5000;
```
*Sets the port equal to 5000*

- Tell the app to listen to the port
```
app.listen(port, () => {
    console.log('Listening to port', port);
})
```
*This logs to the console what port our server is currently listening to and is helpful for determining what to type into our browser to find out locally hosted server.  In this case we should go to 'localhost:5000' to see our project.*

## What the hell does "app.get" do? and How do we write it?

### Raw Code
*Arrow functions are ES6.  If they look new, refer back to TuesdayNodeReview.pdf for notes on arrow functions*
```
app.get('/link', (req, res) => {
    console.log('Handling the GET request for /link');
    res.send('Just be yourself!');
});
```
### Code Breakdown

- What is this '/link' thing?
```
app.get('/(link)');
```
*link refers to the tail of the file path where the specific page that will be sending requests to the server and recieving responses lives.*
*If we were to replace 'link' with 'quote', then our app.get would handle all requests coming from the location 'localhost:5000/quote' (as long as we are locally hosting the server on the port 5000)*

- What is (req, res)?
```
app.get('/link', (req, res) => {
    // Code Block
});
```
*req is the "request" that the server is receiving from the webpage specified as /link.  res is the "response" the server will send back to the client at /link.*
*This automatically populates so we don't need to worry about what they are, just what we're doing with them in the subsequent code block.*

- What can I expect from the code block?
```
app.get('/link', (req, res) => {
    console.log('Handling the GET request for /link');
    res.send('Just be yourself!');
});
```
*The string in the console.log will show up in the console.  The string in res.send() will be sent to the client.*
*This is how we can handle requests and respond with data or other types of information.*
##### Right now because we are missing some other parts to this, the browser will do some magic for us and display the string on the DOM.  Know that this is browser magic, and is not the behavior we should expect when we have all the pieces in place.

#### 404 example
```
app.get('/link', (req, res) => {
    res.sendStatus(404);
});
```
*If you were to navigate to the /link page, then the server would receive a request from you (the client), and send a response of 404, otherwise known as a 404 error message.*

- What is res.send();
```
app.get('/link', (req, res) => {
    res.send('Just be yourself!');
});
```
*res.send() is sending what is in the parens to the client.*

