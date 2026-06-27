# Socket.IO Basics

## What is Socket.IO?

Socket.IO is a library that enables **real-time, bidirectional communication** between clients and servers.

Unlike normal HTTP requests:

```
Client ----Request----> Server
Client <---Response---- Server
```

Socket.IO keeps a **persistent connection** open:

```
Client <=====> Server
```

Both client and server can send messages anytime.

---

# Why use Socket.IO?

Common use cases:

- Chat applications
- Multiplayer games
- Live notifications
- Collaborative editors
- Live dashboards
- Stock market updates

---

# Basic Architecture

```
Browser (Client)
      |
      |
   Socket.IO
      |
      |
Node.js Server
```

---

# Installation

### Server

```bash
npm install socket.io
```

### Client

```bash
npm install socket.io-client
```

---

# Creating a Socket.IO Server

```javascript
const express = require("express");
const http = require("http");
const { Server } = require("socket.io");

const app = express();

const server = http.createServer(app);

const io = new Server(server);

server.listen(3000);
```

---

# Client Connection

```javascript
import { io } from "socket.io-client";

const socket = io("http://localhost:3000");
```

---

# Connection Flow

When client connects:

```javascript
io.on("connection", (socket) => {
    console.log("User Connected");
});
```

Every connected user gets a unique socket object.

---

# Socket ID

Each connection has a unique id.

```javascript
io.on("connection", (socket) => {
    console.log(socket.id);
});
```

Example:

```
aBc123
xYz456
pQr789
```

Useful for:

- Identifying users
- Sending private messages
- Tracking connections

---

# Events

Socket.IO works using events.

### Client → Server

Client:

```javascript
socket.emit("message", "Hello Server");
```

Server:

```javascript
socket.on("message", (data) => {
    console.log(data);
});
```

---

### Server → Client

Server:

```javascript
socket.emit("welcome", "Hello Client");
```

Client:

```javascript
socket.on("welcome", (msg) => {
    console.log(msg);
});
```

---

# emit()

Used to send data.

```javascript
socket.emit("eventName", data);
```

Example:

```javascript
socket.emit("chat-message", {
    user: "Ravi",
    text: "Hello"
});
```

---

# on()

Used to listen for data.

```javascript
socket.on("eventName", callback);
```

Example:

```javascript
socket.on("chat-message", (msg) => {
    console.log(msg);
});
```

---

# Broadcasting

Send message to everyone except sender.

```javascript
socket.broadcast.emit("new-user", "Ravi joined");
```

Example:

```
User A sends event

User B receives
User C receives

User A does NOT receive
```

---

# io.emit()

Send to ALL clients.

```javascript
io.emit("message", "Hello Everyone");
```

Example:

```
User A receives
User B receives
User C receives
```

Including sender.

---

# socket.emit() vs io.emit()

### socket.emit()

Only current socket receives.

```javascript
socket.emit("hello");
```

```
Sender ✓
Others ✗
```

### io.emit()

Everyone receives.

```javascript
io.emit("hello");
```

```
Sender ✓
Others ✓
```

---

# Rooms

Rooms are groups of sockets.

Useful for:

- Game rooms
- Chat rooms
- Private groups

---

## Join Room

```javascript
socket.join("room1");
```

---

## Emit to Room

```javascript
io.to("room1").emit("message", "Hello Room");
```

Only users inside room1 receive.

---

# Example

```javascript
socket.join("chess-123");

io.to("chess-123").emit(
    "move-made",
    moveData
);
```

Only players in that game receive updates.

---

# Leaving Room

```javascript
socket.leave("room1");
```

---

# Multiple Rooms

A socket can join many rooms.

```javascript
socket.join("room1");
socket.join("room2");
socket.join("room3");
```

---

# Disconnect Event

When user closes tab or loses connection.

```javascript
socket.on("disconnect", () => {
    console.log("User Disconnected");
});
```

---

# Acknowledgements (Callback)

Client asks something and waits for response.

Client:

```javascript
socket.emit(
    "create-room",
    data,
    (response) => {
        console.log(response);
    }
);
```

Server:

```javascript
socket.on(
    "create-room",
    (data, callback) => {

        callback({
            success: true
        });
    }
);
```

---

# Namespaces

Separate communication channels.

```javascript
const admin = io.of("/admin");
```

Client:

```javascript
const socket =
    io("http://localhost:3000/admin");
```

Useful when:

- Admin panel
- User panel
- Analytics panel

---

# Middleware

Run logic before connection.

```javascript
io.use((socket, next) => {

    const token =
        socket.handshake.auth.token;

    if (!token) {
        return next(new Error("Unauthorized"));
    }

    next();
});
```

Useful for:

- JWT authentication
- Permission checks

---

# Handshake

Data sent during connection.

Client:

```javascript
const socket = io(
    "http://localhost:3000",
    {
        auth: {
            token: "abc123"
        }
    }
);
```

Server:

```javascript
io.on("connection", (socket) => {

    console.log(
        socket.handshake.auth.token
    );

});
```

---

# Common Methods

| Method | Purpose |
|----------|----------|
| socket.emit() | Send event |
| socket.on() | Listen event |
| socket.join() | Join room |
| socket.leave() | Leave room |
| io.emit() | Send to all |
| io.to(room).emit() | Send to room |
| socket.broadcast.emit() | Send to everyone except sender |
| socket.disconnect() | Disconnect user |

---

# Socket.IO in a Chess Game

### Create Game

Player A:

```javascript
socket.emit("create-game");
```

Server:

```javascript
socket.join(gameId);
```

---

### Join Game

Player B:

```javascript
socket.emit("join-game", gameId);
```

Server:

```javascript
socket.join(gameId);
```

---

### Make Move

Player:

```javascript
socket.emit("move", move);
```

Server:

```javascript
io.to(gameId).emit(
    "move-made",
    move
);
```

Both players get update instantly.

---

# Mental Model

Think of Socket.IO as:

```
HTTP
-----
Request → Response → Connection Ends


Socket.IO
---------
Connect Once
↓
Send Events Anytime
↓
Receive Events Anytime
↓
Disconnect
```

This is the core idea behind most real-time systems such as WhatsApp, Discord, multiplayer games, and live collaboration apps.

```js
io.on("connection", (socket) => {

    socket.data.userId = "123";

    socket.on("move", () => {
        if(socekt.data.userId !== game.turn)  socket.emit("Not Your Turn");
        else // perform business logic
        console.log(socket.data.userId);

    });
});


socket.data is the recommended place for custom data

```


```js

io.use(async (socket, next) => {

    const token = socket.handshake.auth.token;

    const user = await verifyToken(token);

    socket.data.user = user;

    next();
});

io.on("connection", (socket) => {

    console.log(socket.data.user);

});

```

# Can Client Access socket.data? -> NOOOOOO. This exists only on the server.
## socket info is stored in the Node.js server memory.

```
Server Memory
┌─────────────┐
│ socket.data │
└─────────────┘

Client
┌─────────────┐
│ No Access   │
└─────────────┘

```


