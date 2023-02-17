---
tags: [Courses]
title: 'Session 8: Mongoose'
created: '2023-02-16T15:40:12.208Z'
modified: '2023-02-16T16:50:45.695Z'
---

# Session 8: Mongoose

1. Introduction to Mongoose: This topic covers the basics of Mongoose and how it works with MongoDB. You'll learn about connecting to the database, creating a schema, and creating a model.

2. Schemas: A schema is a blueprint for defining the structure of your data. This topic covers how to create a schema, define fields with different data types, and add validators.

3. Models: A model is a class that represents a collection of documents in MongoDB. This topic covers how to create a model from a schema, interact with the database using the model, and define methods on the model.

4. Querying data: This topic covers how to retrieve data from MongoDB using Mongoose. You'll learn about querying using filters, sorting the results, and implementing pagination.

5. Updating data: Updating data is a fundamental operation when working with databases. This topic covers how to update existing documents in the database using Mongoose, including updating a single document, updating multiple documents, and upserting new documents.

6. Validation: Validation ensures that the data you're storing in the database meets certain criteria. This topic covers how to add validation to your schema, use built-in validators, and create custom validators.

7. Middleware: Middleware functions are functions that are executed before or after certain operations on the database. This topic covers how to use middleware functions in Mongoose to perform tasks like data transformation, logging, and error handling.

8. Relationships: Relationships are a way to represent connections between different documents in MongoDB. This topic covers how to define relationships between documents using Mongoose, including one-to-one, one-to-many, and many-to-many relationships.

9. Aggregation: Aggregation is a powerful feature in MongoDB that allows you to perform complex operations on your data. This topic covers how to use aggregation pipelines in Mongoose to perform operations like grouping, sorting, and projecting.

10. Indexes: Indexes are a way to improve the performance of database queries. This topic covers how to create indexes on your data using Mongoose, including single field indexes, compound indexes, and geospatial indexes.



## 1-Introduction to Mongoose

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a simple and powerful way to interact with MongoDB by defining schemas and models for your data. Here are some basic concepts to get started with Mongoose.

**Connecting to the database:**
```js
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/my_database', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
.then(() => {
  console.log('Connected to MongoDB');
})
.catch((err) => {
  console.error(err);
});

```

In this example, we're connecting to a MongoDB database running on `localhost` with the name `my_database`. We're also specifying some options to avoid deprecation warnings. If the connection is successful, the message 'Connected to MongoDB' will be logged to the console.

**Creating a schema:**

In this example, we're creating a schema for a user that has a name, email, and age. The `email` field is required and must be unique. We're then creating a model called `User` that uses this schema.

**Creating a model:**
```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: Number
});

const User = mongoose.model('User', userSchema);
```
In this example, we're creating a model called User that uses the userSchema we defined earlier. The model is a class that provides an interface for interacting with the users collection in the database.

## 2-Schemas: 

A Schema is a blueprint for defining the structure of your data. It is a way to enforce consistency in the structure of the documents stored in a MongoDB collection. Here are some basic concepts to get started with Schemas in Mongoose.

**Creating a schema:**
```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: Number,
  status: {
    type: String,
    enum: ['active', 'inactive', 'deleted'],
    default: 'active'
  }
});

```

In this example, we're creating a schema for a user that has a name, email, age, and status. The `email` field is required and must be unique. The `age` field is a number. The `status` field is a string that must be one of the values in the enum array, with a default value of 'active'.


**Defining fields with different data types:**

**Mongoose supports a variety of data types, including:**String
- Number
- Date
- Boolean
- ObjectID
- Mixed

Here's an example of a schema with different data types:

```js
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: String,
  price: Number,
  releaseDate: Date,
  isAvailable: Boolean,
  tags: [String],
  details: {
    weight: Number,
    dimensions: {
      height: Number,
      width: Number,
      depth: Number
    }
  },
  reviews: [{
    author: String,
    text: String,
    rating: {
      type: Number,
      min: 1,
      max: 5
    }
  }]
});

```

In this example, we're creating a schema for a product that has a name, price, release date, availability, tags, details, and reviews. The `tags` field is an array of strings. The `details` field is an object that has a weight field and a dimensions field that is itself an object with `height`, `width`, and `depth` fields. The reviews field is an array of objects that have an `author`, `text`, and `rating` field that must be a number between 1 and 5.


**Adding validators:**

Mongoose has built-in validators for common use cases, such as required, min, and max. You can also define custom validators using the validate property.

