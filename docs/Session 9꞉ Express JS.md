---
tags: [Courses]
title: 'Session 9: Express JS'
created: '2023-02-16T16:51:37.456Z'
modified: '2023-02-16T17:20:57.662Z'
---

# Session 9: Express JS

Express.js is a popular web framework for Node.js, used for building web applications and APIs. In this session, we'll cover the following topics:

1. Setting up an Express.js application
- Installing Express.js
- Creating an Express.js application
- Configuring the application
- Working with routes
2. Creating routes and Handling HTTP requests
3. What is middleware?
4. Validation with Ajv?
5. What is Eslint?




## Setting up an Express.js application

### Installing Express.js
To install Express.js, you first need to have Node.js installed on your computer. Once you have Node.js installed, you can use the npm package manager to install Express.js.

**Here's how to install Express.js using npm:**

1. Open your command line interface (CLI) or terminal.
2. Navigate to your project directory where you want to install Express.js.
3. Run the following command to initialize a new Node.js project:
```bash
npm init
```
4. Answer the prompts to set up your project, or use the default settings.
5. Run the following command to install Express.js:
```bash
npm install express
```

6. Once the installation is complete, you can start using Express.js in your project.
Here's an example of a simple Express.js application that responds with "Hello, World!" when you visit the root URL:
```js
// require the Express module
const express = require('express');

// create a new Express application
const app = express();

// define a route for the root URL
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// start the server on port 3000
app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```

To run this application, save the code in a file called `app.js` and run the following command in your CLI or terminal:
```bash
node app.js
```
Then visit `http://localhost:3000` in your web browser to see the "Hello, World!" message.

### Creating an Express.js application
To create an Express.js application, you can follow these steps:
1. Create a new directory for your application and navigate to it in your terminal.
2. Initialize a new Node.js project by running npm init and following the prompts.
3. Install the Express.js package by running npm install express.
4. Create a new JavaScript file for your application, such as app.js or server.js.
5. In your new JavaScript file, require the Express.js module and create a new instance of the Express application:
```js
const express = require('express');
const app = express();
```
6. Define some routes for your application using the `app` object. For example, you might define a route for the root URL that sends a "Hello, World!" message:

```js
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
```
7. Start the server and listen for incoming requests:

```js
app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```
**Here's an example of a simple Express.js application that responds with "Hello, World!" when you visit the root URL:**
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```
Save this code in a file called `app.js`, then run the command `node app.js` to start the server. You can then visit `http://localhost:3000` in your web browser to see the "Hello, World!" message.

### Configuring the application

Configuring an Express.js application for different environments, such as development, production, or testing, is important to ensure that your application behaves correctly in different scenarios. Here are some examples of how to configure an Express.js application for different environments:

1. Setting up environment variables:
Environment variables are a way to pass configuration information to your application without hardcoding it in your code. You can set environment variables in your system or in a `.env` file. Here's an example of how to set up environment variables for a development environment:
```js
const express = require('express');
const app = express();

if (process.env.NODE_ENV !== 'production') {
  require('dotenv').config();
}

const PORT = process.env.PORT || 3000;
const DB_URL = process.env.DB_URL || 'mongodb://localhost/myapp_dev';
```
2. Using different configurations for different environments:

You can create different configuration files for different environments and load the correct configuration based on the environment. Here's an example of how to set up configuration files for a development and production environment:
```js
// config/development.js
module.exports = {
  dbUrl: 'mongodb://localhost/myapp_dev',
  logLevel: 'debug',
};

// config/production.js
module.exports = {
  dbUrl: 'mongodb://localhost/myapp_prod',
  logLevel: 'info',
};

```
Then, in your main application file, you can load the configuration based on the environment:

```js
const express = require('express');
const app = express();

const env = process.env.NODE_ENV || 'development';
const config = require(`./config/${env}`);

console.log(`Using ${env} configuration`);
console.log(config);

```
This will output the correct configuration based on the environment.


### Working with routes

Working with routes is an essential part of building web applications with Express.js. Routes are used to define how your application responds to client requests for specific URLs. In this section, we'll cover how to work with routes in Express.js, including defining routes, using route parameters, and handling errors.

