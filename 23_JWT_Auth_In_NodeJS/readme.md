This video explains **Token-Based Authentication using JWT (JSON Web Token)**. I'll explain it in very simple language, like teaching a kid.

# First Understand the Problem

Imagine you log in to a website.

The website needs to remember:

* Who you are
* Whether you are logged in
* Your user ID

There are two ways to do this:

## 1. Stateful Authentication (Session-Based)

In this method, the **server remembers everything**.

Example:

```
User Login
     |
     v
Server
     |
     +--> Session ID: ABC123
     +--> User: Zeeshan
```

The server stores:

```js
{
   sessionId: "ABC123",
   user: "Zeeshan"
}
```

### Problems

#### Problem 1: Server Restart

If server crashes or restarts:

```js
{
   sessionId: "ABC123",
   user: "Zeeshan"
}
```

gets deleted.

Result:

❌ All users get logged out.

---

#### Problem 2: Memory Usage

Every logged-in user consumes server memory.

Example:

```
100 users = 100 sessions
1000 users = 1000 sessions
10000 users = 10000 sessions
```

Server memory keeps increasing.

---

# Solution: Stateless Authentication

Stateless means:

> Server stores NO login state.

Instead of storing data on the server:

```
Store data inside a Token
```

Think of a token like a school ID card.

Example:

```
Name: Zeeshan
Roll No: 123
Class: BSCS
```

All information is written on the card itself.

The school doesn't need to look in a register every time.

Just show the card.

---

# What is JWT?

JWT stands for:

**JSON Web Token**

It is a special token that stores user information.

Example:

```js
{
   id: "123",
   email: "zeeshan@gmail.com"
}
```

This information is called the **Payload**.

---

# JWT Structure

A JWT looks like:

```text
xxxxx.yyyyy.zzzzz
```

Three parts:

## 1. Header

Contains information about token type.

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

---

## 2. Payload

Contains user data.

Example:

```json
{
  "id": "123",
  "email": "zeeshan@gmail.com"
}
```

---

## 3. Signature

Used to verify the token.

Example:

```text
abcXYZ123
```

This is generated using a Secret Key.

---

# Secret Key

Very important concept.

Suppose:

```js
SECRET_KEY =
"Zeeshan@123$"
```

Server uses this secret key to create the signature.

```js
jwt.sign(payload, SECRET_KEY)
```

Only someone with the secret key can generate a valid token.

---

# Login Flow

## Step 1

User logs in.

```js
Email
Password
```

↓

Server checks credentials.

---

## Step 2

Server creates payload.

```js
{
  id: "123",
  email: "zeeshan@gmail.com"
}
```

---

## Step 3

Generate token.

```js
const token = jwt.sign(
   payload,
   SECRET_KEY
);
```

---

## Step 4

Store token in cookie.

```js
res.cookie("token", token);
```

Browser stores:

```text
token = eyJhbGciOi...
```

---

# Later When User Visits Again

Browser automatically sends:

```text
token = eyJhbGciOi...
```

to the server.

Server verifies it.

```js
jwt.verify(
   token,
   SECRET_KEY
);
```

If valid:

✅ User is authenticated

If invalid:

❌ User is rejected

---

# Why Can't Someone Change the Token?

Suppose token contains:

```json
{
  "email": "zeeshan@gmail.com"
}
```

Attacker changes it to:

```json
{
  "email": "admin@gmail.com"
}
```

Problem:

The signature no longer matches.

When server runs:

```js
jwt.verify(token, SECRET_KEY);
```

JWT detects tampering.

Result:

❌ Invalid Token

---

# Important Point

JWT data is:

✅ Readable

❌ Not editable

Anyone can decode the payload.

But nobody can modify it without the secret key.

Think of it like a currency note:

* Everyone can read ₹500 or Rs.5000
* Nobody can legally print one

Because the government signature/security features are missing.

JWT works similarly.

---

# Code Used in the Video

## Install JWT

```bash
npm install jsonwebtoken
```

Import:

```js
const jwt = require("jsonwebtoken");
```

---

## Create Token

```js
function setUser(user) {
  return jwt.sign(
    {
      id: user._id,
      email: user.email
    },
    "SECRET_KEY"
  );
}
```

---

## Verify Token

```js
function getUser(token) {
  return jwt.verify(
    token,
    "SECRET_KEY"
  );
}
```

---

# Session vs JWT

| Feature                 | Session | JWT           |
| ----------------------- | ------- | ------------- |
| State stored on server  | ✅ Yes   | ❌ No          |
| Uses server memory      | ✅ Yes   | ❌ No          |
| Survives server restart | ❌ No    | ✅ Yes         |
| Easy scaling            | ❌ Hard  | ✅ Easy        |
| Popular in APIs         | ❌ Less  | ✅ Very common |

---

# Why Banks Often Use Sessions

Banks need:

* Very high security
* Short login duration
* Immediate logout

So many banking systems prefer:

```
Session Authentication
```

instead of long-lived JWT tokens.

---

# Why Social Media Uses JWT

Apps like:

* Facebook
* Instagram
* X

often need users to stay logged in for long periods.

JWT works well because:

* No server-side session storage
* Easy scaling
* Good for APIs
* Works well with mobile apps

---

# Interview Questions

### Q1: What is JWT?

JWT (JSON Web Token) is a compact token used to securely transfer user information and perform stateless authentication.

---

### Q2: What are the three parts of JWT?

1. Header
2. Payload
3. Signature

---

### Q3: What is Payload?

The data stored inside the token.

Example:

```json
{
  "id": "123",
  "email": "user@gmail.com"
}
```

---

### Q4: What is a Secret Key?

A private key used to create and verify JWT signatures.

---

### Q5: Why is JWT called Stateless?

Because the server does not store user login state. The token itself contains the required information.

---

### Q6: Can JWT Payload be hidden?

No. JWT payload is encoded, not encrypted. Anyone with the token can decode and read it.

Therefore:

❌ Don't store passwords inside JWT.

Only store non-sensitive information such as:

```js
{
   id: user._id,
   email: user.email,
   role: "user"
}
```

This is the core concept from the video: **instead of storing login state on the server (sessions), JWT stores the necessary information inside a signed token that the client carries and sends back with each request.**