Here's an example of a schema with validators:
```js
const mongoose = require('mongoose');

const postSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true
  },
  content: {
    type: String,
    required: true,
    minlength: 10
  },
  author: {
    type: String,
    validate: {
      validator: function(v) {
        return v.length >= 5;
      },
      message: 'Author name must be at least 5 characters'
    }
  }
});

```

In this example, we're creating a schema for a post that has a title, content, and author. The `title` and `content` fields are required, and the `content` field must be at least 10 characters long. The `author` field has a custom validator that checks that the length of the string is at least 5 characters.

## Models: 
A Model is a class that represents a collection of documents in MongoDB. It is the primary interface for interacting with the database using Mongoose. Here are some basic concepts to get started with Models in Mongoose.

**Creating a model:**
To create a model, you need to pass a name and a schema to the `mongoose.model()` method:
```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: Number,
  status: {
    type: String,
    enum: ['active', 'inactive', 'deleted'],
    default: 'active'
  }
});

const User = mongoose.model('User', userSchema);

```
In this example, we're creating a model called `User` from the `userSchema`.

**Interacting with the database using the model:**

Once you have a model, you can use it to interact with the database using the Mongoose API. Here are some examples:

- Creating a new document:
```js
const newUser = new User({
  name: 'John Doe',
  email: 'john.doe@example.com',
  age: 30,
  status: 'active'
});

newUser.save((err) => {
  if (err) console.error(err);
  console.log('User saved');
});
```
In this example, we're creating a new user document and saving it to the database using the `save()` method on the `newUser` object.

- Finding documents:
```js
User.find({ status: 'active' }, (err, users) => {
  if (err) console.error(err);
  console.log(users);
});

```
In this example, we're finding all user documents that have a status of 'active' using the `find()` method on the `User` model.

- Updating documents:
```js
User.updateOne({ _id: '123' }, { age: 35 }, (err) => {
  if (err) console.error(err);
  console.log('User updated');
});
```
In this example, we're updating a user document with the ID '123' to have an age of 35 using the `updateOne()` method on the `User` model.

- Deleting documents:
```js
User.deleteOne({ _id: '123' }, (err) => {
  if (err) console.error(err);
  console.log('User deleted');
});
```

In this example, we're deleting a user document with the ID '123' using the `deleteOne()` method on the `User` model.

**Defining methods on the model:**
You can define custom instance and static methods on your model using the `methods` and `statics` properties of the schema, respectively.

**Here's an example of a model with custom methods:**
```js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: Number,
  status: {
    type: String,
    enum: ['active', 'inactive', 'deleted'],
    default: 'active'
  }
});

userSchema.methods.getFullName = function() {
  return this.name + ' (' + this.email + ')';
};

userSchema.statics.findActive = function(callback) {
  return this.find({ status: 'active' }, callback);
};

const User = mongoose.model('User', userSchema);
```
In this example, we've defined an instance method

## Querying data: 

Querying data is a fundamental aspect of working with any database, and Mongoose provides a rich set of features for querying data from MongoDB. Here are some basic concepts to get started with querying data in Mongoose.


**Filters:**

The most common way to filter results in Mongoose is by using the find() method. You can specify a filter object as the first argument to the find() method to filter the results based on a set of conditions.

**Here's an example:**
```js
const User = require('./models/user');

User.find({ age: { $gte: 25 } }, (err, users) => {
  if (err) console.error(err);
  console.log(users);
});
```

In this example, we're using the `find()` method to retrieve all user documents that have an age greater than or equal to 25.


**Sorting:**

You can sort the results of a query by using the sort() method. You can specify a sort object as the argument to the sort() method to sort the results based on one or more fields.

Here's an example:

```js
const User = require('./models/user');

User.find()
  .sort({ name: 1 })
  .exec((err, users) => {
    if (err) console.error(err);
    console.log(users);
  });
```

In this example, we're using the `sort()` method to sort the results by the `name` field in ascending order.

**Pagination:**

To implement pagination in Mongoose, you can use the `skip()` and `limit()` methods. The `skip()` method skips a specified number of documents, while the `limit()` method limits the number of documents returned.

**Here's an example:**
```js
const User = require('./models/user');

User.find()
  .sort({ name: 1 })
  .skip(10)
  .limit(5)
  .exec((err, users) => {
    if (err) console.error(err);
    console.log(users);
  });

```
In this example, we're using the `skip()` method to skip the first 10 documents and the `limit()` method to limit the results to 5 documents.

**Combining filters, sorting, and pagination:**

You can combine filters, sorting, and pagination to retrieve specific subsets of documents from the database.

Here's an example:

```js
const User = require('./models/user');

User.find({ age: { $gte: 25 } })
  .sort({ name: 1 })
  .skip(10)
  .limit(5)
  .exec((err, users) => {
    if (err) console.error(err);
    console.log(users);
  });
```

