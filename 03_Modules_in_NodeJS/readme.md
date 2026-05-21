# Node.js Modules Notes

# Introduction to Modules

In this video, the instructor explains:

* What modules are
* What modular programming means
* How to export functions
* How to import modules using `require()`
* Difference between built-in modules and custom modules
* `module.exports`
* `exports`
* Multiple exports
* Built-in Node.js modules

---

# Running the Project Using npm Start

Earlier, a `start` script was created inside `package.json`.

Example:

```json
"scripts": {
  "start": "node hello.js"
}
```

Now instead of writing:

```bash
node hello.js
```

We can simply write:

```bash
npm start
```

This executes the program automatically.

---

# What is Modular Programming?

In real-world applications, projects become very large.

If all code is written in one file:

* It becomes messy
* Hard to manage
* Hard to debug
* Hard to scale

So developers divide code into small reusable parts called modules.

This concept is called:

```text
Modular Programming
```

---

# Example of a Simple Function

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(2, 5));
```

Output:

```text
7
```

Currently, everything is inside one file.

But in large projects, this is not a good practice.

---

# Creating Separate Modules

Suppose we create a new file:

```text
maths.js
```

Purpose:

* Keep all math-related functions inside this file

---

# Moving Function to Another File

## maths.js

```javascript
function add(a, b) {
    return a + b;
}
```

Now the function exists in another file.

---

# Problem After Moving the Function

If `hello.js` tries to use:

```javascript
add(2, 5);
```

It gives error:

```text
add is not defined
```

Why?

Because the function belongs to another module/file.

---

# Importing Modules Using require()

Node.js provides a built-in function:

```javascript
require()
```

This is used to import modules.

---

# Importing Custom Module

## hello.js

```javascript
const maths = require("./maths");
```

---

# Understanding `"./maths"`

## `./`

Means:

```text
Current Directory
```

Node.js searches for the file in the current folder.

---

# Very Important Concept

## Without `./`

```javascript
require("maths")
```

Node.js searches:

* Built-in modules
* Installed packages

NOT your current project folder.

So it gives error:

```text
Module not found
```

---

# With `./`

```javascript
require("./maths")
```

Now Node.js searches inside the current directory.

---

# What Does require() Return?

Initially:

```javascript
console.log(maths);
```

Output:

```text
{}
```

An empty object.

Why?

Because nothing was exported from the module.

---

# Private vs Public Functions

Functions inside a module are private by default.

Other files cannot access them until they are exported.

---

# Exporting Using module.exports

## maths.js

```javascript
function add(a, b) {
    return a + b;
}

module.exports = add;
```

Now the function becomes public.

---

# Importing the Exported Function

## hello.js

```javascript
const maths = require("./maths");

console.log(maths(2, 4));
```

Output:

```text
6
```

---

# Understanding What Happened

This line:

```javascript
module.exports = add;
```

means:

```text
Export the add function from this module
```

So `require("./maths")` returns the `add` function.

---

# Multiple Functions Problem

Suppose we create another function:

```javascript
function subtract(a, b) {
    return a - b;
}
```

---

# Wrong Approach

```javascript
module.exports = add;
module.exports = subtract;
```

This is wrong.

Why?

Because the second export overwrites the first one.

Final export becomes only:

```javascript
subtract
```

---

# Important Rule

`module.exports` stores only ONE final value.

If assigned multiple times:

* Previous value is overwritten

---

# Correct Way: Export an Object

## maths.js

```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

---

# What Happens Now?

`require("./maths")` returns:

```javascript
{
   add: [Function],
   subtract: [Function]
}
```

---

# Using Multiple Functions

## hello.js

```javascript
const maths = require("./maths");

console.log(maths.add(2, 4));
console.log(maths.subtract(2, 4));
```

Output:

```text
6
-2
```

---

# Object Shorthand Syntax

This:

```javascript
module.exports = {
    add: add,
    subtract: subtract
};
```

