## What is MVC (Model View Controller)?

MVC is a design pattern used to organize backend code in a clean and scalable way.

It divides the application into **3 parts**:

1. **Model**
2. **View**
3. **Controller**

---

# Simple Understanding

Imagine a restaurant:

* **View** → Waiter takes your order and shows food
* **Controller** → Chef manager who handles requests
* **Model** → Kitchen/database where actual food(data) exists

Flow:

```text
User Request → Route → Controller → Model → Database
                                 ↓
                              Response
```

---

# 1. Model

A **Model** is responsible for:

* Database structure
* Schema
* Database operations

In this video, MongoDB + Mongoose model is used.

Example:

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  firstName: String,
  lastName: String,
  email: String,
});

const User = mongoose.model("user", userSchema);

module.exports = User;
```

### What this does

* Creates blueprint for users
* Connects with MongoDB collection
* Handles CRUD operations

---

# 2. Controller

Controller contains the business logic.

It:

* Receives request
* Uses model
* Sends response

Example:

```js
const User = require("../models/user");

async function handleGetAllUsers(req, res) {
  const users = await User.find({});
  return res.json(users);
}

module.exports = {
  handleGetAllUsers,
};
```

### Meaning

Controller says:

> “Get all users from database and send them back.”

---

# 3. Routes

Routes decide:

```text
Which URL → Which controller
```

Example:

```js
const express = require("express");
const router = express.Router();

const {
  handleGetAllUsers,
} = require("../controllers/user");

router.get("/", handleGetAllUsers);

module.exports = router;
```

Meaning:

```text
GET /users → handleGetAllUsers()
```

---

# 4. Views

Views are frontend pages/UI.

Examples:

* EJS
* React
* HTML

In this video they are not used yet.

Later:

```text
Model → Data
View → Display
```

---

# Why MVC is Important

Before MVC:

```text
Everything inside index.js
```

Problem:

* Huge file
* Difficult to manage
* Hard teamwork
* Hard debugging

After MVC:

```text
models/
controllers/
routes/
views/
```

Everything becomes organized.

---

# Final Folder Structure

```text
project/
│
├── models/
│   └── user.js
│
├── controllers/
│   └── user.js
│
├── routes/
│   └── user.js
│
├── middlewares/
│   └── index.js
│
├── connection.js
│
├── index.js
```

---

# How Request Flows

Suppose user calls:

```text
GET /api/users
```

---

## Step 1 → index.js

```js
app.use("/api/users", userRouter);
```

Meaning:

> If request starts with `/api/users`,
> use `userRouter`

---

## Step 2 → routes/user.js

```js
router.get("/", handleGetAllUsers);
```

Meaning:

```text
/api/users + /
```

Final route:

```text
GET /api/users
```

Controller called:

```js
handleGetAllUsers
```

---

## Step 3 → controller

```js
const users = await User.find({});
```

Controller talks to model.

---

## Step 4 → model

```js
User.find({})
```

Mongoose fetches data from MongoDB.

---

## Step 5 → Response

```js
res.json(users);
```

Data sent back to user.

---

# Why Routes Remove `/users`

Inside route file:

```js
router.get("/")
```

NOT:

```js
router.get("/users")
```

Why?

Because:

```js
app.use("/users", userRouter)
```

already added `/users`.

So:

```text
"/" means "/users"
```

and:

```text
"/:id" means "/users/:id"
```

---

# Middleware in MVC

Middleware runs before request reaches controller.

Example:

```js
app.use(logReqRes("log.txt"));
```

Purpose:

* Logging
* Authentication
* Validation
* Error handling

---

# Connection File

Instead of writing MongoDB connection inside `index.js`,
they moved it into:

```text
connection.js
```

Example:

```js
const mongoose = require("mongoose");

async function connectMongoDB(url) {
  return mongoose.connect(url);
}

module.exports = connectMongoDB;
```

Cleaner architecture.

---

# Benefits of MVC

## 1. Clean Code

Everything separated properly.

---

## 2. Easier Debugging

If user issue exists:

* Check routes
* Check controller
* Check model

Very easy.

---

## 3. Team Collaboration

Backend developers can work independently.

Example:

* One handles routes
* One handles controllers
* One handles models

---

## 4. Scalable

When project grows:

```text
users
posts
comments
likes
messages
payments
```

MVC keeps everything manageable.

---

# CRUD Operations Used

## GET all users

```js
router.get("/")
```

---

## GET user by ID

```js
router.get("/:id")
```

---

## CREATE user

```js
router.post("/")
```

---

## UPDATE user

```js
router.patch("/:id")
```

---

## DELETE user

```js
router.delete("/:id")
```

---

# Important Concept

## Controller manipulates Model

As mentioned in the video:

```text
Controller → changes/uses Model
Model → handles database
```

This is the core idea of MVC.

---

# Real Production Practice

Large companies always separate:

* Routes
* Controllers
* Models
* Services
* Middlewares
* Utilities

Because large apps can contain:

* Thousands of files
* Hundreds of APIs
* Multiple developers

MVC helps keep structure predictable.

---

# Simple Memory Trick

```text
Model     = Database
View      = UI
Controller= Logic
```

or

```text
M → Manages Data
V → Visuals/UI
C → Controls Logic
```
