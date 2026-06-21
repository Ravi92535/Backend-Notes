Let's compare both cases carefully.

---

# Case 1: **Without Native Addon (`spawn()`)**

```text
Browser
   ↓
Node.js (JS)
   ↓
OS Pipe (stdin/stdout)
   ↓
Stockfish Process (C++)
```

Suppose you do:

```js
engine.stdin.write("go depth 15\n");
```

### What happens internally?

1. JS calls `engine.stdin.write()`.
2. Node's C++ runtime passes bytes to the OS.
3. The OS sends them to another process (Stockfish).
4. Stockfish computes for 5 seconds.
5. Meanwhile, Node's event loop is FREE.

During those 5 seconds:

```text
Node Event Loop
├── Serve HTTP requests
├── Handle Socket.IO events
├── Chat messages
├── Other games
└── Waiting for Stockfish response
```

When Stockfish finishes:

```text
bestmove e7e5
```

Node gets notified:

```js
engine.stdout.on("data", data => {
    socket.emit("move", data.toString());
});
```

### Timeline

```text
t=0    send "go depth 15"
t=0-5s Stockfish thinking
        Node is free
t=5s   Stockfish returns bestmove
```

---

# Case 2: **With Native Addon**

Architecture:

```text
Browser
   ↓
Node.js
   ↓
N-API
   ↓
Chess Engine C++
```

Suppose:

```js
const move = engine.bestMove(fen);
```

### Internally

1. JS enters C++.
2. Wrapper converts JS string → C++ string.
3. `engine.bestMove()` starts searching.
4. CPU spends 5 seconds searching.
5. Result returns to JS.

### Problem

Node's main thread is inside C++.

During these 5 seconds:

```text
Node Event Loop
❌ Cannot process requests
❌ Cannot process Socket.IO
❌ Cannot handle other games
❌ Everything waits
```

Timeline:

```text
t=0    engine.bestMove()
t=0-5s Node completely blocked
t=5s   result returns
t=5.1s pending requests execute
```

---

# Visual Comparison

### Without Addon (Separate Process)

```text
Node Process                Engine Process

Socket.IO      ✓
HTTP           ✓
Other Games    ✓
               -------->
                Thinking 5 sec
               <--------
Result arrives ✓
```

Node remains responsive.

---

### With Addon (Same Process)

```text
Node Process

Socket.IO      ✗
HTTP           ✗
Other Games    ✗
Engine Search  ✓ (5 sec)
Result         ✓
```

Everything freezes while searching.

---

# Why?

Node has one main thread:

```text
JS
↓
V8
↓
N-API Wrapper
↓
Chess Engine
```

When C++ is executing:

```cpp
bestMove(fen);
```

control hasn't returned to JS, so the event loop can't continue.

---

# Is Addon Faster?

Yes.

### Spawn()

```text
JS
↓
Node C++
↓
OS
↓
Pipe
↓
Another Process
↓
Engine
```

There is IPC overhead (microseconds).

---

### Addon

```text
JS
↓
N-API
↓
Engine
```

Almost like a normal function call.

Overhead is extremely small.

---

# So which should you use?

### Small calculations (<10 ms)

Native addon is excellent.

Example:

```cpp
evaluate(fen)
```

or

```cpp
generateLegalMoves()
```

---

### Heavy searches (1–10 sec)

Separate process (`spawn`) is usually better because:

* Engine crashes won't crash Node.
* Node remains responsive.
* Multiple engines can run simultaneously.
* Easier to scale.

---

# Best of Both Worlds

Use a worker thread:

```text
Main Thread
    ↓
Worker Thread
    ↓
N-API
    ↓
C++ Engine
```

Now:

```text
Main Event Loop
✓ Socket.IO
✓ HTTP Requests
✓ Other Games

Worker Thread
✓ Search for 5 sec
```

This gives:

* No IPC overhead.
* Node remains responsive.
* Maximum performance.

This is how many high-performance systems integrate C++ with Node.js.
