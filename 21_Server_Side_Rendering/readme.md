This video explains **Server-Side Rendering (SSR)** in Express.js using the EJS templating engine.

Here’s the complete concept in simple wording.

---

# What is Server-Side Rendering (SSR)?

Normally in frontend apps, the browser renders the UI.

But in **Server-Side Rendering**, the server creates the HTML page first and then sends the complete page to the browser.

Example:

```js
app.get("/test", (req, res) => {
   res.end("<h1>Hi From Server</h1>")
})
```

When user visits `/test`, the server directly sends:

```html
<h1>Hi From Server</h1>
```

This is called:

# Server-Side Rendering

Because the HTML is generated on the server.

---

# Problem Without Templating Engine

Imagine writing full HTML inside Node.js like this:

```js
res.end(`
   <html>
      <body>
         <h1>Hello</h1>
      </body>
   </html>
`)
```

For small pages it works.

But in real projects:

* Navbar
* Tables
* Forms
* CSS
* Dynamic Data
* Loops
* Conditions

become very difficult to manage.

The server code becomes messy.

---

# Solution → Templating Engines

Templating engines help generate HTML dynamically.

Popular template engines:

* EJS
* Pug
* Handlebars

The video uses EJS because it is simple.

---

# What is EJS?

EJS means:

# Embedded JavaScript

It allows you to write:

* HTML
* JavaScript loops
* Conditions
* Variables

inside the same file.

Example:

```ejs
<h1>Hello</h1>

<% urls.forEach(url => { %>
   <li><%= url.shortId %></li>
<% }) %>
```

---

# Installing EJS

```bash
npm install ejs
```

---

# Setting Up EJS in Express

## Step 1: Import path module

```js
const path = require("path")
```

---

## Step 2: Set View Engine

```js
app.set("view engine", "ejs")
```

Meaning:

> Express should use EJS for rendering pages.

---

## Step 3: Tell Express where views are stored

```js
app.set("views", path.resolve("./views"))
```

Meaning:

> All `.ejs` files are inside the `views` folder.

---

# Folder Structure

```txt
project/
│
├── views/
│   └── home.ejs
│
├── routes/
│   └── staticRouter.js
│
├── index.js
```

---

# Creating EJS File

Inside `views/home.ejs`

```html
<h1>Home Page</h1>
```

---

# Rendering EJS File

Instead of:

```js
res.end()
```

use:

```js
res.render("home")
```

Express automatically finds:

```txt
views/home.ejs
```

and renders it.

---

# Passing Data from Backend to Frontend

This is very important.

Example:

```js
res.render("home", {
   urls: allUrls
})
```

Now `urls` becomes available inside `home.ejs`.

---

# Accessing Data Inside EJS

Example:

```ejs
<% urls.forEach(url => { %>

   <li>
      <%= url.shortId %>
   </li>

<% }) %>
```

---

# EJS Syntax

## 1. JavaScript Code

```ejs
<% code %>
```

Example:

```ejs
<% urls.forEach(url => { %>
```

This runs JavaScript.

---

## 2. Print Value

```ejs
<%= value %>
```

Example:

```ejs
<%= url.shortId %>
```

This prints data on screen.

---

# Dynamic URL Shortener Example

The project in the video creates:

* URL Shortener
* Form
* Table
* Dynamic Rendering

using SSR + EJS.

---

# Form Example

```ejs
<form method="POST" action="/url">

   <input
      type="text"
      name="url"
      placeholder="Enter URL"
   />

   <button type="submit">
      Generate
   </button>

</form>
```

---

# Important Part → Form Data Middleware

Forms send URL-encoded data.

So Express needs middleware:

```js
app.use(express.urlencoded({ extended: false }))
```

Without this:

```js
req.body
```

will be undefined.

---

# POST Route Example

```js
router.post("/url", async (req, res) => {

   const shortId = "abc123"

   res.render("home", {
      id: shortId
   })

})
```

---

# Condition Rendering in EJS

Example:

```ejs
<% if (id) { %>

   <p>
      URL Generated:
      <%= id %>
   </p>

<% } %>
```

Meaning:

> Only show this block if `id` exists.

---

# Creating Dynamic Table

Example:

```ejs
<table>

   <tr>
      <th>ID</th>
      <th>Clicks</th>
   </tr>

   <% urls.forEach((url, index) => { %>

      <tr>
         <td><%= index + 1 %></td>
         <td><%= url.shortId %></td>
      </tr>

   <% }) %>

</table>
```

---

# Main Benefit of EJS

Without EJS:

* HTML inside JS
* Hard to read
* Hard to maintain

With EJS:

* Separate frontend files
* Cleaner backend
* Dynamic rendering
* Reusable pages

---

# What Actually Happens Internally?

When this runs:

```js
res.render("home", {
   urls
})
```

EJS:

1. Takes `home.ejs`
2. Runs JavaScript inside it
3. Generates final HTML
4. Sends HTML to browser

The browser only receives final HTML.

If you inspect page source, you only see rendered HTML.

---

# Important Difference

## Client-Side Rendering (CSR)

Frameworks like:

* React
* Vue.js

render UI in browser.

---

## Server-Side Rendering (SSR)

Frameworks/tools like:

* Express.js + EJS
* Next.js

render UI on server.

---

# Key Concepts Learned in This Video

You learned:

1. What SSR is
2. Why SSR is useful
3. What EJS is
4. How to install EJS
5. How to configure EJS in Express
6. How `views` folder works
7. How `res.render()` works
8. Passing backend data to frontend
9. EJS loops
10. EJS conditions
11. Form handling
12. Dynamic tables
13. Clean server architecture
