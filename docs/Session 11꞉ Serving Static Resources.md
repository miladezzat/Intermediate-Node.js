---
tags: [Courses]
title: 'Session 11: Serving Static Resources'
created: '2023-02-16T17:21:29.952Z'
modified: '2023-02-16T17:33:58.017Z'
---

# Session 11: Serving Static Resources

When building a web application in Node.js, it's common to have static files like HTML, CSS, and JavaScript that need to be served to the client. These files can be hosted on a static file server, or they can be served directly from Node.js using the built-in http module.

To serve static resources in Node.js, you can use the express framework. Express provides a built-in middleware called express.static that can be used to serve static files.

Here's an example of how to serve a static file in Node.js using the express framework:
```js
const express = require('express');
const app = express();

// Serve static files
app.use(express.static('public'));

// Start the server
app.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

In the above example, we're creating a new express application and adding the express.static middleware to it. The express.static middleware takes a directory path as its argument, which is the root directory for serving static files.

In this case, we're serving files from the public directory. When a request comes in for a static file, Express will look for the file in the public directory and serve it if it exists.

You can also specify an absolute path for the directory by using the path module:
```js
const path = require('path');

// Serve static files
app.use(express.static(path.join(__dirname, 'public')));

```
In the above example, we're using the path module to join the __dirname (the directory of the current module) with the public directory to create an absolute path.

Another way to serve static files in Node.js is to use the built-in http module. Here's an example:

```js
const http = require('http');
const fs = require('fs');
const path = require('path');

const server = http.createServer((req, res) => {
  if (req.url === '/') {
    const filePath = path.join(__dirname, 'public', 'index.html');
    const readStream = fs.createReadStream(filePath);
    readStream.pipe(res);
  } else if (req.url === '/about') {
    const filePath = path.join(__dirname, 'public', 'about.html');
    const readStream = fs.createReadStream(filePath);
    readStream.pipe(res);
  } else {
    res.statusCode = 404;
    res.end('Not found');
  }
});

server.listen(3000, () => {
  console.log('Server started on port 3000');
});

```

In the above example, we're creating an HTTP server using the http module and handling requests with a callback function. We're checking the req.url to determine which file to serve, and using the fs module to read the file and pipe it to the response.

However, this approach can become unwieldy as the number of files and routes grows. That's why using express and its express.static middleware is the preferred method for serving static files in Node.js.



