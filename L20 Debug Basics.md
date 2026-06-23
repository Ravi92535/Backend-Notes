# 🐞 Using the `debug` Package

The `debug` package is used for conditional logging. Logs are printed only when the corresponding debug namespace is enabled.

---

## 1. Install Debug

```bash
npm install debug
```

---

## 2. Create a Debugger

In the file where you want logs:

```js
const dbgr = require("debug")("development:owner");
```

Here:

- `development` → Environment name
- `owner` → Module name

---

## 3. Use the Debugger

```js
dbgr("Owner creation started");

dbgr("Owner already exists");

dbgr("Error creating owner");
```

Unlike `console.log()`, these messages are shown only when debugging is enabled.

---

## 4. Enable Debugging

Go to the **project root folder** and set the `DEBUG` environment variable.

### Mac/Linux

```bash
export DEBUG=development:*
```

Then start the application:

```bash
npm start
```

or

```bash
node app.js
```

---

### Windows CMD

```cmd
set DEBUG=development:*
npm start
```

---

## 5. Enable Specific Modules

Only owner logs:

```bash
export DEBUG=development:owner
```

Multiple modules:

```bash
export DEBUG=development:owner,development:user
```

All development modules:

```bash
export DEBUG=development:*
```

---

## Example

```js
const dbgr = require("debug")("development:owner");

dbgr("Owner creation started");

router.post("/create", async (req, res) => {
    dbgr("Request received");
});
```

Output:

```
development:owner Owner creation started +0ms
development:owner Request received +15ms
```

---

## Benefits

✅ Better than `console.log()`

✅ Can enable/disable logs without changing code

✅ Module-wise logging

✅ Useful for development and debugging

---

## Summary

1. Install:

```bash
npm install debug
```

2. Create debugger:

```js
const dbgr = require("debug")("development:owner");
```

3. Write logs:

```js
dbgr("Message");
```

4. In the project root, enable debugging:

```bash
export DEBUG=development:*
```

5. Run the app:

```bash
npm start
```