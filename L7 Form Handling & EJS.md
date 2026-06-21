

# 🚀 EJS + Express Basics (Simple Notes)

## 📌 What is EJS?

EJS (Embedded JavaScript) is a template engine used in Node.js (Express) to show dynamic data inside HTML.

We can:

* Send variables from backend to frontend
* Use conditions (`if`, `for`, etc.)
* Display data from files, folders, or databases

---

## 📤 Sending Data from Backend (Express → EJS)

In Express, we pass data like this:

```js
res.render("index", {
  name: "Ravi",
  age: 20,
  items: ["pen", "book", "laptop"]
});
```

---

## 📄 Using Data in EJS File

Inside `.ejs` file, we use `<%= %>` and `<% %>`:

### 👉 Print variable

```ejs
<h1>Hello <%= name %></h1>
```

### 👉 Condition

```ejs
<% if(age >= 18) { %>
  <p>You are eligible</p>
<% } else { %>
  <p>Not eligible</p>
<% } %>
```

### 👉 Loop

```ejs
<ul>
  <% items.forEach(item => { %>
    <li><%= item %></li>
  <% }) %>
</ul>
```

---

## 📁 Working with Folders / Files

When we want to load a file or view from a folder:

```js
app.set("views", "./views");
```

Example structure:

```
project/
 ├── views/
 │    ├── index.ejs
 │    ├── home.ejs
```

Then:

```js
res.render("index");
```

---

## 📬 Forms in EJS (VERY IMPORTANT)

### 👉 HTML Form

```html
<form action="/create" method="POST">
  <input type="text" name="username" />
  <input type="password" name="password" />
  <button type="submit">Submit</button>
</form>
```

---

## 📌 Important Form Concepts

### 1. action

* Tells where data goes (route in backend)

```html
This is a route👇
action="/create"
```

---

### 2. method

* `GET` → data in URL
* `POST` → secure, data in body

```html
method="POST"
```

---

### 3. name (MOST IMPORTANT)

Each input must have a `name`

```html
<input name="username" />
```

👉 Backend uses this name to access data:

```js
req.body.username
```

---

## ⚙️ Backend Handling (Express)

```js
app.post("/submit", (req, res) => {
  console.log(req.body); // all form data
});
```

---

## 🧠 Summary

* EJS shows dynamic HTML using backend data
* We send data using `res.render()`
* We use `<%= %>` for printing and `<% %>` for logic
* Forms send data using `GET` or `POST`
* `name` attribute is used to access form values in backend
* Backend reads data using `req.body` or `req.query`

---

