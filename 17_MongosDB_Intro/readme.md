# MongoDB — Beginner Friendly Explanation

## What is MongoDB?

MongoDB is a **NoSQL document-based database**.

A database is a place where applications store data.

Examples:

* users
* posts
* products
* comments
* messages

---

# Types of Databases

Mainly 2 types:

| Type           | Example           |
| -------------- | ----------------- |
| SQL Database   | MySQL, PostgreSQL |
| NoSQL Database | MongoDB           |

---

# SQL vs MongoDB

## SQL Database

Stores data in **tables**.

Example:

| id | name    | age |
| -- | ------- | --- |
| 1  | Zeeshan | 22  |

Structure is fixed.

---

## MongoDB

Stores data in **documents** (JSON-like objects).

Example:

```json id="t5m1wd"
{
  "name": "Zeeshan",
  "age": 22
}
```

Flexible structure.

---

# Why MongoDB is Popular in Node.js

Because JavaScript already uses objects and JSON.

MongoDB stores data almost in same format.

JavaScript Object:

```js id="dnv2g6"
const user = {
  name: "Zeeshan",
  age: 22
}
```

MongoDB Document:

```json id="2w3b2t"
{
  "name": "Zeeshan",
  "age": 22
}
```

Very compatible with:

* Node.js
* Express
* React
* MERN Stack

---

# MongoDB Architecture

MongoDB structure is very important.

---

# 1. Database

Top-level container.

Example:

```text id="1hl93u"
socialMediaDB
```

---

# 2. Collection

Inside database we have collections.

Collection = similar to SQL table.

Example:

```text id="k8iy8g"
users
posts
comments
```

---

# 3. Document

Inside collection we store documents.

Document = actual data/object.

Example:

```json id="78s15t"
{
  "name": "Ali",
  "email": "ali@gmail.com"
}
```

---

# Real Structure

```text id="z5g0r2"
Database
   └── Collection
           └── Documents
```

Example:

```text id="8eh72w"
socialMediaDB
   └── users
           └── {name: "Ali"}
           └── {name: "Ahmed"}

   └── posts
           └── {title: "Hello"}
```

---

# MongoDB Uses BSON

MongoDB stores data internally in BSON.

BSON = Binary JSON

JSON:

```json id="6r0byf"
{
  "name": "Zeeshan"
}
```

MongoDB converts it internally into BSON format.

---

# Installing MongoDB

## Official Website

[MongoDB Official Website](https://www.mongodb.com/?utm_source=chatgpt.com)

---

# Install on Windows

Download MongoDB Community Edition installer.

Typical steps:

* Next
* Agree
* Install

---

# Install on macOS (Homebrew)

```bash id="vjlwm9"
brew tap mongodb/brew
brew update
brew install mongodb-community@6.0
```

---

# Start MongoDB Service

macOS:

```bash id="m7glw5"
brew services start mongodb/brew/mongodb-community
```

---

# Open MongoDB Shell

```bash id="fqu0ck"
mongosh
```

If successful:

```bash id="fxc3u4"
test>
```

You are now inside MongoDB shell.

---

# Important MongoDB Commands

---

# Show Databases

```bash id="n4vx9g"
show dbs
```

Shows all databases.

Example:

```text id="fpr6va"
admin
config
local
```

---

# Use/Create Database

```bash id="vgq5m6"
use myDatabase
```

Switches database.

If database doesn't exist:
MongoDB creates it automatically later.

---

# Show Current Database

```bash id="9pq93f"
db
```

---

# Show Collections

```bash id="g7rwmv"
show collections
```

---

# Insert Document

```js id="a9v66r"
db.users.insertOne({
  name: "Zeeshan",
  age: 22
})
```

Meaning:

* collection = users
* inserted one document

---

# Find Documents

```js id="0y2u9u"
db.users.find()
```

Shows all users.

---

# Example Output

```json id="aj7m24"
{
  "_id": ObjectId("123"),
  "name": "Zeeshan",
  "age": 22
}
```

---

# What is `_id`?

MongoDB automatically creates unique ID.

Example:

```json id="14pk0u"
"_id": ObjectId("...")
```

Every document gets unique identity.

---

# Insert Multiple Documents

```js id="hndnza"
db.users.insertMany([
  { name: "Ali" },
  { name: "Ahmed" }
])
```

---

# Find One Document

```js id="8w2mr5"
db.users.findOne({ name: "Ali" })
```

---

# Delete Document

```js id="vk1o26"
db.users.deleteOne({ name: "Ali" })
```

---

# Update Document

```js id="h95frh"
db.users.updateOne(
  { name: "Ahmed" },
  { $set: { age: 25 } }
)
```

---

# CRUD Operations

| Operation | Meaning     |
| --------- | ----------- |
| Create    | Insert data |
| Read      | Get data    |
| Update    | Modify data |
| Delete    | Remove data |

MongoDB supports all CRUD operations.

---

# MongoDB in Real Applications

Used in:

* social media apps
* chat apps
* e-commerce
* AI apps
* MERN stack apps
* dashboards
* analytics systems

---

# Why Developers Like MongoDB

| Feature            | Benefit              |
| ------------------ | -------------------- |
| Flexible schema    | Easy changes         |
| JSON-like data     | Easy with JavaScript |
| Fast development   | Less complexity      |
| Scalable           | Good for large apps  |
| Great with Node.js | MERN stack           |

---

# SQL vs MongoDB Mapping

| SQL      | MongoDB    |
| -------- | ---------- |
| Database | Database   |
| Table    | Collection |
| Row      | Document   |
| Column   | Field      |

---

# Example Comparison

## SQL

```sql id="2r8z0n"
INSERT INTO users(name, age)
VALUES ('Ali', 22);
```

---

## MongoDB

```js id="u69knr"
db.users.insertOne({
  name: "Ali",
  age: 22
})
```

---

# MongoDB + Express Flow

```text id="6z2v4t"
Client
   ↓
Express Server
   ↓
MongoDB Database
```

Example:

1. React frontend sends request
2. Express backend receives it
3. MongoDB stores data
4. Response sent back

---

# Real Example

Suppose user registers:

```json id="cv1ljo"
{
  "name": "Zeeshan",
  "email": "zee@gmail.com"
}
```

Backend stores it in:

```text id="kn0pq5"
users collection
```

Inside MongoDB database.

---

# Important Concepts to Remember

| Concept    | Meaning                   |
| ---------- | ------------------------- |
| Database   | Container                 |
| Collection | Group of documents        |
| Document   | Actual object/data        |
| BSON       | Binary JSON               |
| mongosh    | MongoDB shell             |
| CRUD       | Create Read Update Delete |

---

# Most Important Commands

| Command            | Purpose             |
| ------------------ | ------------------- |
| `show dbs`         | Show databases      |
| `use dbName`       | Switch database     |
| `show collections` | Show collections    |
| `db.users.find()`  | Read data           |
| `insertOne()`      | Insert one document |
| `updateOne()`      | Update              |
| `deleteOne()`      | Delete              |

---

# Final Summary

MongoDB is:

* a NoSQL database
* document-based
* JSON/BSON oriented
* highly used with Node.js and MERN stack

Structure:

```text id="r1z9gq"
Database
   └── Collection
           └── Document
```

Main benefits:

* flexible
* easy with JavaScript
* scalable
* beginner friendly
* powerful for modern apps
