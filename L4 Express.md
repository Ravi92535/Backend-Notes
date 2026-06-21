# What is Express.js

Express.js is a **fast, lightweight web framework** built on top of **Node.js**. At its core, it is an **npm package** that makes building web servers and APIs easy and efficient.

---

## The Restaurant Waiter Analogy 🍽️

Think of **Express** as a highly efficient waiter in a restaurant.

1. A customer (**browser/frontend**) places an order (**Request**).
2. Express receives the order and takes it to the kitchen (**server/database**).
3. The kitchen prepares the food.
4. Express brings the prepared food back to the customer (**Response**).

```javascript
app.get(
    "/dashboard",
    authenticate,       // middleware
    authorizeAdmin,     // middleware
    (req, res) => {     // route handler
        res.send("Dashboard");
    }
);

```

## almost same as function

```javascript
function logger(req, res, next) {
    console.log("Request received");
    next();
}

app.use(logger);
```
## Flow 
```text
Browser (Client)
       |
    Request
       ↓
   Express.js
       |
 Server / Database
       |
    Response
       ↓
Browser (Client)
```

```javascript
// Error Handler Middleware
app.use((err, req, res, next) => {
    console.error(err.message);

    res.status(500).json({
        message: err.message
    });
})

```
---

## Middleware (The Helpers)

Middleware is a function that runs between receiving a request and sending a response.
It can inspect, modify, or stop the request/response, or pass control to the next function.
            
            or

Express uses **middleware**, which are functions that sit between the **request** and **response** cycle.

Think of middleware as **checkpoints or security guards**.

For example:

- Check whether a user is logged in.
- Parse JSON data from requests.
- Log incoming requests.
- Handle errors.

```text
Request
   ↓
Middleware 1 (Logging)
   ↓
Middleware 2 (Authentication)
   ↓
Route Handler
   ↓
Response
```

Example:

```js
app.use((req, res, next) => {
    console.log("Request received");
    next(); // Pass control to the next middleware
});
```

---

# What is a Framework?

A **framework** is a structured set of tools, rules, and reusable components that helps developers solve problems and build applications efficiently.

A framework:

- Provides an overall structure.
- Defines where your code fits.
- Handles much of the boilerplate code.
- Follows the principle of **Inversion of Control** (the framework calls your code).

Examples:

- Express.js
- React
- Django
- Spring Boot

---

# What is a Library?

A **library** is a collection of prewritten code that you can use to perform specific tasks.

You decide:

- When to use it.
- Which functions to call.
- How to organize your application.

Examples:

- Lodash
- Axios
- NumPy
- jQuery

---

# Simple Analogy

## Library = Toolbox 🧰

You pick the tools you need and decide how to use them.

```text
Your Program
     ↓
Uses
     ↓
Library
```

**You are in control.**

---

## Framework = House Blueprint 🏠

The framework provides the overall structure and tells your code where it should fit.

```text
Framework
     ↓
Calls
     ↓
Your Code
```

**The framework is in control.**

---

# What are Routes in Express.js?

A **route** defines how the server responds to a particular **URL (path)** and **HTTP method**.

In simple terms, a route tells Express:

> "If a request comes to this URL with this method, run this function."

---

## Real-Life Analogy 🚗

Think of routes as **roads leading to different destinations**.

- `/` → Home page
- `/about` → About page
- `/products` → Products page
- `/login` → Login page

When a user requests a specific URL, Express follows the corresponding route and sends back a response.

---

## Basic Syntax

```js
app.METHOD(PATH, HANDLER);
```

Where:

- **METHOD** → HTTP method (`GET`, `POST`, `PUT`, `DELETE`, etc.)
- **PATH** → URL path
- **HANDLER** → Function that handles the request and sends a response

---

## Example

```js
app.get("/", (req, res) => {
    res.send("Welcome to Home Page");
});

app.get("/about", (req, res) => {
    res.send("About Us");
});
```

If a user visits:

```
http://localhost:3000/
```

Response:

```
Welcome to Home Page
```



# Express Routing Flow

```text
Client (Browser)
       |
       | GET /about
       ↓
    Express
       |
       | Matches route "/about"
       ↓
 Route Handler
       |
       ↓
 Response
       |
       ↓
Client
```

---

# Key Point

> A route is a mapping between an HTTP request (method + URL) and the function that handles that request.

```text
GET    /users      → Get all users
GET    /users/:id  → Get one user
POST   /users      → Create user
PUT    /users/:id  → Update user
DELETE /users/:id  → Delete user
```