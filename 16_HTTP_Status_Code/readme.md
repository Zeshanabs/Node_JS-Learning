## HTTP Headers — Simple Explanation

Think of an HTTP request like sending a parcel or letter.

* **Inside the parcel** → actual data (message, JSON, image, etc.)
* **Outside information on parcel** → sender, receiver, weight, type, etc.

That outside information is called **HTTP Headers**.

### Example

When your browser sends a request to a website:

```http
GET /users HTTP/1.1
Host: example.com
Content-Type: application/json
User-Agent: Chrome
```

These lines are headers.

They give extra information about the request.

---

# What Headers Do

Headers tell:

* where request came from
* what type of data is being sent
* browser/device information
* authentication token
* language
* cache rules
* content size

---

# Request vs Response Headers

## 1. Request Headers

Sent from **client → server**

Example:

```http
GET /api/users HTTP/1.1
Host: localhost:8000
Accept: application/json
User-Agent: PostmanRuntime
```

Meaning:

* `Accept` → client wants JSON
* `User-Agent` → request came from Postman/browser
* `Host` → which server

---

## 2. Response Headers

Sent from **server → client**

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
```

Meaning:

* `Content-Type` → response is JSON
* `Content-Length` → response size

---

# Important Common Headers

| Header           | Purpose                              |
| ---------------- | ------------------------------------ |
| `Content-Type`   | Type of data                         |
| `Authorization`  | Login token/auth                     |
| `User-Agent`     | Browser/app info                     |
| `Accept`         | Which response format client accepts |
| `Cookie`         | Stores session/login data            |
| `Cache-Control`  | Cache behavior                       |
| `Content-Length` | Size of data                         |

---

# Content-Type Examples

## JSON

```http
Content-Type: application/json
```

## HTML

```http
Content-Type: text/html
```

## Form Data

```http
Content-Type: application/x-www-form-urlencoded
```

---

# Custom Headers

You can create your own headers.

Example in Express:

```js
res.setHeader("X-My-Name", "Zeeshan");
```

Result:

```http
X-My-Name: Zeeshan
```

`X-` is commonly used for custom headers.

---

# Reading Headers in Express

## Read Request Header

```js
app.get("/", (req, res) => {
  console.log(req.headers);
  res.send("Done");
});
```

## Set Response Header

```js
res.setHeader("Content-Type", "application/json");
```

---

# Why Headers Are Important

Headers are heavily used in:

* Authentication
* APIs
* Security
* File uploads
* Browser communication
* Content parsing
* CORS
* Caching

Example:

When user logs in:

```http
Authorization: Bearer token123
```

Server checks token from headers.

---

# HTTP Status Codes — Simple Explanation

Status codes tell whether request succeeded or failed.

Example:

```http
200 OK
404 Not Found
500 Internal Server Error
```

---

# 5 Main Categories

| Range | Meaning      |
| ----- | ------------ |
| 1xx   | Information  |
| 2xx   | Success      |
| 3xx   | Redirection  |
| 4xx   | Client Error |
| 5xx   | Server Error |

---

# 1xx — Informational

Temporary information.

Rarely used directly.

Example:

```http
100 Continue
```

---

# 2xx — Success

Request worked successfully.

## 200 OK

Everything worked.

```http
200 OK
```

Example:

```js
res.status(200).json(users);
```

---

## 201 Created

New resource created.

Mostly for POST requests.

```js
res.status(201).json(newUser);
```

Meaning:

* user created
* post created
* product added

---

## 204 No Content

Request succeeded but no response body.

```js
res.status(204).send();
```

---

# 3xx — Redirection

User/browser should go somewhere else.

## 301 Moved Permanently

Permanent redirect.

## 302 Found

Temporary redirect.

Example:

```js
res.redirect("/login");
```

---

# 4xx — Client Errors

Problem from user's side.

---

## 400 Bad Request

Client sent invalid/missing data.

Example:

```js
if (!email) {
  return res.status(400).json({
    message: "Email required"
  });
}
```

---

## 401 Unauthorized

User not logged in.

Example:

```http
401 Unauthorized
```

---

## 403 Forbidden

Logged in, but no permission.

Example:

* normal user trying to access admin panel

---

## 404 Not Found

Resource/route doesn't exist.

Example:

```http
/api/unknown
```

---

# 5xx — Server Errors

Problem from server side.

---

## 500 Internal Server Error

Something broke in backend code.

Example:

```js
undefinedVariable.test();
```

Server crashes → 500 error.

---

## 503 Service Unavailable

Server temporarily unavailable.

Example:

* maintenance
* database down

---

# Express Examples

## 200

```js
res.status(200).json(users);
```

---

## 201

```js
res.status(201).json({
  message: "User created"
});
```

---

## 400

```js
res.status(400).json({
  error: "All fields required"
});
```

---

## 404

```js
res.status(404).json({
  error: "User not found"
});
```

---

## 500

```js
res.status(500).json({
  error: "Internal server error"
});
```

---

# Real-World Flow

## Login Example

### Request

```http
POST /login
Content-Type: application/json
```

Body:

```json
{
  "email": "abc@gmail.com",
  "password": "123"
}
```

---

## Successful Response

```http
200 OK
```

---

## Wrong Password

```http
401 Unauthorized
```

---

## Missing Email

```http
400 Bad Request
```

---

# Easy Memory Trick

| Code Type | Meaning        |
| --------- | -------------- |
| 2xx       | Success        |
| 3xx       | Redirect       |
| 4xx       | User mistake   |
| 5xx       | Server mistake |

---

# Most Important Codes to Remember

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 201  | Created               |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |

---

# Final Summary

## Headers

Headers are extra information sent with requests/responses.

Used for:

* content type
* authentication
* browser info
* caching
* data size

---

## Status Codes

Status codes tell result of request.

* `2xx` → success
* `3xx` → redirect
* `4xx` → client mistake
* `5xx` → server mistake

These concepts are fundamental in:

* Node.js
* Express
* React APIs
* FastAPI
* Backend development
* REST APIs
* Authentication systems
