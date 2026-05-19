# Node.js Introduction Notes

## What is Node.js?

Node.js is a runtime environment for JavaScript.

It allows us to run JavaScript code outside the browser.

Before Node.js, JavaScript could only run inside web browsers such as:

* Google Chrome
* Mozilla Firefox
* Safari

With Node.js, JavaScript can run directly on the computer or server.

This means we can use JavaScript to:

* Create web servers
* Build backend applications
* Handle files
* Work with databases
* Create REST APIs
* Build real-time applications

---

# Understanding JavaScript Before Node.js

## JavaScript is a Browser Language

Normally, JavaScript runs inside a browser.

Example:

```javascript
console.log("Hello");
```

If you run this inside the browser console, it works.

But earlier, if you tried to run this directly in your computer terminal, it would not work.

Why?

Because JavaScript needs a JavaScript Engine to execute the code.

---

# What is a JavaScript Engine?

A JavaScript Engine is a program that reads and executes JavaScript code.

Every browser has its own JavaScript engine.

## Different Browsers and Their Engines

| Browser | JavaScript Engine |
| ------- | ----------------- |
| Chrome  | V8 Engine         |
| Firefox | SpiderMonkey      |
| Safari  | JavaScriptCore    |

---

# V8 Engine

The most popular JavaScript engine is the V8 Engine.

It is used inside:

* Google Chrome
* Chromium-based browsers

The V8 engine executes JavaScript code very fast.

---

# Problem Before Node.js

Earlier, JavaScript engines only existed inside browsers.

So JavaScript code could only run inside browsers.

Example:

```javascript
console.log("Hello");
```

This code could run in:

* Chrome
* Firefox
* Safari

But not directly in:

* Terminal
* Operating System
* Server

So JavaScript was limited only to frontend development.

---

# The Idea Behind Node.js

A developer took the V8 Engine from Chrome and embedded it with C++.

This created Node.js.

Now JavaScript could run outside the browser.

---

# What Does “Embedded with C++” Mean?

Node.js internally uses:

* V8 Engine
* C++

C++ helps Node.js interact with the operating system and machine-level features.

Because of C++, Node.js can:

* Read files
* Create servers
* Access system memory
* Handle networking
* Work with databases
* Perform file handling

---

# Simple Understanding of Node.js

## Browser JavaScript

```text
JavaScript + Browser Engine = Runs inside browser
```

## Node.js

```text
JavaScript + V8 Engine + C++ = Runs outside browser
```

---

# Why Node.js is Powerful

Because now JavaScript is no longer limited to browsers.

We can use one language (JavaScript) for:

* Frontend
* Backend
* APIs
* Databases
* Servers

This is why Node.js became very popular.

---

# Node.js is NOT a Framework

Many beginners think Node.js is:

* A framework
* A library

But this is incorrect.

## Correct Definition

Node.js is a Runtime Environment for JavaScript.

---

# What is a Runtime Environment?

A runtime environment is a platform where code executes.

Example:

Node.js provides an environment where JavaScript can execute outside the browser.

---

# Browser vs Node.js

| Browser                        | Node.js                         |
| ------------------------------ | ------------------------------- |
| Runs JavaScript inside browser | Runs JavaScript outside browser |
| Used for frontend              | Used for backend                |
| Has DOM access                 | No DOM access                   |
| Uses browser APIs              | Uses Node APIs                  |

---

# Running JavaScript in Browser

Example:

Open browser console and write:

```javascript
console.log("Hello");
```

Output:

```text
Hello
```

Another example:

```javascript
2 + 3
```

Output:

```text
5
```

This works because the browser uses the V8 Engine.

---

# Running JavaScript Using Node.js

After installing Node.js, open terminal and type:

```bash
node
```

This opens the Node.js interactive terminal.

Now write:

```javascript
console.log("Hello");
```

Output:

```text
Hello
```

Example:

```javascript
2 + 5
```

Output:

```text
7
```

This shows JavaScript is now running outside the browser.

---

# Why This is Possible

This is possible because Node.js already contains:

* V8 Engine
* C++ functionalities

So Node.js can execute JavaScript directly on the machine.

---

# Machine-Level Functionalities in Node.js

Because Node.js uses C++, JavaScript can now perform machine-level tasks.

Examples:

## File Handling

```javascript
const fs = require("fs");

fs.writeFileSync("test.txt", "Hello");
```

This creates a file.

---

## Creating a Server

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
    res.end("Hello World");
});

server.listen(3000);
```

This creates a web server.

---

# Main Purpose of Node.js

Node.js is mainly used for backend development.

Using Node.js, we can create:

* APIs
* Servers
* Authentication systems
* Real-time chat apps
* Streaming applications
* Database applications

---

# Important Features of Node.js

## 1. Open Source

Node.js is free to use.

---

## 2. Cross Platform

It works on:

* Windows
* Linux
* macOS

---

## 3. Fast Performance

Because it uses the V8 Engine.

---

## 4. JavaScript Everywhere

Frontend and backend both use JavaScript.

---

# Important Interview Question

## Question:

What is Node.js?

## Answer:

Node.js is a JavaScript runtime environment that allows JavaScript to run outside the browser using the V8 engine.

---

# Another Important Question

## Question:

Is Node.js a framework?

## Answer:

No.

Node.js is not a framework.

It is a runtime environment for JavaScript.

---

# Key Terms to Remember

| Term                | Meaning                        |
| ------------------- | ------------------------------ |
| JavaScript Engine   | Executes JavaScript code       |
| V8 Engine           | Google's JavaScript engine     |
| Runtime Environment | Platform where code runs       |
| Node.js             | JavaScript runtime environment |
| Backend             | Server-side development        |

---

# Final Summary

* JavaScript originally worked only inside browsers.
* Browsers use JavaScript engines to execute JavaScript code.
* Chrome uses the V8 Engine.
* Node.js uses the V8 Engine with C++.
* Node.js allows JavaScript to run outside the browser.
* Node.js is a runtime environment, not a framework.
* Using Node.js, we can create backend applications, servers, and APIs.
