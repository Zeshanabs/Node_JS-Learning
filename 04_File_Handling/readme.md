# Node.js `fs` Module (File System) — Beginner Friendly Explanation

The video is teaching the Node.js built-in module called `fs`.

`fs` = **File System**

It helps Node.js work with:

* files
* folders/directories

Using `fs`, you can:

* create files
* read files
* write data
* append data
* delete files
* copy files
* create folders
* check file info

---

# 1. Importing the `fs` Module

```js
const fs = require("fs");
```

### What is happening here?

* `require()` is used to import modules in Node.js
* `"fs"` is a built-in Node.js module
* We store it inside variable `fs`

Now we can use all file-related functions.

---

# 2. Creating a File

## Synchronous Version

```js
fs.writeFileSync("./test.txt", "Hello World");
```

---

## Understanding Word by Word

### `fs`

The file system module.

---

### `.writeFileSync()`

This function:

* creates a file
* writes data inside it

If file already exists:

* it overwrites old content

---

### `"./test.txt"`

This is the file path.

* `./` means current folder
* `test.txt` is file name

---

### `"Hello World"`

This is the content written inside file.

---

# Result

A file is created:

```txt
test.txt
```

Inside it:

```txt
Hello World
```

---

# 3. What Does "Sync" Mean?

`Sync` = Synchronous

Meaning:

* Node.js waits until task finishes
* then moves to next line

Example:

```js
console.log("1");

fs.writeFileSync("./test.txt", "Hello");

console.log("2");
```

Output order:

```txt
1
2
```

But internally:

* Node waits for file writing to complete first

This is called:

* blocking operation

---

# 4. Asynchronous Version

```js
fs.writeFile("./test.txt", "Hello", (err) => {
    if(err) {
        console.log(err);
    }
});
```

---

# Difference Between Sync and Async

## Synchronous

```js
writeFileSync()
```

* waits
* blocks execution

---

## Asynchronous

```js
writeFile()
```

* does NOT wait
* runs in background
* needs callback

---

# 5. Callback Function

```js
(err) => {}
```

This function runs AFTER operation finishes.

If error happens:

* `err` contains error

If no error:

* `err` becomes `null`

---

# 6. Reading Files

## Sync Version

```js
const result = fs.readFileSync("./contacts.txt", "utf-8");

console.log(result);
```

---

# Understanding

## `readFileSync()`

Reads file content.

---

## `"utf-8"`

Encoding format.

Text files are stored in encoded form.

`utf-8` tells Node.js:

> "Treat this as normal text."

Without encoding:

* Node returns binary buffer data

---

# Example Output

If file contains:

```txt
Ali : 1111
Ahmed : 2222
```

Output:

```txt
Ali : 1111
Ahmed : 2222
```

---

# 7. Async Read File

```js
fs.readFile("./contacts.txt", "utf-8", (err, result) => {
    if(err) {
        console.log("Error", err);
    } else {
        console.log(result);
    }
});
```

---

# Important Difference

## Sync version

Returns result directly:

```js
const data = fs.readFileSync(...)
```

---

## Async version

Does NOT return result directly.

Instead:

* result comes inside callback

```js
(err, result) => {}
```

---

# 8. Appending Data to File

Appending means:

> adding new content without deleting old content

---

## Example

```js
fs.appendFileSync("./test.txt", "\nNew Line");
```

---

# `\n` Means

New line.

Without it:

```txt
HelloNew Line
```

With it:

```txt
Hello
New Line
```

---

# Real World Use Case — Logs

Servers often store logs.

Example:

```txt
10:00 User Login
10:05 User Logout
```

Every new request gets appended.

Example:

```js
fs.appendFileSync("./log.txt", `${Date.now()} New Request\n`);
```

---

# 9. Copying Files

```js
fs.copyFileSync("./test.txt", "./copy.txt");
```

This creates:

* `copy.txt`

With same content as:

* `test.txt`

---

# 10. Deleting Files

```js
fs.unlinkSync("./copy.txt");
```

Deletes file permanently.

---

# 11. Checking File Information

```js
const stats = fs.statSync("./test.txt");

console.log(stats);
```

---

# What Information Comes?

* file size
* creation time
* modification time
* whether it's file/folder

---

# Example

```js
console.log(stats.isFile());
```

Output:

```txt
true
```

---

# 12. Creating Folders

```js
fs.mkdirSync("./myDocs");
```

Creates folder:

```txt
myDocs
```

---

# Nested Folder Creation

```js
fs.mkdirSync("./myDocs/a/b", { recursive: true });
```

Creates:

```txt
myDocs
 └── a
      └── b
```

---

# Why Browser JavaScript Cannot Do This

Normal browser JavaScript cannot directly:

* create files
* delete folders
* access system files

Because of:

* security reasons

But Node.js can do this because:

* it runs on server/computer
* not inside browser sandbox

---

# 13. Main Concept of the Video

The MOST important concept in the video:

## Synchronous vs Asynchronous

### Sync

* blocking
* waits

### Async

* non-blocking
* faster for servers
* uses callbacks/promises

This is extremely important in backend development.

---

# 14. Full Beginner Example

```js
const fs = require("fs");

// Create file
fs.writeFileSync("./test.txt", "Hello");

// Read file
const data = fs.readFileSync("./test.txt", "utf-8");
console.log(data);

// Append data
fs.appendFileSync("./test.txt", "\nNew Line");

// Copy file
fs.copyFileSync("./test.txt", "./copy.txt");

// Delete copy
fs.unlinkSync("./copy.txt");

// Create folder
fs.mkdirSync("./docs");
```

---

# 15. Simple Mental Model

Think of `fs` as:

> A manager that controls files and folders.

You tell it:

* create
* read
* write
* delete
* copy

And Node.js performs those operations on your computer/server.

---

# Important Interview Question

Difference between:

```js
readFile()
```

and

```js
readFileSync()
```

## Answer

| readFileSync            | readFile              |
| ----------------------- | --------------------- |
| synchronous             | asynchronous          |
| blocking                | non-blocking          |
| returns result directly | uses callback         |
| slower for servers      | better for production |

---

# Production Recommendation

In real backend applications:

* asynchronous methods are preferred

Because:

* Node.js servers should not block requests.
