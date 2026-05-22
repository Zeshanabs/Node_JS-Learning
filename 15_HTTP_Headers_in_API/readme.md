# What are HTTP Headers? (Simple Explanation)

Think about sending a **letter through a post office**.

Before sending the letter, you write some extra information on the envelope:

* From address
* To address
* Weight
* Type of package

But the **real message** is inside the envelope.

HTTP works in the same way.

---

# HTTP Request & Response

When your browser opens a website:

1. Browser sends a **Request**
2. Server sends back a **Response**

Inside both request and response, there are:

* **Body** → Actual data
* **Headers** → Extra information about the data

---

# Simple Real-Life Example

## Letter Example

Envelope:

* From: Zeeshan
* To: Friend
* Weight: 200g

Inside:

* Actual letter/message

Here:

* Envelope info = **Headers**
* Letter inside = **Body**

---

# In Web Development

When browser sends request:

```http
GET /users
```

Headers may look like:

```http
Content-Type: application/json
User-Agent: Chrome
Accept-Language: en-US
```

These headers tell the server:

* What type of data is being sent
* Which browser/device sent request
* Which language user prefers

---

# Definition of HTTP Headers

HTTP headers are extra pieces of information sent with:

* Requests
* Responses

They describe the request or response.

---

# Request Headers

These are sent by the **client/browser** to the server.

Example:

```http
GET /api/users HTTP/1.1
Host: example.com
User-Agent: Chrome
Accept: application/json
```

## Meaning

| Header     | Meaning                            |
| ---------- | ---------------------------------- |
| Host       | Which website                      |
| User-Agent | Which browser/device               |
| Accept     | Which response format client wants |

---

# Response Headers

These are sent by the **server** back to browser.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
```

## Meaning

| Header         | Meaning                 |
| -------------- | ----------------------- |
| Content-Type   | Type of response        |
| Content-Length | Size of data            |
| Server         | Which server technology |

---

# Most Common Headers

## 1. Content-Type

Tells data type.

Example:

```http
Content-Type: application/json
```

Means:

> “This data is JSON.”

Other examples:

```http
text/html
image/png
multipart/form-data
```

---

## 2. User-Agent

Tells server about browser/device.

Example:

```http
User-Agent: Chrome
```

---

## 3. Authorization

Used for login/authentication.

Example:

```http
Authorization: Bearer TOKEN
```

Server checks:

* Who is user?
* Is user logged in?

---

## 4. Accept

Tells which response type client accepts.

Example:

```http
Accept: application/json
```

---

# Headers in Browser (Inspect → Network)

When you open a website:

1. Right click
2. Inspect
3. Network tab
4. Refresh page
5. Click any request

You will see:

* Request Headers
* Response Headers

Exactly like shown in the video.

---

# Headers in Postman

When sending API request in Postman:

You can add custom headers like:

| Key           | Value            |
| ------------- | ---------------- |
| Authorization | token123         |
| Content-Type  | application/json |

---

# Express.js Example

## Setting Response Header

```js
app.get("/users", (req, res) => {
    res.setHeader("Content-Type", "application/json");

    res.send(users);
});
```

---

# Custom Headers

You can create your own headers.

Example:

```js
res.setHeader("X-My-Name", "Zeeshan");
```

---

# Why `X-`?

`X-` means:

> “This is a custom header.”

Example:

```http
X-My-Name: Zeeshan
```

This is considered a good practice.

---

# Reading Request Headers

In Express:

```js
app.get("/", (req, res) => {
    console.log(req.headers);

    res.send("Done");
});
```

---

# Important Concept

## Headers = Metadata

Metadata means:

> “Data about data.”

Example:

* Actual JSON = Data
* Content-Type = Metadata about that data

---

# How Middleware Uses Headers

Example:

```js
app.use(express.json());
```

This middleware checks:

```http
Content-Type: application/json
```

If content type is JSON:

* It parses JSON body
* Adds data into:

```js
req.body
```

---

# Example Flow

## Client Sends

```http
POST /users

Headers:
Content-Type: application/json

Body:
{
   "name": "Zeeshan"
}
```

---

## Server Reads Header

Server sees:

```http
Content-Type: application/json
```

Then Express converts body into JS object.

---

# Important Interview Question

## Difference Between Body and Headers

| Body           | Headers           |
| -------------- | ----------------- |
| Actual data    | Extra information |
| JSON/User data | Metadata          |
| Inside packet  | Outside packet    |

---

# Very Important Point

Every HTTP request and response contains:

* Headers
* Body (optional)

Headers are extremely important because they help:

* Authentication
* Content handling
* Caching
* Security
* Browser communication
* API communication

---

# Quick Summary

## Headers are:

* Extra information
* Sent with request/response
* Metadata about data

---

## Request Headers

Sent from:

* Browser/client → Server

---

## Response Headers

Sent from:

* Server → Browser/client

---

# Common Headers

| Header        | Purpose                |
| ------------- | ---------------------- |
| Content-Type  | Data type              |
| Authorization | Login/auth             |
| User-Agent    | Browser/device         |
| Accept        | Accepted response type |

---

# Express Functions

## Set Header

```js
res.setHeader("X-My-Name", "Zeeshan");
```

## Read Headers

```js
req.headers
```
