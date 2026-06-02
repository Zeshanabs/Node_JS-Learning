This video explains **Stateful Authentication in Node.js + Express**. Since you like beginner-friendly explanations, here's the entire concept in very simple words.

# What is Authentication?

Authentication means:

> "How does a website know that you are really you?"

Example:

* Facebook asks for email and password.
* You enter them.
* Facebook verifies them.
* Facebook remembers you are logged in.

This process is called **Authentication**.

---

# Two Types of Authentication

## 1. Stateful Authentication

Server remembers users.

## 2. Stateless Authentication

Server does not remember users.

The video focuses on **Stateful Authentication**.

---

# Parking Ticket Example

Imagine:

* You go to a mall.
* Give your car to a parking boy.
* Parking boy gives you Ticket #24.

Now:

You only have:

```text
Ticket #24
```

The parking boy has a notebook:

```text
24 -> Zeeshan's Car
25 -> Ali's Car
26 -> Ahmed's Car
```

When you return:

```text
Ticket #24
```

Parking boy checks notebook:

```text
24 -> Zeeshan's Car
```

and returns your car.

---

# What is State?

The notebook is called the **State**.

Because the parking boy is storing information.

```text
Ticket Number -> Car
```

Similarly:

```text
Session ID -> User
```

is stored on the server.

---

# Stateful Authentication Flow

## Step 1

User sends:

```text
Email
Password
```

to server.

---

## Step 2

Server checks database.

If correct:

```text
Email = zeeshan@gmail.com
Password = 123456
```

Server creates:

```text
Session ID
```

Example:

```text
abc123xyz
```

---

## Step 3

Server stores:

```text
abc123xyz -> Zeeshan
```

inside memory/database.

---

## Step 4

Server sends Session ID back.

```text
abc123xyz
```

---

## Step 5

Browser saves Session ID in Cookie.

```text
Cookie:
UID = abc123xyz
```

---

## Step 6

Every request automatically sends cookie.

```text
GET /profile

Cookie:
UID=abc123xyz
```

---

## Step 7

Server checks:

```text
UID=abc123xyz
```

Looks inside its stored state:

```text
abc123xyz -> Zeeshan
```

Server now knows:

```text
This request belongs to Zeeshan.
```

Access granted.

---

# What is a Session?

A Session is simply:

```text
A temporary identity given after login.
```

Example:

```text
Session ID:
a1b2c3d4
```

This ID identifies you.

---

# What are Cookies?

Cookies are small pieces of data stored by browser.

Example:

```text
UID=abc123xyz
```

Browser stores it automatically.

---

# Why Cookies?

Without cookies:

Every request would require:

```text
Email
Password
```

Again and again.

Imagine:

```text
Open profile
Enter password

Open settings
Enter password

Open dashboard
Enter password
```

Very annoying.

Cookies solve this problem.

---

# User Schema

The video creates a User model.

```js
{
  name: String,
  email: String,
  password: String
}
```

Example User:

```js
{
  name: "Zeeshan",
  email: "zeeshan@gmail.com",
  password: "123456"
}
```

Stored in MongoDB.

---

# Signup Route

```js
router.post("/signup")
```

Purpose:

```text
Create new user
```

Flow:

```text
Get Name
Get Email
Get Password
Store in DB
```

---

# Login Route

```js
router.post("/login")
```

Purpose:

```text
Verify user
```

Flow:

```text
Receive Email
Receive Password
Find User
```

---

## User Lookup

```js
User.findOne({
 email,
 password
})
```

If user exists:

```text
Login Success
```

Otherwise:

```text
Invalid Email or Password
```

---

# UUID Package

The video uses:

```bash
npm install uuid
```

Purpose:

Generate unique session IDs.

Example:

```js
const sessionId = uuidv4();
```

Output:

```text
9e8a1bc0-8fa3-4e6a-a52d
```

---

# Session Map

Video creates:

```js
const sessionIdToUserMap = new Map();
```

Think of it as:

