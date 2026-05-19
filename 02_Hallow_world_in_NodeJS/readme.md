# Node.js First Program Notes

# Creating the First Node.js Program

In this video, the instructor explains how to:

* Create the first Node.js program
* Run JavaScript using Node.js
* Understand differences between browser JavaScript and Node.js
* Use terminal commands
* Create `package.json`
* Use npm scripts

---

# Project Folder Setup

## Step 1: Create a Main Folder

First, create an empty folder for Node.js projects.

Example:

```text
nodejs
```

Inside this folder, different projects can be created separately.

---

## Step 2: Create a Project Folder

Create another folder inside it.

Example:

```text
01-hello-world
```

This helps organize projects properly.

### Why Create Separate Folders?

Because later there will be many projects and files.

Separate folders help:

* Keep projects organized
* Track code easily
* Avoid confusion

---

# Opening the Project in VS Code

Open the folder in your favorite code editor.

Most developers use:

* Visual Studio Code

After opening the folder, it will initially be empty.

---

# Creating the First JavaScript File

Inside the project folder, create a new file:

```text
hello.js
```

`.js` means JavaScript file.

---

# Writing the First Node.js Code

Write the following code:

```javascript
console.log("Hey There");
```

This prints text in the console.

---

# Running JavaScript Using Node.js

Open the terminal inside VS Code.

Run this command:

```bash
node hello.js
```

Output:

```text
Hey There
```

---

# Important Shortcut

You can also run the file without `.js`

Example:

```bash
node hello
```

Node.js automatically understands it is a JavaScript file.

---

# Understanding What is Happening

When we write:

```bash
node hello.js
```

Node.js executes the JavaScript file outside the browser.

This proves that Node.js is a runtime environment for JavaScript.

---

# Important Difference Between Browser and Node.js

The instructor explains a very important concept:

Not every browser JavaScript feature exists in Node.js.

---

# Example: `window` Object

## In Browser

If you write:

```javascript
console.log(window);
```

inside browser console, it works.

Because browsers provide the `window` object.

---

## In Node.js

If you write:

```javascript
console.log(window);
```

and run it using Node.js:

```bash
node hello.js
```

You get an error:

```text
window is not defined
```

---

# Why Does This Happen?

Because Node.js is not a browser.

Node.js only includes features needed for server-side development.

Browser-related objects are removed.

Examples not available in Node.js:

* `window`
* `document`
* `alert`
* `navigator`
* `getElementById`

---

# Example: `alert()`

## Browser

```javascript
alert("Hello");
```

Works in browser.

---

## Node.js

```javascript
alert("Hello");
```

Gives error:

```text
alert is not defined
```

---

# Why Browser APIs Are Removed

Node.js is designed for backend/server-side programming.

Backend applications do not need:

* Browser windows
* HTML document access
* DOM manipulation

So these features are removed.

---

# Core JavaScript Still Works

Normal JavaScript still works in Node.js.

Example:

```javascript
console.log("Hello");
```

```javascript
let a = 5 + 10;
console.log(a);
```

```javascript
const name = "Zeeshan";
console.log(name);
```

These work perfectly.

---

# Features Added in Node.js

Node.js removes browser features but adds server-related features.

Examples:

* File handling
* Cryptography
* Networking
* HTTP servers
* Operating system access

---

# Example: File Handling

```javascript
const fs = require("fs");

fs.writeFileSync("test.txt", "Hello");
```

This creates a file in the system.

This is not possible in normal browser JavaScript.

---

# npm (Node Package Manager)

## What is npm?

`npm` stands for:

```text
Node Package Manager
```

It is used to:

* Install packages
* Manage dependencies
* Configure projects
* Run scripts

---

# Checking npm Version

Run:

```bash
npm -v
```

This shows the installed npm version.

---

# Initializing a Node.js Project

## Command

```bash
npm init
```

This initializes a Node.js project.

---

# What Happens After `npm init`

Node.js asks some questions:

* Package name
* Version
* Description
* Entry point
* Author
* License

After answering, a new file is created:

```text
package.json
```

---

# What is `package.json`?

`package.json` is the configuration file of a Node.js project.

It contains:

* Project information
* Dependencies
* Scripts
* Version
* Entry file

---

# Example of package.json

```json
{
  "name": "hello-world",
  "version": "1.0.0",
  "main": "hello.js"
}
```

---

# Why package.json is Important

It helps Node.js manage the project.

It stores:

* Installed packages
* Project settings
* Startup commands
* Scripts

---

# Manual vs Automatic Creation

We can manually create `package.json`.

But using:

```bash
npm init
```

is better because:

* Faster
* Less errors
* Automatically structured

---

# Scripts in package.json

Inside `package.json`, there is a section called:

```json
"scripts"
```

Scripts help automate commands.

---

# Example Script

```json
"scripts": {
  "start": "node hello.js"
}
```

---

# Running the Script

Instead of writing:

```bash
node hello.js
```

every time, we can simply write:

```bash
npm start
```

This runs the project automatically.

---

# Why Scripts Are Useful

In real projects:

* Many files exist
* Database connections are needed
* Configurations are required
* Cleanup tasks may run

Scripts automate everything.

---

# Real-World Benefit

Instead of remembering long commands, developers simply run:

```bash
npm start
```

This makes project management easier.

---

# Important Concepts Learned

| Concept            | Meaning                               |
| ------------------ | ------------------------------------- |
| Node.js            | Runtime environment for JavaScript    |
| `.js` file         | JavaScript file                       |
| `node filename.js` | Runs JavaScript file                  |
| npm                | Node Package Manager                  |
| `npm init`         | Creates Node.js project configuration |
| `package.json`     | Project configuration file            |
| `npm start`        | Runs start script                     |

---

# Browser vs Node.js APIs

| Feature         | Browser   | Node.js       |
| --------------- | --------- | ------------- |
| `window`        | Available | Not Available |
| `document`      | Available | Not Available |
| `alert()`       | Available | Not Available |
| `console.log()` | Available | Available     |

---

# Final Summary

* Node.js can execute JavaScript outside the browser.
* JavaScript files are created using `.js` extension.
* Files run using:

```bash
node filename.js
```

* Browser-specific objects like `window` and `alert()` do not exist in Node.js.
* Node.js adds backend/server-side features.
* `npm` is used to manage Node.js projects.
* `npm init` creates the `package.json` configuration file.
* Scripts inside `package.json` help automate project commands.
