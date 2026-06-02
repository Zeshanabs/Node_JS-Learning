This video explains **Authentication vs Authorization in Node.js/Express** and then implements **Role-Based Authorization** using middleware.

# 1. Authentication vs Authorization

## Authentication (Who are you?)

Authentication means verifying a user's identity.

Example:

* User enters email and password.
* Server verifies credentials.
* User gets logged in.

✅ Result: "Yes, this user is Zeeshan."

---

## Authorization (What are you allowed to do?)

Authorization means checking whether the authenticated user has permission to access a specific resource.

Example:

Imagine a club:

* Membership card = Authentication
* Access to VIP room = Authorization

A normal member can enter the club.

But only VIP members can enter the VIP area.

---

# 2. Problem in Previous Code

The instructor noticed authentication code was repeated many times.

Repeated code is bad because:

* Hard to maintain
* Hard to update
* Creates bugs

Instead of writing authentication logic everywhere, he created middleware.

---

# 3. Authentication Middleware

He creates:

```javascript
function checkForAuthentication(req, res, next) {
  // authentication logic
}
```

Purpose:

```javascript
checkForAuthentication()
```

will run before routes and verify the user.

---

# 4. Get Authorization Header

The middleware reads:

```javascript
const authorizationHeaderValue =
  req.headers["authorization"];
```

Example header:

```http
Authorization: Bearer abc123xyz
```

---

# 5. Check if Token Exists

```javascript
if (
  !authorizationHeaderValue ||
  !authorizationHeaderValue.startsWith("Bearer ")
) {
  return next();
}
```

Meaning:

If:

* Header doesn't exist
* Token doesn't exist

Then simply move to next middleware.

---

# 6. Extract Token

Authorization header looks like:

```http
Bearer abc123xyz
```

Need only:

```javascript
abc123xyz
```

So:

```javascript
const token =
  authorizationHeaderValue.split(" ")[1];
```

### How split works

```javascript
"Bearer abc123xyz".split(" ");
```

Result:

```javascript
["Bearer", "abc123xyz"]
```

Index:

```javascript
[0] = Bearer
[1] = abc123xyz
```

Therefore:

```javascript
const token =
  authorizationHeaderValue.split(" ")[1];
```

---

# 7. Validate Token

The middleware calls:

```javascript
getUser(token);
```

This function:

* Verifies JWT
* Extracts user data

Example:

```javascript
{
  _id: "123",
  email: "test@gmail.com"
}
```

---

# 8. Attach User to Request

```javascript
req.user = user;
```

Now every route can access:

```javascript
req.user
```

Example:

```javascript
console.log(req.user.email);
```

Output:

```javascript
test@gmail.com
```

---

# 9. Move to Next Middleware

```javascript
next();
```

Meaning:

"Authentication completed. Continue."

---

# Final Authentication Middleware

```javascript
function checkForAuthentication(req, res, next) {
  const authorizationHeaderValue =
    req.headers["authorization"];

  if (
    !authorizationHeaderValue ||
    !authorizationHeaderValue.startsWith("Bearer ")
  ) {
    return next();
  }

  const token =
    authorizationHeaderValue.split(" ")[1];

  const user = getUser(token);

  req.user = user;

  next();
}
```

---

# 10. Creating Authorization Middleware

Now comes Authorization.

Instructor creates:

```javascript
function restrictTo(roles) {
}
```

Purpose:

```javascript
restrictTo(["ADMIN"])
```

Only admin can access.

---

# 11. Why Closure is Used?

He returns another function:

```javascript
function restrictTo(roles) {
  return function(req, res, next) {

  };
}
```

This is called a Closure.

Example:

```javascript
restrictTo(["ADMIN"])
```

stores:

```javascript
roles = ["ADMIN"]
```

for future use.

---

# 12. Check if User Logged In

```javascript
if (!req.user) {
  return res.redirect("/login");
}
```

Meaning:

If user not authenticated:

```text
Redirect to Login Page
```

---

# 13. Check User Role

```javascript
if (
  !roles.includes(req.user.role)
) {
  return res.end("Unauthorized");
}
```

Example:

Allowed roles:

```javascript
["ADMIN"]
```

User role:

```javascript
"USER"
```

Check:

```javascript
["ADMIN"].includes("USER")
```

Output:

```javascript
false
```

Result:

```javascript
Unauthorized
```

---

# 14. Allow Access

If role matches:

```javascript
next();
```

User enters route.

---

# Final Authorization Middleware

```javascript
function restrictTo(roles) {
  return function(req, res, next) {

    if (!req.user) {
      return res.redirect("/login");
    }

    if (!roles.includes(req.user.role)) {
      return res.end("Unauthorized");
    }

    next();
  };
}
```

---

# 15. Adding Roles in User Model

Before this, users had:

```javascript
{
  name,
  email,
  password
}
```

Now add:

```javascript
role
```

Schema:

```javascript
role: {
  type: String,
  required: true,
  default: "NORMAL"
}
```

Meaning:

Every new user gets:

```javascript
role = "NORMAL"
```

by default.

---

# 16. Include Role in JWT Token

Before:

```javascript
{
  _id,
  email
}
```

Now:

```javascript
{
  _id,
  email,
  role
}
```

Token generation:

```javascript
jwt.sign({
  _id: user._id,
  email: user.email,
  role: user.role
}, SECRET);
```

Why?

Because middleware needs role information.

---

# 17. Register Authentication Middleware Globally

```javascript
app.use(checkForAuthentication);
```

Meaning:

Every request passes through authentication middleware.

Flow:

```text
Request
   ↓
checkForAuthentication
   ↓
Route
```

---

# 18. Protect a Route

Example:

```javascript
router.get(
  "/",
  restrictTo(["NORMAL"]),
  homeController
);
```

Meaning:

Only NORMAL users can access.

---

# 19. Admin Route

```javascript
router.get(
  "/admin/urls",
  restrictTo(["ADMIN"]),
  adminController
);
```

Meaning:

Only ADMIN users can access.

---

# Request Flow Diagram

```text
User Request
      │
      ▼
checkForAuthentication
      │
      ▼
req.user created
      │
      ▼
restrictTo(["ADMIN"])
      │
      ├── User not logged in
      │      ▼
      │   Redirect Login
      │
      ├── Wrong role
      │      ▼
      │   Unauthorized
      │
      └── Correct role
             ▼
          next()
             ▼
          Route
```

# Interview Questions

### Q1. Authentication and Authorization difference?

**Authentication**
→ Verifies identity.

**Authorization**
→ Verifies permissions.

---

### Q2. Why store user in `req.user`?

To make user information available to all routes and middleware during the current request.

---

### Q3. Why use middleware for authentication?

Because it avoids repeating the same code in every route.

---

### Q4. What does `roles.includes(req.user.role)` do?

Checks whether the user's role exists in the allowed roles array.

---

### Q5. What is Role-Based Access Control (RBAC)?

RBAC means granting access based on user roles.

Example:

```text
ADMIN  → Full Access
MODERATOR → Limited Access
USER → Basic Access
```

This video introduces a basic RBAC (Role-Based Access Control) system where:

1. Authentication identifies the user.
2. Authorization checks the user's role.
3. Middleware protects routes based on allowed roles.
