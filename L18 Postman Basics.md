# 🚀 Postman Basics Guide

Postman is an API development and testing tool used to send HTTP requests and inspect responses.

---

# 📂 Workspace

A **Workspace** is a place where all collections, environments, and APIs are organized.

### Create a Workspace

1. Open Postman.
2. Click **Workspaces** → **Create Workspace**.
3. Give it a name.

Example:

```
E-Commerce Backend
```

---

# 📁 Collections

Collections help organize APIs by modules.

Create separate collections for each feature.

Example:

```
📦 User Collection
📦 Owner Collection
📦 Product Collection
📦 Cart Collection
📦 Order Collection
```

Inside each collection, store related requests.

Example:

```
User Collection
│
├── Register User
├── Login User
├── Get Profile
├── Update User
└── Delete User
```

---

# 🌐 Environment Variables

Instead of writing the complete URL every time, create variables.

## Example

Base URL:

```text
http://localhost:3000
```

Create an environment variable:

```text
baseUrl = http://localhost:3000
```

Now requests become:

```text
{{baseUrl}}/users/register
{{baseUrl}}/users/login
{{baseUrl}}/products
{{baseUrl}}/owner
```

This makes changing server addresses easy.

---

# 📨 HTTP Methods

| Method | Purpose |
|----------|---------|
| GET | Fetch data |
| POST | Create data |
| PUT | Replace entire data |
| PATCH | Update specific fields |
| DELETE | Remove data |

---



# 📋 Query Parameters

Example:

```http
GET {{baseUrl}}/posts?id=685f123abc
```

or using Params tab:

| Key | Value |
|------|------|
| id | 685f123abc |

Backend:

```js
req.query.id
```

---

# 📌 Route Parameters

URL:

```http
GET {{baseUrl}}/posts/685f123abc
```

Backend:

```js
req.params.id
```

Express:

```js
app.get("/posts/:id", (req,res)=>{
    console.log(req.params.id);
});
```

---

# 🍪 Cookies

After login, cookies can be viewed from:

```
Cookies → localhost
```

Example response:

```js
res.cookie("token", token);
```

---

# 🔐 Authorization

For JWT authentication:

1. Open **Authorization** tab.
2. Select:

```
Bearer Token
```

Enter:

```
eyJhbGc...
```

Header generated:

```http
Authorization: Bearer eyJhbGc...
```

---

# 📂 Example Project Structure

```
Workspace
│
├── User Collection
│      ├── Register
│      ├── Login
│      └── Profile
│
├── Owner Collection
│      ├── Create Owner
│      └── Get Owner
│
├── Product Collection
│      ├── Add Product
│      ├── Get Products
│      ├── Update Product
│      └── Delete Product
│
├── Cart Collection
│
└── Order Collection
```

---

# ⚡ Recommended Workflow

### 1. Create Workspace

```
E-Commerce Backend
```

### 2. Create Environment

```
Local Development
```

Variables:

```text
baseUrl = http://localhost:3000
```

### 3. Create Collections

```
Users
Owners
Products
Cart
Orders
```

### 4. Add Requests

Examples:

```http
POST {{baseUrl}}/users/register

POST {{baseUrl}}/users/login

GET {{baseUrl}}/products

POST {{baseUrl}}/products/create

DELETE {{baseUrl}}/products/:id
```

---

# ⭐ Best Practices

✅ Create separate collections for each module.

✅ Use `{{baseUrl}}` variable instead of hardcoding URLs.

✅ Give meaningful request names.

✅ Save example responses.

✅ Keep authentication tokens in environment variables.

✅ Group requests logically.

✅ Test all CRUD operations.

---

# Example

```text
Workspace
│
└── E-Commerce Backend
      │
      ├── Users
      │     ├── Register
      │     ├── Login
      │     └── Profile
      │
      ├── Owners
      │     ├── Create Owner
      │     └── Get Owner
      │
      └── Products
            ├── Create Product
            ├── Get Products
            ├── Update Product
            └── Delete Product
```

Happy API Testing 🚀