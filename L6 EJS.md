# EJS Setup with Express

## 1. Create a Node Project

```bash
mkdir my-app
cd my-app
npm init -y
```

---

## 2. Install Dependencies

```bash
npm i express ejs
```

---

## 3. Create Folder Structure

```
my-app/
│
├── app.js
├── package.json
│
├── views/
│   ├── index.ejs
│   ├── about.ejs
│   └── partials/
│       ├── header.ejs
│       └── footer.ejs
│
├── public/
│   ├── stylesheets/
│   │   └── style.css
│   │
│   ├── scripts/
│   │   └── script.js
│   │
│   └── images/
│
└── node_modules/
```

---

## 4. Setup Express and EJS

### app.js

```js
const express = require('express');
const app = express();

// Set EJS as template engine
app.set('view engine', 'ejs');

// Serve static files
app.use(express.static('public'));

app.get('/', (req, res) => {
    res.render('index');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

---

## 5. Create `views/index.ejs`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Home Page</title>

    <!-- Link CSS -->
    <link rel="stylesheet" href="/stylesheets/style.css">
</head>
<body>

    <h1>Hello EJS!</h1>

    <!-- Link JavaScript -->
    <script src="/scripts/script.js"></script>

</body>
</html>
```

---

## 6. Create CSS File

### public/stylesheets/style.css

```css
body{
    background-color: lightblue;
}

h1{
    color: red;
}
```

---

## 7. Create JavaScript File

### public/scripts/script.js

```js
console.log("JavaScript Loaded");
```

---

## 8. Start Server

```bash
node app.js
```

Open:

```
http://localhost:3000
```

---

# Passing Data

### app.js

```js
app.get('/', (req, res) => {
    res.render('index', {
        name: "Ravindra",
        age: 21
    });
});
```

### index.ejs

```html
<h1><%= name %></h1>
<h2><%= age %></h2>
```

---

# Conditional Rendering

```ejs
<% if(age >= 18){ %>
    <h1>Adult</h1>
<% } else { %>
    <h1>Minor</h1>
<% } %>
```

---

# Loop

```ejs
<% fruits.forEach(fruit => { %>
    <li><%= fruit %></li>
<% }) %>
```

---

# Include Components (Partials)

### views/partials/header.ejs

```html
<header>
    <h1>My Website</h1>
</header>
```

### views/partials/footer.ejs

```html
<footer>
    Copyright 2026
</footer>
```

### index.ejs

```ejs
<%- include('partials/header') %>

<h2>Home Page</h2>

<%- include('partials/footer') %>
```

---

# Summary

### Install

```bash
npm i express ejs
```

### Configure

```js
app.set('view engine', 'ejs');
app.use(express.static('public'));
```

### Views

```
views/
    index.ejs
```

### Static Files

```
public/
    stylesheets/
    scripts/
    images/
```

### Render Page

```js
res.render('index');
```

### Render with Data

```js
res.render('index', {name: "Ravindra"});
```

### Include Components

```ejs
<%- include('partials/header') %>
```

-----
```javascript

💡 3. How it actually works

Inside <% %> → you are in JavaScript

Outside → you are in HTML

<ul>
<% for(let i = 0; i < 3; i++) { %>
    <li>Item <%= i %></li>
<% } %>
</ul>```