In this example, we're using a filter to retrieve all user documents that have an age greater than or equal to 25, sorting the results by the `name` field in ascending order, skipping the first 10 documents, and limiting the results to 5 documents.


## Updating data: 

Updating data is a common operation in Mongoose, and there are several ways to update documents in the database. Here are some basic concepts to get started with updating data in Mongoose.

**Updating a Single Document:**

To update a single document in Mongoose, you can use the `updateOne()` method or the `findOneAndUpdate()` method. Both methods take two arguments: a filter object to specify the document to update, and an update object to specify the changes to make to the document.

Here's an example using `updateOne()`:
```js
const User = require('./models/user');

User.updateOne({ name: 'John' }, { age: 30 }, (err, result) => {
  if (err) console.error(err);
  console.log(result);
});

```
In this example, we're using the `updateOne()` method to update the age of a user named John to 30.

Here's an example using f`indOneAndUpdate()`:
```js
const User = require('./models/user');

User.findOneAndUpdate({ name: 'John' }, { age: 30 }, { new: true }, (err, user) => {
  if (err) console.error(err);
  console.log(user);
});

```

In this example, we're using the `findOneAndUpdate()` method to update the age of a user named John to 30, and we're passing the `{ new: true }` option to return the updated document.

**Updating Multiple Documents:**

To update multiple documents in Mongoose, you can use the `updateMany()` method. The `updateMany()` method takes a filter object to specify the documents to update, and an update object to specify the changes to make to the documents.

**Here's an example:**
```js
const User = require('./models/user');

User.updateMany({ age: { $lt: 30 } }, { status: 'inactive' }, (err, result) => {
  if (err) console.error(err);
  console.log(result);
});

```
In this example, we're using the `updateMany()` method to update the status of all users under 30 to 'inactive'.

**Upserting New Documents:**

To upsert a new document in Mongoose, you can use the `updateOne()` method with the `upsert` option set to `true`. The upsert option tells Mongoose to create a new document if no documents match the filter.

**Here's an example:**

```js
const User = require('./models/user');

User.updateOne({ name: 'Jane' }, { age: 25, status: 'active' }, { upsert: true }, (err, result) => {
  if (err) console.error(err);
  console.log(result);
});
```
In this example, we're using the `updateOne()` method with the `upsert` option to create a new user named Jane with an age of 25 and a status of 'active'. If a user named Jane already exists, her age and status will be updated.

## Validation: 

Validation is an important part of any database application, and Mongoose makes it easy to add validation to your schema. Here are some basic concepts to get started with adding validation to your schema in Mongoose.

**Defining Fields with Validators:**

When you define a field in your schema, you can include validators to ensure that the data stored in that field meets certain criteria. Mongoose has many built-in validators that you can use, including validators for data types, required fields, and regular expressions.

**Here's an example of defining a field with a built-in validator:**

```js
const UserSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
    unique: true,
    match: /^\S+@\S+\.\S+$/,
  },
  age: {
    type: Number,
    min: 18,
  },
});

```

In this example, we're defining a user schema with fields for name, email, and age. The name and email fields are both required, and the email field is also unique and must match a regular expression pattern. The age field has a minimum value of 18.

**Creating Custom Validators:**

In addition to using built-in validators, you can also create your own custom validators in Mongoose. Custom validators can be useful when you need to check data against more complex criteria that can't be expressed with built-in validators.

**Here's an example of creating a custom validator:**
```js
const UserSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
    unique: true,
    match: /^\S+@\S+\.\S+$/,
  },
  age: {
    type: Number,
    validate: {
      validator: function(value) {
        return value >= 18 && value <= 65;
      },
      message: 'Age must be between 18 and 65',
    },
  },
});

```

In this example, we're defining a custom validator for the age field that checks that the age is between 18 and 65.

**Using Validation in Models:**

Once you've defined your schema with validators, Mongoose will automatically validate the data whenever you create or update a document. If the data fails validation, Mongoose will throw a validation error.

**Here's an example of using validation in a model:**

```js
const User = mongoose.model('User', UserSchema);

const newUser = new User({
  name: 'John',
  email: 'john@example.com',
  age: 16,
});

newUser.save((err) => {
  if (err) console.error(err);
});
```

In this example, we're creating a new user with a name, email, and age that fails validation because the age is less than 18. When we try to save the new user, Mongoose will throw a validation error.


## Middleware: 

Middleware functions are functions that are executed before or after certain operations on the database. In Mongoose, you can use middleware to perform tasks like data transformation, logging, and error handling. Here are some basic concepts to get started with using middleware in Mongoose.