can be shortened to:

```javascript
module.exports = {
    add,
    subtract
};
```

This is modern JavaScript shorthand syntax.

---

# Destructuring Imported Functions

Instead of:

```javascript
maths.add()
```

we can use destructuring.

---

# Example

```javascript
const { add, subtract } = require("./maths");

console.log(add(2, 4));
console.log(subtract(2, 4));
```

Output:

```text
6
-2
```

---

# Understanding Destructuring

This extracts functions directly from the object.

So instead of:

```javascript
maths.add()
```

we can directly use:

```javascript
add()
```

---

# Another Export Method: exports

Node.js also provides:

```javascript
exports
```

---

# Example

## maths.js

```javascript
exports.add = (a, b) => {
    return a + b;
};

exports.subtract = (a, b) => {
    return a - b;
};
```

---

# Importing

```javascript
const maths = require("./maths");

console.log(maths.add(2, 4));
```

Works exactly the same.

---

# Important Difference

## module.exports

Used for exporting a final object/value.

---

## exports

Used for attaching multiple properties/functions.

---

# Important Note

`module.exports` can overwrite values.

Example:

```javascript
module.exports = add;
```

---

But:

```javascript
exports.add = add;
exports.subtract = subtract;
```

adds properties one by one.

---

# Which One is Better?

The instructor personally prefers:

```javascript
module.exports = {
   add,
   subtract
};
```

because:

* Cleaner
* Easier to read
* Better structure

---

# Built-in Modules in Node.js

Node.js already provides many built-in modules.

These modules come automatically with Node.js.

No installation required.

---

# Example: fs Module

`fs` means:

```text
File System
```

Used for:

* Reading files
* Writing files
* Deleting files

---

# Importing fs

```javascript
const fs = require("fs");
```

Notice:

```javascript
"fs"
```

No `./`

Because it is a built-in module.

---

# Example: http Module

```javascript
const http = require("http");
```

Used for:

* Creating web servers

---

# Example: crypto Module

```javascript
const crypto = require("crypto");
```

Used for:

* Encryption
* Hashing
* Security

---

# Important Rule About require()

| Syntax               | Meaning                                     |
| -------------------- | ------------------------------------------- |
| `require("./maths")` | Import custom module from current directory |
| `require("fs")`      | Import built-in or installed package        |

---

# Why Built-in Modules are Powerful

Node.js provides powerful backend features such as:

* File handling
* Cryptography
* Networking
* HTTP servers
* Streams
* Path handling

This makes Node.js very powerful for backend development.

---

# Real-Life Analogy of Modules

Imagine a company.

Different departments handle different work:

* HR Department
* Finance Department
* IT Department

Similarly, in programming:

* One module handles authentication
* One handles database
* One handles file handling
* One handles APIs

This keeps projects clean and manageable.

---

# Key Concepts Learned

| Concept             | Meaning                             |
| ------------------- | ----------------------------------- |
| Module              | Separate reusable file/code         |
| Modular Programming | Dividing code into small modules    |
| require()           | Imports module                      |
| module.exports      | Exports data/functions              |
| exports             | Another export method               |
| Built-in Modules    | Modules already included in Node.js |

---

# Important Interview Questions

## Question:

What is a module in Node.js?

## Answer:

A module is a reusable block of code stored in a separate file.

---

## Question:

What does `require()` do?

## Answer:

`require()` imports modules into another file.

---

## Question:

Difference between `module.exports` and `exports`?

## Answer:

* `module.exports` exports a final value/object.
* `exports` attaches properties/functions individually.

---

# Final Summary

* Node.js supports modular programming.
* Modules help organize large applications.
* `require()` imports modules.
* `module.exports` exports functions or objects.
* Multiple functions should be exported using objects.
* Built-in modules like `fs`, `http`, and `crypto` are already included in Node.js.
* `./` means current directory while importing custom modules.
