# Wednesday: Tuesday Node HW Review

## Setting up with Node

- Step 1: "npm init" (initializes new node package in the cd and runs through guided package.json build).
- Step *1a*: "npm init -y" (skips guided build of package.json)
- Step 2: "npm install express" (--save (optional)) (installs express from the web into the project).
- Step *2a*: "npm install" (installs dependencies from the package.json).
- Step *2b*: "--save" (tells node to automatically save installed package to the package.json (older versions of Node)).
- Step 3: Create a ".gitignore" file and ignore "node-modules" folder.
- Step 4: "git init" (initialize local git repo).

##### Good Practice: When pulling an existing project from a remote repo down to your local computer, first run "npm install".  Then check the package.json and "npm install _____" anything that you need that isn't already listed there.

## How will the Client side and Server side communicate?

- The server side will all be connected using "module.exports" and require("*the module*").
- Client side will all be connected through linking and sourcing files into the "index.html".
- The server and client will communicate through requests and responses, *NOT BY LINKING!*

# Coding Example

## First Example ES5

### person.js
```
function PersonES5(name){
    this.name = name;
    this.age = 0;

    setInterval(function() {
        this.age++;
        console.log(this.age);
    }, 1000); *function that runs another function based on a time interval (in this example it will run the function every 1000ms (milliseconds))* 
}

module.exports = PersonES5;
```
*Our person.js file will export the PersonES5 function when required by our server.js*


### server.js
```
const PersonES5 = require('./person'); 
```
*Now the variable PersonES5 contains the value of the export from person.js, which is a function.  So PersonES5 in server.js is a function.*
```
let mary = PersonES5('mary');
```
*Our variable 'mary' in server.js is an object built using the PersonES5 function from person.js*
## Corrected Example ES5

### person.js
```
function PersonES5(name){
    this.name = name;
    this.age = 0;
    self = this;

    setInterval(function() {
        self.age++;
        console.log('What is this?', self);
    }, 1000); *function that runs another function based on a time interval (in this example it will run the function every 1000ms (milliseconds))* 
}

module.exports = PersonES5;
```
*Our person.js file will export the PersonES5 function when required by our server.js*

### server.js
```
const PersonES5 = require('./person'); 
```
*Now the variable PersonES5 contains the value of the export from person.js, which is a function.  So PersonES5 in server.js is a function.*
```
let mary = PersonES5('mary');
```
*Our variable 'mary' in server.js is an object built using the PersonES5 function from person.js*

##### What's going on here? By setting 'self' equal to 'this' we have created a variable in the function block scope that is equal to 'this' meaning the object the function creates.  Then when we reference 'self' in the setInterval scope, it references the object in the PersonES5 block scope.

## This is ES5 not ES6.

~Now for ES6~

## Coding Example ES6

### person.js
```
function PersonES6(name){
    this.name = name;
    this.age = 0;

    setInterval( () => {
        this.age++;
        console.log('What is this?', this);
    }, 1000); *function that runs another function based on a time interval (in this example it will run the function every 1000ms (milliseconds))* 
}

module.exports = PersonES6;
```
*Our person.js file will export the PersonES6 function when required by our server.js*

### server.js
```
const PersonES6 = require('./person'); 
```
*Now the variable PersonES6 contains the value of the export from person.js, which is a function.  So PersonES6 in server.js is a function.*
```
let mary = PersonES6('mary');
```
*Our variable 'mary' in server.js is an object built using the PersonES5 function from person.js*

##### Now this references the person, and not the function it is nested in.  The arrow function preserves the lexical context of "this".  Meaning, it preserves the value of something as what it was where you wrote it.

### Main Takeaway

- Keep in mind the version of javascript that you are working with when you bring in libraries.  
- If they are written in ES5, don't use arrows but instead *hoist* this using 'self'.
- If they are written in ES6, use arrow functions to preserve the lexical context of 'this'.

## Traditional Function

### script
```
let myFunction = function (string) {
    console.log(string);
}

myFunction('hello');
```
*With traditional functions we need the function keyword*

### console
```
hello
```

## Arrow Function

### script
```
let myFunction = (string) => {
    console.log(string);
}

myFunction('hello');
```
*With arrow functions we remove the function keyword and place '=>' between the parens and the curlies*
### console
```
hello
```

#### Our function is one line so we can write it differently

```
let myFunction = string => console.log(string);
```
*We can do this ONLY because it is ONE LINE! If you can't write it in one line, use the arrow function example above for reference.*