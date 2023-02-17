---
tags: [Courses]
title: 'Session 7: MongoDB'
created: '2023-02-16T14:00:25.321Z'
modified: '2023-02-16T15:40:09.270Z'
---

# Session 7: MongoDB

## Introduction to MongoDB
MongoDB is a document-oriented NoSQL database that is designed for scalability and flexibility. It stores data in JSON-like documents, making it easy to work with data in a flexible and dynamic way. MongoDB is widely used in modern web applications and has become a popular choice for developers who need to handle large and complex datasets.

## How to install and use basic command lines and main functions
To use MongoDB with Node.js, you'll need to install the official MongoDB Node.js driver. You can do this using npm by running the following command:

```bash
npm install mongodb
```

Once you've installed the MongoDB driver, you can create a connection to your MongoDB database using the `MongoClient` class. Here's an example:

```js
const { MongoClient } = require('mongodb');

const uri = 'mongodb://localhost:27017';
const client = new MongoClient(uri);

client.connect((err) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log('Connected to MongoDB');

  const db = client.db('my-database');

  // Use the db object to interact with your MongoDB database
});

```
The `uri` variable specifies the connection string to your MongoDB database, while the `client` object creates a new MongoDB client instance. Once you've created a connection to your database, you can use the `db` object to interact with it.

Here are some basic MongoDB command line examples:

- `show dbs` - displays a list of all databases on the MongoDB server
- `use my-database` - switches to the `my-database` database
`db.createCollection('my-collection')` - creates a new collection called my-collection
- `db.my-collection.insertOne({ name: 'John Doe', age: 30 })` - inserts a new document into the my-collection collection

## How to use Mongoose and models
Mongoose is a popular Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a higher-level, more user-friendly interface for working with MongoDB that is built on top of the official MongoDB Node.js driver.

To use Mongoose, you'll need to install it using npm:
```bash
npm install mongoose
```

Here's an example of how to create a Mongoose model:

```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  age: { type: Number, required: true },
  email: { type: String, required: true, unique: true },
});

const User = mongoose.model('User', userSchema);

module.exports = User;

```

This code creates a Mongoose model called `User`, which is based on a schema that defines the properties of the documents that will be stored in the database. The User model can be used to perform operations on the `users` collection in the database.

Here's an example of how to use the User model to create a new document:

```js
const User = require('./models/user');

const user = new User({
  name: 'John Doe',
  age: 30,
  email: 'johndoe@example.com',
});

user.save((err) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log('User saved to database');
});
```

This code creates a new `User` object and saves it to the database using the `save` method. If there are any errors, they will be logged to the console.

These are just a few examples of how to use MongoDB and Mongoose with Node.
Here's an example of how to retrieve documents from the database using the find method in Mongoose:
```js
const User = require('./models/user');

User.find((err, users) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(users);
});

```
This code retrieves all documents from the `users` collection and logs them to the console. You can also add conditions to the `find` method to retrieve specific documents that match certain criteria.

Here's an example of how to update a document in the database using the updateOne method in Mongoose:

```js
const User = require('./models/user');

User.updateOne({ name: 'John Doe' }, { age: 31 }, (err, result) => {
  if (err) {
    console.error(err);
    return;
  }

  console.log(result);
});
```

This code updates the age of the first document that has a name of 'John Doe' in the users collection. The updateOne method takes two arguments: a filter object that specifies the document to update, and an update object that specifies the new values to set.

In addition to these basic operations, Mongoose provides many other useful features for working with MongoDB. You can define relationships between documents, perform aggregation operations, and more.

I hope this brief introduction to MongoDB with code examples for Node.js has been helpful to you. Let me know if you have any further questions!













