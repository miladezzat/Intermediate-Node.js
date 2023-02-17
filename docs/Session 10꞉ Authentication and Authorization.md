---
tags: [Courses]
title: 'Session 10: Authentication and Authorization'
created: '2023-02-16T17:24:27.802Z'
modified: '2023-02-16T17:30:11.285Z'
---

# Session 10: Authentication and Authorization

Introduction to Authentication and Authorization:
Authentication and Authorization are two important concepts in web development that are used to ensure that only authorized users can access certain parts of a website or application. Authentication refers to the process of verifying a user's identity, while Authorization refers to determining what actions a user is allowed to perform.


## Creating a Registration System:
In order to create a registration system for our Node.js application, we need to create a form that allows users to enter their email and password. We can then use a library like bcrypt to hash the password and store the email and hashed password in a database.

## Creating a Login System:
Once we have a registration system in place, we can create a login system that allows users to log in using their email and password. We can use a library like Passport.js to handle the authentication process, and create a session to keep track of the user's login status.

## Protecting Routes:
Once we have a login system in place, we can use it to protect certain routes in our application. We can use middleware to check if the user is logged in before allowing them to access a particular route. If the user is not logged in, we can redirect them to the login page.

## Conclusion:
Authentication and Authorization are important concepts in web development that are used to ensure the security of an application. By creating a registration and login system and protecting certain routes, we can ensure that only authorized users can access sensitive information.


**Here are some code examples to help illustrate these concepts:**

### Creating a Registration System:
```js
const bcrypt = require('bcrypt');
const User = require('./models/User');

app.post('/register', async (req, res) => {
  const { email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const user = new User({ email, password: hashedPassword });
  await user.save();
  res.redirect('/login');
});

```

### Creating a Login System:

```js
const passport = require('passport');

app.post('/login', passport.authenticate('local', { failureRedirect: '/login' }), (req, res) => {
  res.redirect('/dashboard');
});
```

### Protecting Routes:
```js
app.get('/dashboard', isLoggedIn, (req, res) => {
  res.render('dashboard');
});

function isLoggedIn(req, res, next) {
  if (req.isAuthenticated()) {
    return next();
  }
  res.redirect('/login');
}
```

**here are some additional topics that could be covered in the session on authentication and authorization in Node.js:**

- Working with JSON Web Tokens (JWTs) for authentication and authorization
- Storing user passwords securely using techniques like salting and hashing
- Implementing password reset functionality
- Setting up user roles and permissions
- Using middleware to protect routes that require authentication or authorization
- Integrating with third-party authentication providers like Google or Facebook


**Here's an example of creating a simple login system using Node.js and Express:**
```js
const express = require('express');
const bodyParser = require('body-parser');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

const app = express();
const port = 3000;

// Set up body parser middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

// Example user data for demonstration purposes only
const users = [
  { id: 1, email: 'user1@example.com', password: '$2b$10$nHhW8Zh5m5l5e5V5h5xGK.vNyX.s2ayC0tVvq3yFmKkgiFhssjkV2' }, // password is "password1"
  { id: 2, email: 'user2@example.com', password: '$2b$10$e5x5n5V5h5m5l5.vNyX.s2Zh5m5l5e2ayC0tVvq3yFmKkgiFhssjkV2' }, // password is "password2"
];

// Route for handling user login
app.post('/login', (req, res) => {
  // Get the email and password from the request body
  const { email, password } = req.body;

  // Find the user with the matching email
  const user = users.find((u) => u.email === email);

  // If there is no matching user or the password is incorrect, return an error
  if (!user || !bcrypt.compareSync(password, user.password)) {
    return res.status(401).json({ message: 'Invalid email or password' });
  }

  // Create a JWT token for the user and return it in the response
  const token = jwt.sign({ id: user.id }, 'secret');
  res.json({ token });
});

// Middleware function for authenticating JWT tokens
function authenticateToken(req, res, next) {
  // Get the authorization header from the request
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];

  // If there is no token, return an error
  if (!token) {
    return res.sendStatus(401);
  }

  // Verify the token and extract the user ID
  jwt.verify(token, 'secret', (err, decoded) => {
    if (err) {
      return res.sendStatus(403);
    }
    req.userId = decoded.id;
    next();
  });
}

// Example route that requires authentication
app.get('/protected', authenticateToken, (req, res) => {
  res.json({ message: 'Hello, authenticated user!' });
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});

```

In this example, we use the bcrypt library to hash and compare passwords, the `jsonwebtoken` library to generate and verify JWT tokens, and the `body-parser` library to parse the request body.

The /login route accepts a POST request with an email and password in the request body. It finds the user with the matching email and compares the hashed password with the input password. If the password is correct, it generates a JWT token for the user and returns it in the response.

The authenticateToken middleware function extracts the JWT token from the authorization header and verifies it using the jsonwebtoken library. If the token is valid, it extracts the user ID from the payload and sets it in the req object for use in the protected route.

The /protected route requires authentication and returns a simple

