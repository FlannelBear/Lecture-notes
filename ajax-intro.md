# Introduction to Ajax

*Corresponds with Devs Video from 5/23/2018*

**Ajax get/post lecture with JQuery, Express, & Node.**

#### Other Notes:
- **Static** refers to the 'base' folder for your entire website.  When you navigate to youtube.com what you see is what is 'static'.  
- 'console.log' shows up in different places depending on where it shows up.  If it shows up in the terminal, it is server side.  If it shows up in the console, it is client side. Important for debugging.

### Set up

##### Typical folder tree template we've been using
- Server folder
- server.js inside of Server
- Public inside of Server
- scripts, index.html, vendors, etc. inside Public. *These are what the client will have access to and be able to see*

##### Initialize node package manager
- **npm init**
- **npm install express body-parser** *This will install both express and body-parser as dependencies* 
- add the "**start**" script to the package.json inside the "scripts" block. *This will provide a shortcut for launching your server*

```
"start": "node server/server.js"
```

### Server.js template
*Dev recommends laying out your server.js file this way to keep track of your server*
```
// requires
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

//globals
const port = 5000

// uses
app.use( express.static('server/public'));

// spin up server
app.listen( port, () => {
    console.log('server up on:", port);
}); // end server up

// get route
See code block below
```
##### There is a list of ports you can't use, stick with 5000 when you are practicing.
---
## Get route: sending data from the server to the client

```
app.get( '/guests', ( req, res ) => {
    console.log('in GET hit for /guests route');
    res.send('meow');
}); // end guests GET
```
*This block of code has created a 'new route' for our web app.  This block is meant to recieve a request for /guests, and then send a response specified in the block between the curlies (meow)*
*The log will show up in the console.  The meow, through **_browser magic_**, will appear on the screen.*
*You can only send one thing, so if you must send multiple sending an object, array, or class will work*

### Raw Code

```
// requires
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

//globals
const port = 5000
let guests = ['Ally', 'Dev', 'Mary'];
// uses
app.use( express.static('server/public'));

// spin up server
app.listen( port, () => {
    console.log('server up on:", port);
}); // end server up

// get route
app.get( '/guests', ( req, res ) => {
    console.log('in GET hit for /guests route');
    res.send(guests);
}); // end guests GET
```
*In this case, we would expect to see the array "guests" appear*

#### What does the code display?
```
[Ally, Dev, Mary]
```

##### *This point in the lecture Dev has created an index.html, and he has sourced in a client.js and jQuery.*

##### Lecture break
---
### Take a 5 min break if you need one. We needed one, too.
---
##### Lecture resume

# **Buckle up now, because this... is the ride**

#### Recap
- Made a server
- Made a get route
- Made the client side

### Raw code
**In client.js**
``` 
function getGuests(){
    console.log('getGuests');
    // make Ajax-get call to server to retrieve guests
    $.ajax({
        method: 'GET', 
        url: '/guest'
    }).then( function( response ){
        consoel.log('back from the server with:', response );
        displayGuests( response );
    });
} // end getGuests
```

### What is happening here?
- Ajax method of jQuery
```
$.ajax()
```
*jQuery has a method 'ajax()' that will handle server calls.*

- object containing method and url
```
$.ajax({
    method: 'GET',
    url: '/guests'
})
```
*Goes into server and makes a call to 'app.get' for the url '/guests'.  app.get in our server.js is the 'GET' method. '/guests' is the url specific to the app.get method we want to use.*

- .then()
```
.then();
```
*Once the call has been made, 'then' do this thing.*

- function(response)
```
.this(function (response) {
    console.log( 'back from the server with:', response);
    displayGuests( response );
});
```
*function specified in the '.this()' method to run once a response comes back from the server.  This function will pass the response into a console log, as well as to our displayGuests function.*

#### Reminders

**_Remember that we are working in a client.js file with a displayGuests() function for appending the guests array (which is on the server) onto the DOM_**

*At this point Dev has created an html button for refreshing the array with the id #refreshButton.  With an event listener for a click, we run getGuests to refresh the list on the DOM without refreshing the page.*

---
## POST route: sending data from the client to the server

**Dev has made an input field and an Add button on the html to take a name, and then push the name to the array on our server**

```
<input id="nameIn">
<button id="addNameButton">Add</button>
```

### Server-side: Raw code
*server.js*
```
app.post( '/guests', (req, res) => {
    console.log( 'in POST hit for /guests route:', req.body);
    res.send( 'ribbet' );
}); // end guests POST
```

### What is happening here?
- app.post();
```
app.post( '/guests', (req, res) => { STUFF } );
```
*This piece specified what to do when the server recieves a POST Ajax call from the client at /guest*

- req.body
```
console.log( 'in POST hit for /guests route:', req.body);
```
**_THIS USES BODY_PARSER_**

*in server.js '//uses'*
```
app.use( bodyParser.urlencoded( {extend: true} ));
```
*This line of code will use body-parser to give our req.body its value.  If we are lacking this line, our req.body will return as undefined.*
*This should be added into the //uses section as we are setting up our server.js, just like how we set up express.static.*

- res.send
```
res.send( 'ribbet' );
```
*This piece sends 'ribbet' back to the client to be logged to the console.*

### Client-side: Raw code
*client.js*
```
function addGuests(){
    let objectToSend = {
        name: $('#nameIn').val();
    };
    $.ajax({
        method: 'POST',
        url: '/guests',
        data: objectToSend
    }.then( function (response){
        console.log('back from server with:', response);
    }));
}
```

### What is happening here?
- objectToSend
```
let objectToSend = {
    name: $('#nameIn').val();
};
```
*This line of code creates an object with a name that is set equal to the value of our input field. HEY JQUERY!*

- $.ajax()
```
$.ajax()
```
*This piece uses the ajax method of jQuery to make a call to the server*

- method, url, data
```
$.ajax({
    method: 'POST',
    url: '/guest',
    data: objectToSend
})
```
*This piece gives the ajax method directions on how, where, and what to send to the server. The method tells ajax to use the app.post.  The url specifies which post method to use. The data specifies exactly what to send, in this case objectToSend*

- .then()
```
.then( function(response){
    STUFF
});
```
*This piece specifies what to do after the server returns a response.*

---

### Dev's conceptual AJAX talk: corresponding photo on slack

- The req and res are the only two ways to get the client and the server to communicate.
- jQuery handles the front end events in client.js.  It makes use of Ajax to make 'GET' and 'POST' calls to interact with the server.
- 'GET' is a method of ajax that goes to the server returns to the client with data to be handled client-side. (Data path: Server to Client).
- 'POST' is a method of ajax that goes to the server with data from the client to be handled server-side.
- After the ajax method runs, the .then() runs to handle the server response appropriately on the client-side.
- Again, jQuery handles the new information to update and change the DOM appropriately.

*Dev and Server - "For what route are you looking?"*
---
# So what have we accomplished today?

- We created a server to host a webpage
- We put an array onto our server
- We built the client side of our webpage using an index.html and client.js file
- We then used Ajax to make a call to our server and get back the array of names
- We used jQuery to append the names in the array to the DOM of our webpage.
- We created an input field and button that takes in more names.
- We used jQuery to handle the new names and send them back to the server.
- Finally we used the server and inputs to dynamically update the page as people added more and more names to the names array.

**We did alot! We're awesome!**

*Dev - "You've moved from making a web-page, to making a web-site."*