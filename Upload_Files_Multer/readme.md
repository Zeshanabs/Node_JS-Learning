This video explains **file uploading in Node.js using Express and Multer**. Here's a beginner-friendly summary:

# 1. Why Do We Need File Uploads?

Many websites allow users to upload:

* Profile pictures
* Resumes (CVs)
* Documents
* Cover images
* PDFs

To handle these uploads in Node.js, the video uses **Multer**.

---

# 2. Install Multer

```bash
npm install multer
```

Multer is middleware that helps Express handle:

```html
multipart/form-data
```

which is required for file uploads.

---

# 3. Create the HTML Form

```html
<form
  action="/upload"
  method="POST"
  enctype="multipart/form-data"
>
  <input
    type="file"
    name="profileImage"
  />

  <button type="submit">
    Upload
  </button>
</form>
```

## Important Attributes

### action

```html
action="/upload"
```

Sends data to:

```js
POST /upload
```

---

### method

```html
method="POST"
```

File uploads should use POST.

---

### enctype

```html
enctype="multipart/form-data"
```

Very important.

Without this:

❌ Files won't be uploaded.

---

# 4. Import Multer

```js
const multer = require("multer");
```

---

# 5. Simple Upload Method

```js
const upload = multer({
  dest: "uploads/"
});
```

Meaning:

> Store uploaded files inside the uploads folder.

---

# 6. Create Upload Route

```js
app.post(
  "/upload",
  upload.single("profileImage"),
  (req, res) => {

    console.log(req.body);

    console.log(req.file);

    res.redirect("/");
  }
);
```

---

## upload.single()

```js
upload.single("profileImage")
```

The name must match:

```html
<input
  type="file"
  name="profileImage"
/>
```

Otherwise Multer won't find the file.

---

# 7. Access Uploaded File

```js
req.file
```

Contains:

```js
{
  fieldname: "profileImage",
  originalname: "photo.png",
  filename: "abc123",
  path: "uploads/abc123",
  size: 1024
}
```

---

# 8. Problem with Simple Upload

When using:

```js
dest: "uploads/"
```

Multer generates random filenames.

Example:

```txt
uploads/
  f9d7c3f1a4
```

You lose control over:

* Filename
* Extension
* Storage logic

---

# 9. Use diskStorage()

To customize storage:

```js
const storage = multer.diskStorage({
  destination: function(req, file, cb) {
    cb(null, "./uploads/");
  },

  filename: function(req, file, cb) {
    cb(
      null,
      Date.now() + "-" + file.originalname
    );
  }
});
```

---

## destination()

```js
destination: function(req, file, cb) {
  cb(null, "./uploads/");
}
```

Tells Multer:

> Save files inside uploads folder.

---

## filename()

```js
filename: function(req, file, cb) {
  cb(
    null,
    Date.now() + "-" + file.originalname
  );
}
```

Example:

```txt
1717664521234-profile.png
```

This makes filenames unique.

---

# 10. Create Upload Object

```js
const upload = multer({
  storage: storage
});
```

Now Multer uses your custom configuration.

---

# 11. Complete Example

```js
const express = require("express");
const multer = require("multer");

const app = express();

const storage = multer.diskStorage({

  destination: function(req, file, cb) {
    cb(null, "./uploads/");
  },

  filename: function(req, file, cb) {
    cb(
      null,
      Date.now() +
      "-" +
      file.originalname
    );
  }

});

const upload = multer({
  storage: storage
});

app.post(
  "/upload",
  upload.single("profileImage"),
  (req, res) => {

    console.log(req.file);

    res.send("File Uploaded");
  }
);

app.listen(8000);
```

---

# 12. Why Use Date.now()?

Suppose two users upload:

```txt
resume.pdf
```

Without Date.now():

```txt
resume.pdf
resume.pdf
```

Second upload may overwrite first.

With Date.now():

```txt
1711111111-resume.pdf
1712222222-resume.pdf
```

No conflict.

---

# 13. Multiple File Uploads

### Single File

```js
upload.single("profileImage")
```

---

### Multiple Files (Same Field)

```js
upload.array("photos", 5)
```

Up to 5 photos.

Access:

```js
req.files
```

---

### Multiple Different Fields

```js
upload.fields([
  { name: "profileImage" },
  { name: "coverImage" }
])
```

HTML:

```html
<input type="file" name="profileImage">
<input type="file" name="coverImage">
```

Access:

```js
req.files.profileImage
req.files.coverImage
```

---

# 14. Common Errors

### Error 1

```txt
ENOENT: no such file or directory
```

Reason:

```txt
uploads folder does not exist
```

Solution:

Create:

```txt
uploads/
```

manually.

---

### Error 2

```txt
req.file is undefined
```

Reason:

Field names don't match.

Example:

```html
name="profileImage"
```

but

```js
upload.single("image")
```

Mismatch.

---

# 15. Interview Questions

### Q1. What is Multer?

Middleware for handling file uploads in Express.

---

### Q2. Why use `multipart/form-data`?

Because normal form encoding cannot send files.

---

### Q3. What does `upload.single()` do?

Uploads one file from a specific field.

---

### Q4. Difference between `req.body` and `req.file`?

```js
req.body
```

Contains text fields.

```js
req.file
```

Contains uploaded file information.

---

### Q5. What is `diskStorage()`?

A Multer storage engine that gives control over:

* File name
* File location
* Storage behavior

---

## Flow of File Upload

```text
User Selects File
        ↓
HTML Form
        ↓
POST /upload
        ↓
Multer Middleware
        ↓
uploads Folder
        ↓
req.file
        ↓
Save Path in Database
        ↓
Display File Later
```

This is the standard approach used in most real-world applications for profile photos, resumes, documents, and image uploads in Node.js + Express.