**Types of Middleware:**

In Mongoose, there are two types of middleware: document middleware and query middleware. Document middleware is executed when you perform operations on a single document, such as saving or removing it. Query middleware is executed when you perform operations on a collection of documents, such as finding or updating them.

**Here's an example of defining document middleware:**

```js
const UserSchema = new mongoose.Schema({
  name: String,
  email: String,
});

UserSchema.pre('save', function(next) {
  // Do something before saving the document
  this.name = this.name.toUpperCase();
  next();
});

const User = mongoose.model('User', UserSchema);

const newUser = new User({
  name: 'John',
  email: 'john@example.com',
});

newUser.save((err, user) => {
  if (err) console.error(err);
  console.log(user);
});

```

In this example, we're defining document middleware that is executed before a document is saved. The middleware function capitalizes the name field of the document, then calls the `next()` function to continue with the save operation.

**Here's an example of defining query middleware:**

```js
UserSchema.pre('update', function(next) {
  // Do something before updating the documents
  this.update({}, { $set: { updatedAt: Date.now() } });
  next();
});

const User = mongoose.model('User', UserSchema);

User.updateMany({ name: 'John' }, { email: 'new-email@example.com' }, (err, result) => {
  if (err) console.error(err);
  console.log(result);
});

```

In this example, we're defining query middleware that is executed before updating documents. The middleware function adds a `updatedAt` field to the documents that are being updated, then calls the `next()` function to continue with the update operation.

**Using Middleware in Models:**

Once you've defined your middleware functions, Mongoose will automatically execute them when you perform the corresponding database operations. Middleware functions can be chained together to perform a series of tasks before or after an operation.

**Here's an example of using middleware functions in a model:**

```js
UserSchema.pre('save', function(next) {
  // Do something before saving the document
  this.name = this.name.toUpperCase();
  next();
});

UserSchema.post('save', function(doc, next) {
  // Do something after saving the document
  console.log(`User ${doc.name} saved`);
  next();
});

const User = mongoose.model('User', UserSchema);

const newUser = new User({
  name: 'John',
  email: 'john@example.com',
});

newUser.save((err, user) => {
  if (err) console.error(err);
  console.log(user);
});

```

In this example, we're defining a `pre` middleware function that capitalizes the name field before saving the document, and a `post` middleware function that logs a message after saving the document. When we save a new user, both middleware functions are executed in order.


## Relationships: 

In Mongoose, relationships between documents are represented using references or subdocuments.

To define a reference relationship, you can use the `ref` property in your schema to specify the model that the field references. For example, if you have a `users` collection and a `posts` collection, and each post belongs to a user, you can define the relationship like this:

```js

const postSchema = new mongoose.Schema({
  title: String,
  content: String,
  author: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
});

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  posts: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Post' }]
});

const Post = mongoose.model('Post', postSchema);
const User = mongoose.model('User', userSchema);
```

In this example, the `author` field in the `post` schema references the `User` model using the `ObjectId` type and the `ref` property. Similarly, the `posts` field in the `user` schema references an array of Post models using `ObjectId` type and the ref property.

To populate the referenced documents, you can use the `populate()` method. For example, to retrieve all posts and their corresponding authors, you can do:

```js
Post.find().populate('author').exec((err, posts) => {
  // handle the result
});
```

To define a subdocument relationship, you can nest one schema inside another. For example, if you have a `users` collection and each user has multiple addresses, you can define the relationship like this:

```js
const addressSchema = new mongoose.Schema({
  street: String,
  city: String,
  state: String,
  zip: String
});

const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  addresses: [addressSchema]
});

const User = mongoose.model('User', userSchema);

```

In this example, the `user` schema has an `addresses` field that contains an array of subdocuments with the `address` schema.

To work with subdocuments, you can use array operators like `$push` and `$pull` to add or remove subdocuments from an array. For example, to add a new address to a user, you can do:

```js
const user = new User({ name: 'John', email: 'john@example.com', addresses: [] });
user.addresses.push({ street: '123 Main St', city: 'Anytown', state: 'CA', zip: '12345' });
user.save((err, user) => {
  // handle the result
});
```

In this example, a new user is created with an empty `addresses` array, and a new address is added to the array using the `$push` operator.


## Aggregation: 

**Introduction to Aggregation:**

Aggregation in MongoDB is a pipeline-based framework for data processing. It allows you to perform complex data analysis and transformation operations on your data in a structured way. The aggregation pipeline consists of multiple stages, each stage performing a specific operation on the data.

