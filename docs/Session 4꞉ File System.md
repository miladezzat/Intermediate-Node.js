---
tags: [Courses]
title: 'Session 4: File System'
created: '2023-02-16T13:41:26.577Z'
modified: '2023-02-16T13:50:35.876Z'
---

# Session 4: File System
In Node.js, working with the file system is made easy with the `fs` module. In this session, we'll cover the basics of reading and writing files, opening and deleting files, and performing other I/O operations.

## Reading and Writing Files
To read a file using Node.js, we can use the fs.readFile() method. This method takes two arguments: the path to the file and a callback function that will be called when the file has been read.

```js
const fs = require('fs');

fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```


To write a file using Node.js, we can use the fs.writeFile() method. This method takes three arguments: the path to the file, the data to write, and a callback function that will be called when the file has been written.

```js
const fs = require('fs');

fs.writeFile('file.txt', 'Hello, World!', (err) => {
  if (err) throw err;
  console.log('File written successfully!');
});

```
## Opening and Deleting Files
To open a file using Node.js, we can use the `fs.open()` method. This method takes two arguments: the path to the file and the file mode (e.g. 'r' for read, 'w' for write).

```js
const fs = require('fs');

fs.open('file.txt', 'r', (err, fd) => {
  if (err) throw err;
  console.log('File opened successfully!');
  fs.close(fd, (err) => {
    if (err) throw err;
    console.log('File closed successfully!');
  });
});

```

To delete a file using Node.js, we can use the fs.unlink() method. This method takes one argument: the path to the file.

```js
const fs = require('fs');

fs.unlink('file.txt', (err) => {
  if (err) throw err;
  console.log('File deleted successfully!');
});

```

## Writing Files Asynchronously
To write a file asynchronously in Node.js, we can use the fs.writeFile() method with a callback function.
```js
const fs = require('fs');

fs.writeFile('file.txt', 'Hello, World!', (err) => {
  if (err) throw err;
  console.log('File written successfully!');
});

```