#### Defining Routes
To define a route in Express.js, you use the `app.METHOD()` function, where METHOD is an HTTP request method such as `GET`, `POST`, `PUT`, `DELETE`, etc. The function takes two arguments: the first is the route path, and the second is a callback function that is called when a client makes a request to that route.

Here's an example of defining a route that responds to `GET` requests to the root `URL`:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello World!');
});

```
In this example, the callback function sends the response "Hello World!" to the client when a `GET` request is made to the root URL.

#### Using Route Parameters
Route parameters allow you to capture dynamic segments of a URL and use them in your code. To define a route with a parameter, you include a placeholder in the route path, indicated by a colon followed by a name.

**Here's an example of defining a route that accepts a parameter:**

```js
app.get('/users/:userId', (req, res) => {
  const userId = req.params.userId;
  res.send(`User ID: ${userId}`);
});
```

In this example, the `:userId` placeholder in the route path captures the user ID from the URL, which is then accessed using the req.params object in the callback function.

#### Handling Errors
Express.js provides a way to handle errors using middleware functions. Middleware functions are functions that have access to the `req`, `res`, and `next` objects, and can be used to modify the request and response objects, or to perform other tasks such as logging.

To handle errors in Express.js, you can define an error-handling middleware function using the `app.use()` function with four arguments. The first argument is an error object, the second is the request object, the third is the response object, and the fourth is the next function.

**Here's an example of an error-handling middleware function:**

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

```

In this example, the middleware function logs the error to the console and sends a 500 status code and a message to the client.

#### Conclusion
In this section, we covered how to work with routes in Express.js, including defining routes, using route parameters, and handling errors. Understanding how to work with routes is essential to building robust web applications with Express.js.

## Creating routes
### Handling HTTP requests
In an Express.js application, handling HTTP requests is done by defining routes for different HTTP methods (GET, POST, PUT, DELETE, etc.) and specifying the appropriate function to be executed when a request is made to that route. Here's an example of how to handle a GET request in an Express.js application:
```js
const express = require('express');
const app = express();

app.get('/hello', (req, res) => {
  res.send('Hello, world!');
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```
In this example, we define a GET route for the path "/hello". When a GET request is made to this route, the callback function is executed and the response "Hello, world!" is sent back to the client. We also start the server and listen on port 3000.

**Here's an example of how to handle a POST request:**
```js
const express = require('express');
const app = express();

app.post('/submit', (req, res) => {
  const name = req.body.name;
  const email = req.body.email;

  // Process the data...

  res.send('Data received');
});

app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

In this example, we define a POST route for the path "/submit". When a POST request is made to this route, the callback function is executed and the request body is parsed to extract the "name" and "email" fields. We then process the data and send a response back to the client.

Note that in order to handle POST requests, we need to include the body-parser middleware, which is used to parse the request body. We can do this by adding the following line to our code:

```js
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({ extended: false }));

```
This tells Express to use the body-parser middleware to parse url-encoded request bodies.

1. Handling a GET request to a specific route:
```js
app.get('/users', (req, res) => {
  // handle GET request to '/users' route
  res.send('List of users');
});

```
2. Handling a POST request to a specific route:
```js
app.post('/users', (req, res) => {
  // handle POST request to '/users' route
  res.send('User created');
});

```
3. Handling a PUT request to a specific route with a dynamic parameter:
```js
app.put('/users/:id', (req, res) => {
  // handle PUT request to '/users/:id' route
  const userId = req.params.id;
  res.send(`User ${userId} updated`);
});
```
4. Handling a DELETE request to a specific route with a dynamic parameter:
```js
app.delete('/users/:id', (req, res) => {
  // handle DELETE request to '/users/:id' route
  const userId = req.params.id;
  res.send(`User ${userId} deleted`);
});

