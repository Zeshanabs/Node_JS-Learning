This video explains how to build a basic URL Shortener service using Node.js, Express.js, and MongoDB with Mongoose.

The project works like Bitly:

* User gives a long URL
* Server generates a short ID
* Short URL redirects to original URL
* Analytics track total clicks and visit history

---

# What is a URL Shortener?

Example:

Original URL:

```txt
https://mywebsite.com/blog/react-course-for-beginners
```

Short URL:

```txt
http://localhost:8001/abc123
```

When someone opens:

```txt
http://localhost:8001/abc123
```

The server redirects them to:

```txt
https://mywebsite.com/blog/react-course-for-beginners
```

---

# Main Features Built in Video

## 1. Create Short URL

POST request:

```txt
POST /url
```

Input:

```json
{
  "url": "https://google.com"
}
```

Response:

```json
{
  "id": "abc123"
}
```

---

## 2. Redirect User

GET request:

```txt
GET /abc123
```

This redirects user to original URL.

---

## 3. Analytics

GET request:

```txt
GET /url/analytics/abc123
```

Response:

```json
{
  "totalClicks": 3,
  "analytics": [
    {
      "timestamp": 1712345678
    }
  ]
}
```

---

# Folder Structure

```txt
short-url/
│
├── controllers/
│   └── url.js
│
├── models/
│   └── url.js
│
├── routes/
│   └── url.js
│
├── connect.js
├── index.js
├── package.json
```

---

# Step 1 — Initialize Project

Create project:

```bash
mkdir short-url
cd short-url
npm init -y
```

Install dependencies:

```bash
npm install express mongoose shortid
npm install --save-dev nodemon
```

---

# Step 2 — Create Express Server

## index.js

```js
const express = require("express");
const mongoose = require("mongoose");

const urlRoute = require("./routes/url");

const app = express();
const PORT = 8001;

app.use(express.json());

mongoose.connect("mongodb://127.0.0.1:27017/short-url")
.then(() => console.log("MongoDB Connected"));

app.use("/url", urlRoute);

app.listen(PORT, () => {
    console.log(`Server Started at PORT:${PORT}`);
});
```

---

# Important Concepts Explained

## express.json()

Middleware that converts JSON request body into JavaScript object.

Example:

Without:

```js
req.body = undefined
```

With:

```js
req.body = {
   url: "https://google.com"
}
```

---

# Step 3 — Create Mongoose Model

## models/url.js

```js
const mongoose = require("mongoose");

const urlSchema = new mongoose.Schema({
    shortId: {
        type: String,
        required: true,
        unique: true,
    },

    redirectURL: {
        type: String,
        required: true,
    },

    visitHistory: [
        {
            timestamp: {
                type: Number,
            },
        },
    ],
},
{ timestamps: true }
);

const URL = mongoose.model("url", urlSchema);

module.exports = URL;
```

---

# Schema Explanation

## shortId

Unique short code.

Example:

```txt
abc123
```

---

## redirectURL

Original long URL.

Example:

```txt
https://google.com
```

---

## visitHistory

Stores all click history.

Example:

```js
[
   { timestamp: 17123456 },
   { timestamp: 17123567 }
]
```

---

# Step 4 — Create Controller

## controllers/url.js

```js
const shortid = require("shortid");
const URL = require("../models/url");

async function handleGenerateNewShortURL(req, res) {

    const body = req.body;

    if (!body.url) {
        return res.status(400).json({
            error: "url is required",
        });
    }

    const shortID = shortid();

    await URL.create({
        shortId: shortID,
        redirectURL: body.url,
        visitHistory: [],
    });

    return res.json({ id: shortID });
}

module.exports = {
    handleGenerateNewShortURL,
};
```

---

# Important Concept

## Why shortid?

It generates random unique IDs.

Example:

```txt
aBc12X
```

Used for:

```txt
http://localhost:8001/aBc12X
```

---

# Step 5 — Create Routes

## routes/url.js

```js
const express = require("express");

const {
    handleGenerateNewShortURL,
} = require("../controllers/url");

const router = express.Router();

router.post("/", handleGenerateNewShortURL);

module.exports = router;
```

---

# Step 6 — Redirect Route

Add this inside controller:

```js
async function handleGetURL(req, res) {

    const shortId = req.params.shortId;

    const entry = await URL.findOneAndUpdate(
        {
            shortId,
        },
        {
            $push: {
                visitHistory: {
                    timestamp: Date.now(),
                },
            },
        }
    );

    res.redirect(entry.redirectURL);
}
```

Export it:

```js
module.exports = {
    handleGenerateNewShortURL,
    handleGetURL,
};
```

---

# Route for Redirect

```js
router.get("/:shortId", handleGetURL);
```

---

# Important MongoDB Concept

## $push

Adds new item into array.

Example:

Before:

```js
visitHistory: []
```

After:

```js
visitHistory: [
   { timestamp: 17123456 }
]
```

---

# Step 7 — Analytics Route

## Controller

```js
async function handleGetAnalytics(req, res) {

    const shortId = req.params.shortId;

    const result = await URL.findOne({ shortId });

    return res.json({
        totalClicks: result.visitHistory.length,
        analytics: result.visitHistory,
    });
}
```

---

# Route

```js
router.get("/analytics/:shortId", handleGetAnalytics);
```

---

# Final Routes

| Method | Route                   | Purpose          |
| ------ | ----------------------- | ---------------- |
| POST   | /url                    | Create short URL |
| GET    | /:shortId               | Redirect         |
| GET    | /url/analytics/:shortId | Get analytics    |

---

# Important Backend Concepts Used

## REST API

API endpoints for communication.

---

## Middleware

Functions running between request and response.

Example:

```js
app.use(express.json())
```

---

## Controller

Contains business logic.

Example:

```js
handleGenerateNewShortURL
```

---

## Model

Database structure.

Example:

```js
urlSchema
```

---

## Route

Defines endpoint paths.

Example:

```js
router.post("/")
```

---

# Complete Flow

## User sends URL

```txt
POST /url
```

↓

## Server generates short ID

```txt
abc123
```

↓

## Saved in MongoDB

```js
{
   shortId: "abc123",
   redirectURL: "https://google.com"
}
```

↓

## User visits short URL

```txt
localhost:8001/abc123
```

↓

## Server:

* Finds original URL
* Stores click history
* Redirects user

---

# Technologies Used

* Node.js
* Express.js
* MongoDB
* Mongoose
* Postman

---

# Real-World Improvements You Can Add

The video mentions future upgrades:

* User authentication
* Custom short URLs
* Expiration time
* IP tracking
* Location tracking
* Click analytics dashboard
* React frontend
* QR code generation
* Rate limiting
* Redis caching

---

# One Important Mistake Fixed in Video

Initially:

```js
timestamp: Date.now
```

Correct:

```js
timestamp: Date.now()
```

Because:

* `Date.now` = function reference
* `Date.now()` = actual current timestamp

---

# What You Learn From This Project

This project teaches:

* Backend development
* REST APIs
* Express routing
* MongoDB CRUD
* Mongoose schema design
* Redirect handling
* Analytics tracking
* Real-world backend architecture
