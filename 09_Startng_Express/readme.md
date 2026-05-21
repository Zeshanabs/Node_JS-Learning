# What is Express.js? (Beginner Friendly)

[Express.js Official Website](https://expressjs.com/?utm_source=chatgpt.com)

![Image](https://images.openai.com/static-rsc-4/0evOkEdSxK9xHsMMoFX2oqy5t0M81ssufyWleDjHZezhsdcXcz52Ucyp_Bo3VT4H9z_DkIdbA6DcWNt6XKDhzlSiDd7vE7cVzIiyfKjDyoeGtmxBy2hwP_wJ0M7YKIdFgOrL6yhOlqqW71iml-TbrnOnh_C6uTGjESDvsQlOFnx-POMIlxBwWfdOAELQjcT2?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/O-Ao1OUpv_XUhAcHxHfErH3Fk0uXsumdXK8UEI5NEBvDcNni2HFIttqFm0icH0J3s8s8htLZaCkzVjD0D3aSWlQTXLMCiE80cXO92meSlu8ldgu_chxCQU3JTODQHrqWP73ScscFBP6u_JwCoEKoBPnM5Kd8H_r7Vc1wFK2rf3m19rpNFYmewdWTYIWrakeq?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/NmptutFjrSetdZ4_A5IoRUSLd9id55sLhlK9_11p-NJIEtdxWeyFnvVr7FUe8lI7lv2ziWw7yMlsZ-YGnxiXT2EdIzXtf0cShcjowepKR_wyGL9cqy8mpPX93MJt2XMSCBecLoZbbUy21OnHep-A4bsm8qlKKdUhXs1PrI59I3dspEKnRn-B9eanwxdwnd-p?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/YqsH0eYxkwlgxIy1_T5IQb0Hcjekv7-0wZwLd5CPW-PoQ5RrxQCsVrBTmeHfp2rFaCrIOxRu1K4qcSIfD2wfKzgA6_1AWkYrGjgE1OwC-5dSaYz1KpBUwj2IGhbyx-Yirc03_N0UlRXRtxqxY4F0q2KTyGU4PCM0GE9ie9uNFP2eZ_AMbozA-JnoxWM2EdsM?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/I0RobDB87MDeNI4EwLBjiCemb0pW76DxjdbHRf2bAYa5YLH8lTKxZVfATXUuS7LWpNdj-25kz_is2608_hBJr5Yssh0VGE6SVUceoLpzpTrozAB8HzvfAjh9T5ch1sftqqfLq2lIwdxIWozzQt7k8woKzuy-yZhnb9V3pdd2GEXCQD7Tv242VYf0f45oMS-V?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/2_86_lBCoBtM3OPKhU07wDsaHOqOArctKmoAwNbyLUCGOVo3VmwhsfhbUcOXqWadgpywqCVgIb19SE1W6Amt-RgV708slqjJwgHA9MsNe4HTHpoXyS3COaBV6aYel998Ydj4KbWXzkoVReUTL6Xw09_7Azt4M5ewkDDRu_7bVrrhy-sGaN-w742CAxVSBt4I?purpose=fullsize)

The video is explaining **why Express.js was created** and **how it makes Node.js backend development easier**.

---

# Before Express (Using Only HTTP Module)

In pure Node.js, we use the built-in:

```js
const http = require("http");
```

Then we create a server manually:

```js
const server = http.createServer((req, res) => {

});
```

Inside this function, we manually handle EVERYTHING:

* Routes
* HTTP methods
* URL parsing
* Query parameters
* Headers
* Responses
* Status codes

---

# Problem Without Express

Imagine this:

```js
if(req.url === "/") {

}
else if(req.url === "/about") {

}
else if(req.url === "/signup") {

}
```

Now inside every route:

```js
if(req.method === "GET") {

}
else if(req.method === "POST") {

}
```

Now imagine:

* 50 routes
* GET requests
* POST requests
* PATCH requests
* DELETE requests

Your code becomes:

❌ Huge
❌ Messy
❌ Hard to manage
❌ Hard to scale

That is the main problem Express solves.

---

# What Express Does

Express gives us a cleaner system.

Instead of manually checking:

```js
req.url
req.method
```

Express gives methods like:

```js
app.get()
app.post()
app.put()
app.patch()
app.delete()
```

So your code becomes very clean.

---

# Installing Express

Install it using npm:

```bash
npm install express
```

---

# Importing Express

```js
const express = require("express");
```

Here:

* `require()` imports the package
* `"express"` is the package name

---

# Creating an Express App

```js
const app = express();
```

Very important:

`express()` returns an application object.

This `app` object controls your entire backend.

---

# Creating Routes

## GET Route

```js
app.get("/", (req, res) => {
    res.send("Hello from Home Page");
});
```

---

# Understanding This Step-by-Step

## `app.get()`

Means:

> “Handle GET requests”

---

## `"/"`

This is the route/path.

`/` means:

> Home page

Examples:

| Route      | Meaning      |
| ---------- | ------------ |
| `/`        | Home page    |
| `/about`   | About page   |
| `/contact` | Contact page |

---

## `(req, res)`

These are objects.

### `req`

Request object.

Contains:

* URL
* Query params
* Body data
* Headers
* Method

---

### `res`

Response object.

Used to send data back.

---

# Sending Response

```js
res.send("Hello from Home Page");
```

This sends data to browser.

Browser receives:

```txt
Hello from Home Page
```

---

# Another Route

```js
app.get("/about", (req, res) => {
    res.send("Hello from About Page");
});
```

Now:

| URL      | Output     |
| -------- | ---------- |
| `/`      | Home page  |
| `/about` | About page |

---

# Starting the Server

```js
app.listen(8000, () => {
    console.log("Server Started");
});
```

---

# Understanding `app.listen()`

## `8000`

Port number.

Server runs on:

```txt
localhost:8000
```

---

## Callback Function

Runs when server starts successfully.

---

# Full Basic Express Server

```js
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Hello from Home Page");
});

app.get("/about", (req, res) => {
    res.send("Hello from About Page");
});

app.listen(8000, () => {
    console.log("Server Started");
});
```

---

# Why Express is Better

| Without Express        | With Express              |
| ---------------------- | ------------------------- |
| Manual routing         | Easy routing              |
| Manual method handling | `app.get()`, `app.post()` |
| Hard query parsing     | Built-in                  |
| Messy code             | Clean code                |
| Difficult scaling      | Easy scaling              |

---

# Query Parameters in Express

Example URL:

```txt
/about?name=Zeeshan&age=23
```

Express automatically parses query params.

Access them like this:

```js
app.get("/about", (req, res) => {

    const name = req.query.name;
    const age = req.query.age;

    res.send(`Hello ${name}, your age is ${age}`);
});
```

---

# If User Opens:

```txt
/about?name=Zeeshan&age=23
```

Output:

```txt
Hello Zeeshan, your age is 23
```

---

# HTTP Methods in Express

| Method | Purpose          |
| ------ | ---------------- |
| GET    | Get data         |
| POST   | Send/Create data |
| PUT    | Replace data     |
| PATCH  | Update data      |
| DELETE | Delete data      |

---

# Example of POST Request

```js
app.post("/signup", (req, res) => {

    // Save user in database

    res.send("User Created");
});
```

---

# Internal Working of Express

Internally, Express still uses Node.js HTTP module.

So this:

```js
app.listen()
```

Internally does something similar to:

```js
http.createServer()
```

Express is basically:

✅ A layer on top of Node.js HTTP module
✅ A framework that simplifies backend development

---

# Important Beginner Concept

## Node.js HTTP Module

Low-level.

You handle everything manually.

---

## Express.js

High-level framework.

Makes backend development easier.

---

# Real Life Analogy

Imagine:

## HTTP Module

Like building a car from raw metal.

You build:

* engine
* tires
* doors
* steering

everything yourself.

---

## Express

Like getting a ready-made car framework.

You only focus on driving/building features.

---

# Main Benefits of Express

✅ Cleaner code
✅ Easier routing
✅ Middleware support
✅ Faster development
✅ Better project structure
✅ Easy API creation
✅ Industry standard

---

# Most Important Thing to Remember

Express is not replacing Node.js.

Express is USING Node.js internally.

Flow:

```txt
Browser → Express → Node.js HTTP → Server
```

---

# Beginner Practice Task

Create these routes:

```js
/
```

```js
/about
```

```js
/contact
```

```js
/profile
```

Each should return different text using:

```js
res.send()
```

---

# Expected Folder Structure

```txt
project/
│
├── node_modules/
├── package.json
├── server.js
```

---

# Run Your Server

```bash
node server.js
```

Then open:

```txt
http://localhost:8000
```

---

# One-Line Summary

> Express.js is a Node.js framework that makes server creation, routing, and request handling simple, clean, and scalable.
