---
tags: [Courses]
title: 'Session 1: Node JS Modules'
created: '2023-02-16T13:23:33.228Z'
modified: '2023-02-16T13:27:49.832Z'
---

# Session 1: Node JS Modules

Node.js provides a module system that allows you to organize your code into reusable pieces. In this session, we'll cover the basics of Node.js modules and how to use them in your code.

## Introduction to Node.js modules

Node.js modules are similar to JavaScript modules, but with some additional features. A Node.js module is a file that contains code that you can use in your Node.js application. Each module has its own scope, so the variables and functions in one module are not accessible in another module unless they are explicitly exported.

Let's take a look at a simple example. Suppose we have two modules, math.js and main.js. math.js exports a function that adds two numbers, and main.js uses that function to add two numbers.

```js
// math.js
function add(a, b) {
  return a + b;
}

module.exports = { add };

// main.js
const math = require('./math.js');

const result = math.add(1, 2);
console.log(result); // Output: 3

```
In this example, we define a function add in math.js, and then we use the module.exports object to make that function available to other modules. In main.js, we use the require function to load the math.js module and then call the add function.

## Built-in Core modules and local modules
Node.js comes with several built-in core modules that you can use in your application without installing any additional packages. Some examples of core modules include the fs module for working with the file system and the http module for creating HTTP servers.

In addition to core modules, you can also create your own local modules. A local module is a file that you create in your project, and then you can use the require function to load that module in other parts of your application.

Let's take a look at an example that uses the fs core module to read a file.

```js
// fileReader.js
const fs = require('fs');

function readFile(path) {
  return fs.readFileSync(path, 'utf8');
}

module.exports = { readFile };

```

In this example, we define a function readFile that uses the fs module to read a file from disk. We then use module.exports to make that function available to other parts of our application.


## How to use modules.exports to share data and functionality between modules
As we saw in the previous examples, we can use the module.exports object to make functions and other data available to other modules. You can export any valid JavaScript object from a module, including functions, objects, and primitive values.

Let's take a look at an example that exports an object with several methods.
```js
// user.js
function createUser(name, email) {
  return { name, email };
}

function getUserInfo(user) {
  return `Name: ${user.name}, Email: ${user.email}`;
}

module.exports = { createUser, getUserInfo };

```
In this example, we define two functions, createUser and getUserInfo, that are both exported using the module.exports object. Other modules can then use these functions to create and manipulate user objects.

Let's continue with some examples of how to use the module.exports to share data and functionality between modules in Node.js:
```js
// greet.js
const greet = (name) => {
  console.log(`Hello, ${name}!`);
}

module.exports = greet;
```
In the above code, we define a greet function that takes a name parameter and logs a greeting to the console. We then use module.exports to export this function so that other modules can use it.

```js
// index.js
const greet = require('./greet');

greet('John');

```

In the above code, we use require to import the greet function from the greet.js module. We can then use this function in our code to greet a user.

Node.js also provides built-in core modules that you can use in your applications without needing to install any external packages. Here is an example of how to use the http module to create a simple HTTP server:

```js
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
  res.write('Hello, World!');
  res.end();
});

server.listen(8080, () => {
  console.log('Server started on port 8080');
});

```
In the above code, we use require to import the http module. We then use the createServer method to create a new HTTP server that listens on port 8080. When a request is received, the server responds with the message "Hello, World!".

In summary, Node.js modules allow you to organize your code into reusable components that can be shared between different parts of your application. By using module.exports to expose functions and data, you can easily import and use these modules in other parts of your code.
