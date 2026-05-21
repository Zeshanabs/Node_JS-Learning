You are learning a very important concept in backend development: **REST API using Express.js**.
Let’s understand the whole video in very simple beginner language.

---

# What They Built in This Video

They created a small backend project called:

## “Users REST API”

This API can:

* Get all users
* Get one user
* Create user
* Update user
* Delete user

This is called **CRUD Operations**

| Operation | Meaning      |
| --------- | ------------ |
| Create    | Add new data |
| Read      | Get data     |
| Update    | Edit data    |
| Delete    | Remove data  |

---

# Project Setup

## Step 1 — Create Node Project

```bash
npm init
```

This creates:

```bash
package.json
```

This file stores:

* project name
* dependencies
* scripts
* version

---

# Step 2 — Install Express

```bash
npm install express
```

Express is a Node.js framework.

It helps us:

* create server
* routes
* APIs
* handle requests

---

# Boilerplate Code

```js
const express = require("express");

const app = express();

const PORT = 8000;

app.listen(PORT, () => {
    console.log(`Server started at ${PORT}`);
});
```

---

# Understanding Line by Line

---

## Import Express

```js
const express = require("express");
```

This imports Express library.

---

## Create App

```js
const app = express();
```

Creates Express application.

Think of it as:

> “Main server object”

---

## Port

```js
const PORT = 8000;
```

Server will run on:

```bash
localhost:8000
```

---

## Start Server

```js
app.listen(PORT, () => {
    console.log(`Server started at ${PORT}`);
});
```

This starts the backend server.

---

# Fake Database

Since they didn’t connect MongoDB yet, they used fake JSON data.

They downloaded fake users from:

[Mockaroo](https://mockaroo.com?utm_source=chatgpt.com)

---

# Example User Object

```js
{
  "id": 1,
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@gmail.com",
  "gender": "Male",
  "job_title": "Developer"
}
```

---

# Import JSON Data

```js
const users = require("./MOCK_DATA.json");
```

Now `users` becomes an array.

---

# First API — Get All Users

```js
app.get("/api/users", (req, res) => {
    return res.json(users);
});
```

---

# Understanding This

## Route

```js
"/api/users"
```

This endpoint returns users.

---

## req

```js
req
```

Means:

> Request from client

---

## res

```js
res
```

Means:

> Response sent back

---

# res.json()

```js
res.json(users)
```

Sends JSON response.

JSON = JavaScript Object Notation

Used for:

* frontend
* mobile apps
* APIs

---

# Browser vs API

They explained something important.

---

# Browser Route

```js
/users
```

Can return HTML.

---

# API Route

```js
/ api/users
```

Returns JSON.

This is a good practice.

---

# Server Side Rendering (SSR)

HTML generated on server.

Fast rendering.

Example:

* Google
* YouTube

---

# Client Side Rendering (CSR)

Server sends JSON.

Frontend renders UI.

Example:

* React
* Flutter

---

# Dynamic Route Parameters

They created:

```js
/api/users/:id
```

---

# What is `:id`

Dynamic value.

Could be:

* 1
* 2
* 50

etc.

---

# Example

```bash
/api/users/1
```

Get user with ID 1.

---

# Code

```js
app.get("/api/users/:id", (req, res) => {

    const id = Number(req.params.id);

    const user = users.find((user) => user.id === id);

    return res.json(user);
});
```

---

# Understanding Important Part

## req.params.id

Gets ID from URL.

Example:

```bash
/api/users/5
```

Then:

```js
req.params.id
```

becomes:

```js
"5"
```

(string)

---

# Convert to Number

```js
Number(req.params.id)
```

Because JSON IDs are numbers.

---

# find()

```js
users.find()
```

Searches array.

Returns first matching user.

---

# Postman

Browser only sends GET requests easily.

For:

* POST
* PATCH
* DELETE

They used:

[Postman](https://www.postman.com?utm_source=chatgpt.com)

---

# POST Request

Create new user.

---

# Route

```js
app.post("/api/users", (req, res) => {

});
```

---

# Problem — req.body Undefined

Initially:

```js
req.body
```

was undefined.

Why?

Because Express cannot understand form data automatically.

---

# Middleware

They used middleware.

```js
app.use(express.urlencoded({ extended: false }));
```

---

# What Middleware Does

Middleware runs before request reaches route.

It can:

* parse data
* authenticate
* log requests
* validate data

---

# urlencoded Middleware

This middleware reads form data.

Then converts it into JavaScript object.

---

# Example

Form data:

```txt
first_name=John
email=john@gmail.com
```

becomes:

```js
{
   first_name: "John",
   email: "john@gmail.com"
}
```

inside:

```js
req.body
```

---

# Create User Logic

```js
app.post("/api/users", (req, res) => {

    const body = req.body;

    users.push({
        ...body,
        id: users.length + 1,
    });

});
```

---

# Understanding

## req.body

Contains form data.

---

## users.push()

Adds new object into array.

---

## Spread Operator

```js
...body
```

Copies all properties.

---

# Example

If body contains:

```js
{
  first_name: "John",
  email: "john@gmail.com"
}
```

Then:

```js
{
   ...body,
   id: 5
}
```

becomes:

```js
{
   first_name: "John",
   email: "john@gmail.com",
   id: 5
}
```

---

# Save Data into JSON File

They used Node.js File System module.

```js
const fs = require("fs");
```

---

# Write File

```js
fs.writeFile("./MOCK_DATA.json",
JSON.stringify(users),
(err, data) => {

});
```

---

# JSON.stringify()

Converts JavaScript object into JSON string.

Because files store text.

---

# Return Response

```js
return res.json({
   status: "success",
   id: users.length
});
```

---

# PATCH Route

For updating user.

```js
app.patch("/api/users/:id", (req, res) => {

});
```

PATCH means:

> partially update data

---

# DELETE Route

```js
app.delete("/api/users/:id", (req, res) => {

});
```

Deletes user.

---

# Route Chaining

Very important Express feature.

---

# Instead of Writing

```js
app.get(...)
app.patch(...)
app.delete(...)
```

They grouped routes.

---

# Cleaner Way

```js
app.route("/api/users/:id")
.get((req, res) => {

})
.patch((req, res) => {

})
.delete((req, res) => {

});
```

This is:

* cleaner
* reusable
* professional

---

# REST API Best Practices Learned

## 1. Use Proper HTTP Methods

| Method | Use        |
| ------ | ---------- |
| GET    | Fetch data |
| POST   | Create     |
| PATCH  | Update     |
| DELETE | Remove     |

---

# Bad Practice

```bash
POST /deleteUser
```

---

# Good Practice

```bash
DELETE /users/:id
```

---

# 2. Separate API Routes

Use:

```bash
/api/users
```

for APIs.

---

# 3. Use JSON

Because:

* platform independent
* works with React
* works with mobile apps

---

# 4. Keep Server and Client Separate

Server:

* handles data

Client:

* handles UI

This is REST architecture.

---

# Full Flow of REST API

```text
Client
   ↓
Request
   ↓
Server
   ↓
Database/File
   ↓
Response(JSON)
   ↓
Frontend Renders UI
```

---

# Main Concepts You Learned

* Express Server
* REST API
* Routes
* GET
* POST
* PATCH
* DELETE
* JSON
* req
* res
* req.params
* req.body
* Middleware
* File System
* Route Chaining
* Dynamic Routes
* Postman
* Server vs Client Rendering

---

# Very Important Beginner Understanding

Backend developer’s job is mostly:

1. Receive request
2. Process data
3. Talk to database
4. Send response

That’s exactly what this project teaches.
