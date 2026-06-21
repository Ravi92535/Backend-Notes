# Cookies and Sessions

Cookies and sessions are used to maintain **state** between multiple HTTP requests because **HTTP is stateless**.

---

# What is a Cookie?

A **cookie** is a small piece of data stored in the **client's browser** and sent automatically with every request to the server.

### Flow

```text
Server
   ↓
Sets Cookie
   ↓
Browser Stores Cookie
   ↓
Future Requests Automatically Include Cookie
```

### Example

Server sends:

```http
Set-Cookie: username=ravindra
```

Browser stores:

```text
username=ravindra
```

On subsequent requests, the browser automatically sends:

```http
Cookie: username=ravindra
```

### Uses of Cookies

- Remembering users
- Storing preferences (theme, language)
- Authentication tokens
- Session IDs

---

# What is a Session?

A **session** is server-side storage that keeps information about a user across multiple requests.

Instead of storing all user data in the browser, the server stores it and gives the browser a **session ID** (usually via a cookie).

### Flow

```text
User Logs In
      ↓
Server Creates Session
      ↓
Server Stores User Data
      ↓
Server Sends Session ID Cookie
      ↓
Browser Stores Session ID
      ↓
Future Requests Send Session ID
      ↓
Server Finds Session Data
```

---

## Example

Server stores:

```js
Session ID: abc123

{
    userId: 101,
    username: "ravindra",
    role: "admin"
}
```

Browser only stores:

```text
sessionId=abc123
```

---

# Cookie vs Session

| Feature | Cookie | Session |
|----------|---------|---------|
| Stored On | Client (Browser) | Server |
| Size Limit | ~4 KB | Depends on server memory |
| Security | Less secure | More secure |
| Data Stored | Actual data | Session ID |
| Lifetime | Can persist after browser closes | Usually expires after inactivity |

---

# Express Example

```js
const session = require("express-session");

app.use(
    session({
        secret: "mySecret",
        resave: false,
        saveUninitialized: true
    })
);
```

Set data:

```js
req.session.username = "ravindra";
```

Access data:

```js
console.log(req.session.username);
```

---

# Real-Life Analogy 🎫

Imagine a movie theater:

### Cookie 🍪

You carry all your information yourself:

```text
Name: Ravindra
Seat: A12
Movie: Avengers
```

Anyone who sees the ticket can read it.

---

### Session 🎫

You only carry a ticket number:

```text
Ticket Number: 12345
```

The theater's computer stores:

```text
12345
↓
Name: Ravindra
Seat: A12
Movie: Avengers
```

The ticket itself contains very little information, making it safer.

---

# Key Point

> **Cookie = data stored in the browser.**
>
> **Session = data stored on the server, with the browser storing only a session ID.**