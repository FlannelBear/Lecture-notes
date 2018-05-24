# The Record Store Lecture
*5/24/18*

*Corresponds with Mary's recordings*

**These notes are supplemental to your code.  The goal is to keep a record of the code Mary wrote, alongside some explanations of what the new shit is and what it is doing.**

---

## General set up

1. Make a new folder for the project
2. git init (*use git status to check that you aren't accidentally creating a repo in a repo*)
3. npm init (-y *if you want to skip the set up and do it later by manually editing package.json*)
4. npm install express body-parser
5. create "start" shortcut in package.json
6. create .gitignore (.DS_Store, node_modules, etc.)

## Project set up
1. Build your folder tree
- Server folder
- Public folder
- Vendors folder
2. Create files
- index.html
- client.js
- server.js
3. Source in necessary files
- Vendors (jQuery, Bootstrap, etc.)
- client.js to index.html

*Good practice: Check the client side before moving on by console logging 'js' in the client.js, and 'jq' in the document.ready function*

## Server.js set up

### Raw code:
```
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use( express.static('server/public'));
app.use( bodyParser.urlencoded( {extended: true} ));

const port = 5000;

app.listen(port, () => console.log( 'Listening on port: ', port ) );


```

### List
1. Require dependencies
``` 
const express = require('express');
const bodyParser = require('body-parser');
```
- require in express dependency
- require in body-parser dependency

2. Use express to create our 'app'
```
const app = express();
```
- use express() to create our app

3. Define app uses
```
app.use( express.static('server/public'));
app.use( bodyParser.urlencoded( {extended: true} ));
```
- set the app to use express.static to contain client access to the public folder
- set the app to use body-parser for 'POST' ajax calls

4. Set the port
```
const port = 5000;
```
- set the port to 5000 so we can see our app on localhost:5000
5. Listen on the port
```
app.listen(port, () => console.log( 'Listening on port: ', port ) );
```
- tells the app to listen to the port and log when its running so we know our server is up

---

## Module Scripts Raw Code

### *record.class.js*
```
class Record {
    constructor( artist, albumName, year, genreList ){
        this.artist = artist;
        this.albumName = albumName;
        this.year = year;
        this.genreList = genreList;
    }
    addGenre(){
        // Make sure that there is an array
        if(this.genreList == null){
            this.genreList = [];
        }
        // TODO - make sure we don't add the same one twice
        genreList.push(string);
    } // end addGenre
} // end Record class

module.exports = Record;
```

### *server.js*
```
// Requires
const express = require('express');
const bodyParser = require('body-parser');

// Set up app
const app = express();

// Uses
app.use( express.static('server/public'));
app.use( bodyParser.urlencoded( {extended: true} ));

// Add records
const Record = require('./modules/record.class');
const recordArray = [
    new Record('Beatles', 'Abbey Road', 1969, ['Rock']),
    new Record('Michael Jackson', 'Off the Wall', 1979, ['Pop']),
    new Record('Prince', 'Purple Rain', 1984, ['Pop']),
    new Record('Cibo Matto', 'Viva la Woman', 1990, ['Jpop'])
];

// GET
app.get('/record', (req, res) => {
    console.log('Handling my GET for /record');
    res.send(recordArray);
    });

// POST
app.post('/record', (req, res) => {
    console.log('Handling my POST for /record', req.body);
    let sentRecord = req.body
    let record = new Record(sentRecord.artist, sentRecord.album, sentRecord.year, sentRecord.genre);
    recordArray.push(record);
    res.sendStatus(201);
});

// Set port
const port = 5000;

// Listen
app.listen(port, () => console.log( 'Listening on port: ', port ) );
```

### *client.js*
```
console.log('js');

$(document).ready( readyNow );

function readyNow(){
    console.log('Client side Woot!');
    getAllRecords();
}

function getAllRecords(){
    $.ajax({
        method: 'GET',
        url: '/record'
    }).then( function(response) {
        displayAllRecords(response);
    });
}

function addRecord(record){
    $.ajax({
        method: 'POST',
        url: './record',
        data: record
    }).then(function(response){
        // clear input fields
        getAllRecords();
    }).catch(function(error) {
        console.log("Something bad happened: ", error);
    });
}

function displayAllRecords(recordArray){
    let $recordsTarget = $('#records);
    $recordsTarget.empty();
    for( let record of recordArray ){
        $recordsTarget.append(makeRowForRecord(record));
    }
}

function makeRowForRecord(record){
    let rowHtml = `<tr>
        <td>${record.artist}</td>
        <td>${record.albumName}</td>
        <td>${record.year}</td>
        <td>${ record.genreList.join(', ') }</td>
        </tr>`;
    return rowHtml;
}
```
*HTML not included.  P.S. tables suck.*
*We added Bootstrap to make it pretty*

##### Mary's zen teachings
1. Magic numbers: Make a const for numbers that you will use later so that they are descriptive in the rest of your code, and other programmers looking at your code can make sense of it.  (Opinions on this varies).
2. Naming conventions: 
- constant variables should use Snake_Case.  
- If setting a variable equal to a jQuery value, put a $ (bling) at the beginning.

##### Dev quotes
- "When in doubt, control C out."

### New shit to walkthrough
- .catch();
```
$.ajax(STUFF).then(STUFF).catch(function(error){
    console.log("Something bad happened: ", error);
});
```
*The .catch() method takes a function (which takes an 'error' as a parameter) as a parameter, and then runs the function.  This is included for handling events when the server sends the client an error in response to a request so the client knows what's up.*

- The whole sentRecord thing
```

```
- res.sendStatus(201) for POST calls
```
res.sendStatus(201);
```
*When we make a POST call we need to send a response back to the client to prevent it from waiting around.  The status code 201 means 'created', so res.sendStatus(201) responds to the client by saying 'I made the thing!'.*
### Old shit that's still confusing
- app.get
- app.post
- $.ajax: method post
- $.ajax: method get
- body-parser