```text
Notebook
```

Example:

```js
{
  "abc123" : User1,
  "xyz456" : User2
}
```

---

# Store Session

```js
setUser(sessionId, user);
```

Stores:

```text
SessionID -> User
```

Example:

```text
abc123 -> Zeeshan
```

---

# Create Cookie

After login:

```js
res.cookie("uid", sessionId);
```

Browser receives:

```text
uid=abc123
```

and stores it.

---

# Middleware

The most important concept.

Middleware means:

```text
A function that runs before route handler.
```

Example:

```js
app.use(...)
```

---

# Authentication Middleware

Purpose:

```text
Check if user is logged in.
```

Flow:

```text
Read Cookie
Find User
Allow Request
```

---

## Read Cookie

```js
req.cookies.uid
```

Gets:

```text
abc123
```

---

## Find User

```js
getUser(uid)
```

Checks:

```text
abc123 -> Zeeshan
```

---

## Attach User

```js
req.user = user;
```

Now every route can access:

```js
req.user
```

Example:

```js
console.log(req.user.name);
```

Output:

```text
Zeeshan
```

---

# next()

```js
next();
```

Meaning:

```text
Continue to next middleware or route.
```

Without it:

```text
Request stops.
```

---

# cookie-parser

Package:

```bash
npm install cookie-parser
```

Used to read cookies.

```js
const cookieParser = require("cookie-parser");
```

Register:

```js
app.use(cookieParser());
```

Now:

```js
req.cookies
```

works.

---

# Protect Routes

Example:

```js
app.use(
 "/url",
 restrictToLoggedInUserOnly
);
```

Meaning:

```text
Only logged-in users can access /url routes.
```

---

# What Happens If User Is Not Logged In?

Middleware checks:

```js
if(!user)
```

Then:

```js
res.redirect("/login");
```

User goes to Login page.

---

# URL Ownership

The video adds:

```js
createdBy
```

inside URL schema.

```js
createdBy: {
 type: mongoose.Schema.Types.ObjectId,
 ref: "user"
}
```

Meaning:

```text
Which user created this URL?
```

---

# Example

User:

```text
Zeeshan
```

creates:

```text
google.com
```

Database:

```js
{
 shortId: "abc",
 redirectURL: "google.com",
 createdBy: UserID
}
```

Now we know:

```text
This URL belongs to Zeeshan.
```

---

# Show Only Current User URLs

Instead of:

```js
URL.find({})
```

Use:

```js
URL.find({
 createdBy: req.user._id
})
```

Meaning:

```text
Show only my URLs.
```

Not everyone else's.

---

# Biggest Problem in This Video

The session is stored in:

```js
Map()
```

which lives in RAM.

Example:

```js
const sessionMap = new Map();
```

When server restarts:

```text
Everything disappears.
```

So:

```text
All users must login again.
```

This is the main limitation of this implementation.

---

# Interview Questions

### What is Authentication?

Verifying the identity of a user.

---

### What is Authorization?

Checking what a user is allowed to do.

Example:

```text
Authentication:
Who are you?

Authorization:
What can you access?
```

---

### What is Session?

Temporary identity after login.

---

### What is Cookie?

Small data stored in browser.

---

### What is Stateful Authentication?

Authentication where server stores user session/state.

---

### Why use Middleware?

To execute logic before route handlers.

---

### What does next() do?

Passes control to the next middleware or route.

---

### Why use cookie-parser?

To read cookies from requests.

---

# Complete Flow Summary

```text
User Signup
     ↓
User Login
     ↓
Server verifies credentials
     ↓
Generate Session ID
     ↓
Store SessionID -> User
     ↓
Send Session ID in Cookie
     ↓
Browser stores Cookie
     ↓
Every Request sends Cookie
     ↓
Middleware reads Cookie
     ↓
Find User
     ↓
req.user = user
     ↓
Allow Access
```

This is the complete **Stateful Authentication using Sessions + Cookies in Express.js** that the video demonstrates.