**How to use Aggregation with Mongoose:**

Mongoose provides a powerful API for using aggregation in your application. Here are the basic steps to use aggregation in Mongoose:

1. Define a schema for your data.
2. Create a Mongoose model using the schema.
3. Use the model to query the database and get the data you want.
4. Use the aggregation pipeline to perform the desired operations on the data.

**Example of Aggregation with Mongoose:**

Let's say we have a collection of products, and we want to find the average price of all the products in a given category. Here's how we can use aggregation in Mongoose to achieve this:

```js
const Product = mongoose.model('Product', new mongoose.Schema({
  name: String,
  price: Number,
  category: String
}));

Product.aggregate([
  { $match: { category: 'Electronics' } },
  { $group: { _id: null, avgPrice: { $avg: '$price' } } }
]).exec(function(err, result) {
  if (err) throw err;
  console.log(result);
});

```

In this example, we first define a schema for our products and create a Mongoose model. Then, we use the `aggregate()` method on the model to define the aggregation pipeline. In this case, we first filter the products by category using the `$match` stage, and then group the results by `null` (meaning all results) using the `$group`stage. We use the `$avg` operator to calculate the average price of all the products in the category, and return the result in the `avgPrice` field. Finally, we call the `exec()` method to execute the aggregation pipeline and log the result to the console.


**Other Aggregation Operations:**

Here are some other common operations you can perform using the aggregation pipeline in Mongoose:

1. Sorting: You can sort the results of your aggregation pipeline using the `$sort` stage. For example, to sort products by price in descending order:

```js
Product.aggregate([
  { $sort: { price: -1 } }
]).exec(function(err, result) {
  if (err) throw err;
  console.log(result);
});

```
2. Filtering: You can filter the results of your aggregation pipeline using the `$match` stage. For example, to filter products by price:
```js
Product.aggregate([
  { $match: { price: { $gte: 500 } } }
]).exec(function(err, result) {
  if (err) throw err;
  console.log(result);
});

```
3. Projecting: You can project specific fields from your documents using the `$project` stage. For example, to return only the name and price of products:

```js
Product.aggregate([
  { $project: { name: 1, price: 1 } }
]).exec(function(err, result) {
  if (err) throw err;
  console.log(result);
});

```

4. Grouping: You can group documents together based on a specific field using the `$group` stage. For example, to group products by category and calculate the average price of each category:

```js
Product.aggregate([
  { $group: { _id: '$category', avgPrice: { $avg: '$price' } } }
]).exec(function(err, result) {
  if (err) throw err;
  console.log(result);
});

```

**Conclusion:**

Aggregation is a powerful feature of MongoDB that allows you to perform complex data analysis and transformation operations on your data. Mongoose provides a powerful API for using aggregation in your application, making it easy to perform advanced operations on your data in a structured way. By mastering aggregation in Mongoose, you can take your MongoDB applications to the next level.

## Indexes: 

Indexes are an essential tool for optimizing database queries. By creating indexes on your data, you can dramatically improve the performance of your database queries. MongoDB supports many different types of indexes, including single field indexes, compound indexes, and geospatial indexes.

**Creating Indexes with Mongoose:**

You can create indexes on your Mongoose models using the Model.index() method. This method accepts an object that specifies the fields to index, as well as any options for the index. For example, to create a single field index on the name field of the users collection:

```js
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number,
  createdAt: { type: Date, default: Date.now }
});

userSchema.index({ name: 1 }); // create a single field index on the name field

const User = mongoose.model('User', userSchema);

```

You can also create compound indexes on multiple fields. To create a compound index on the `name` and `email` fields:
```js
userSchema.index({ name: 1, email: 1 }); // create a compound index on the name and email fields
```
You can also create geospatial indexes for location-based queries. For example, to create a 2d geospatial index on the `location` field:

```js
const placeSchema = new mongoose.Schema({
  name: String,
  location: { type: [Number], index: '2d' }
});

const Place = mongoose.model('Place', placeSchema);

```

**Querying with Indexes:**

Once you've created indexes on your data, you can take advantage of them in your database queries. MongoDB will automatically use the appropriate index to optimize your query if it exists.

For example, to find all users with a name starting with 'J':

```js
User.find({ name: /^J/ }, function(err, users) {
  // ...
});
```

If you've created an index on the name field, MongoDB will use it to optimize the query and return results more quickly.

**Conclusion:**

Indexes are a critical tool for optimizing database queries, and MongoDB provides a rich set of indexing options to help you improve the performance of your application. By creating indexes on your data and taking advantage of them in your queries, you can dramatically improve the performance of your MongoDB application.