```
In each of these examples, we define a route using a specific HTTP method (`GET`, `POST`, `PUT`, or `DELETE`) and a specific path. When a request is made to that route, the corresponding handler function is called and is passed two objects: req, which contains information about the incoming request, and res, which is used to send a response back to the client. In the examples above, we're simply sending back a string as the response, but you could also send back JSON data, HTML, or other types of data as needed.

## What is middleware?

In Express.js, middleware is a function that can be used to intercept and process incoming requests before they reach the route handlers. Middleware can be used for a variety of purposes, such as authentication, logging, and error handling.

**Here's an example of how to create a middleware function in Express.js:**
```js
// Define a middleware function
const myMiddleware = (req, res, next) => {
  console.log('Incoming request:', req.method, req.url);
  next();
};

// Use the middleware function
app.use(myMiddleware);

```

In this example, myMiddleware is a middleware function that logs information about each incoming request to the console. The next parameter is a function that is used to pass control to the next middleware function in the chain.

To use the middleware function in your Express.js application, you can use the a`pp.use()` method, which adds the middleware function to the middleware chain. The middleware function will be executed for every incoming request that matches the route.

## Validation with Ajv?
Ajv is a popular library for validating JSON data. In an Express.js application, you can use Ajv as middleware to validate incoming HTTP requests. Here's an example of how to use Ajv for validating a POST request in an Express.js application:

```js
const express = require('express');
const app = express();
const Ajv = require('ajv');

// Define a JSON schema for validating the request body
const schema = {
  type: 'object',
  properties: {
    name: { type: 'string' },
    age: { type: 'number' }
  },
  required: ['name', 'age']
};

// Create an Ajv instance and compile the schema
const ajv = new Ajv();
const validate = ajv.compile(schema);

// Define a route that handles a POST request
app.post('/users', (req, res) => {
  const body = req.body;

  // Validate the request body against the schema
  const valid = validate(body);
  if (!valid) {
    return res.status(400).json(validate.errors);
  }

  // If the request body is valid, create a new user and send a response
  const user = createUser(body);
  return res.status(201).json(user);
});

// Start the server
app.listen(3000, () => {
  console.log('Server listening on port 3000');
});

```

In this example, we define a JSON schema for validating the request body of a POST request to the /users route. We create an Ajv instance and compile the schema using `ajv.compile()`.

We then define a route that handles the POST request, and use the validate() method to validate the request body against the schema. If the request body is invalid, we return a 400 response with the validation errors. Otherwise, we create a new user and return a 201 response with the user data.

By using Ajv middleware, we can easily add validation to our Express.js application and ensure that incoming requests meet our data validation requirements.

## What is Eslint?

Eslint is a popular JavaScript linter that helps you identify and fix code quality issues in your code. It checks your code against a set of configurable rules and highlights any deviations from those rules. This helps you maintain consistency and readability in your codebase.

**To install Eslint, you can use the npm package manager:**
```js
npm install eslint --save-dev
```

Once Eslint is installed, you can configure its rules by creating a `.eslintrc` file in the root directory of your project. This file specifies the rules that Eslint should follow. Here's an example .eslintrc file that sets some basic rules:
```js

{
  "env": {
    "browser": true,
    "es6": true,
    "node": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "sourceType": "module"
  },
  "rules": {
    "no-console": "off",
    "indent": [
      "error",
      2
    ],
    "linebreak-style": [
      "error",
      "unix"
    ],
    "quotes": [
      "error",
      "single"
    ],
    "semi": [
      "error",
      "always"
    ]
  }
}
```

In this example, we're setting some rules for the code style, including requiring two-space indentation, Unix-style line breaks, single quotes for strings, and semicolons at the end of statements. We're also turning off the "no-console" rule, which would normally flag any console.log statements as errors.

You can customize the rules to suit your preferences and coding style. Eslint also has many plugins and presets available, which can help you enforce more specific rules for certain frameworks or libraries.

**To run Eslint on your code, you can use the following command:**

```js
npx eslint your-file.js
```

This will check your JavaScript file against the rules specified in your .eslintrc file and report any errors or warnings. You can also configure your text editor or IDE to automatically run Eslint on your code as you type.

Overall, Eslint is a powerful tool for maintaining code quality and consistency in your JavaScript projects.








