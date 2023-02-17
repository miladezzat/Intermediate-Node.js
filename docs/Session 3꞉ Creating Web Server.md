---
tags: [Courses]
title: 'Session 3: Creating Web Server'
created: '2023-02-16T13:33:18.949Z'
modified: '2023-02-16T13:41:23.636Z'
---

# Session 3: Creating Web Server
In this session, we will cover how to create a web server using Node.js and the HTTP module. The HTTP module is a core module of Node.js that provides a way to create and handle HTTP requests and responses.

To get started, we first need to require the HTTP module using the `require` function:

```js
const http = require('http');
```
We can then create a server using the createServer method of the HTTP module:

```js
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});
```

Here, we pass a callback function to the `createServer` method, which takes two parameters: req (the request object) and `res` (the response object). In the callback function, we set the status code to 200 and the content type to text/plain, and then we send the response with the message "Hello, World!".

To start the server, we use the `listen` method:

```js
server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

Here, we pass a port number (3000) to the listen method, and we log a message to the console to indicate that the server is running.

Now, if you go to http://localhost:3000/ in your web browser, you should see the message "Hello, World!".

That's it! You've created a simple web server using Node.js and the HTTP module.

## How to handle HTTP requests
When creating a web server with Node.js, handling HTTP requests is an important aspect to consider. Here are some examples of how to handle HTTP requests in Node.js:

1. Handling a GET request:
```
const http = require('http');

http.createServer((req, res) => {
  if (req.method === 'GET') {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello, world!');
  }
}).listen(8080);
```

This code creates a server that listens on port 8080 and responds with "Hello, world!" when it receives a GET request.

2. Handling a POST request:

```js
const http = require('http');
const qs = require('querystring');

http.createServer((req, res) => {
  if (req.method === 'POST') {
    let body = '';

    req.on('data', (data) => {
      body += data;
    });

    req.on('end', () => {
      const post = qs.parse(body);
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.end(`Hello, ${post.name}!`);
    });
  }
}).listen(8080);

```

This code creates a server that listens on port 8080 and responds with "Hello, {name}!" when it receives a POST request with a form that includes a "name" field.

3. Handling a file upload:
```js
const http = require('http');
const formidable = require('formidable');

http.createServer((req, res) => {
  if (req.url === '/upload' && req.method === 'POST') {
    const form = new formidable.IncomingForm();

    form.parse(req, (err, fields, files) => {
      if (err) throw err;
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.write(`File uploaded: ${files.file.name}`);
      res.end();
    });
  } else {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.write('<form action="/upload" method="post" enctype="multipart/form-data">');
    res.write('<input type="file" name="file"><br>');
    res.write('<input type="submit">');
    res.write('</form>');
    return res.end();
  }
}).listen(8080);

```

This code creates a server that listens on port 8080 and allows users to upload files via a form. When a file is uploaded, the server responds with "File uploaded: {filename}".

These examples demonstrate how to handle different types of HTTP requests in Node.js. By using the built-in HTTP module and other libraries, it's easy to create a robust web server that can handle a wide range of requests.


