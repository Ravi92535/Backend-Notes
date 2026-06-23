# ⚙️ Config Package

The `config` package is used to store application settings separately from code.

## Installation

```bash
npm install config
```

---

## Folder Structure

```
project/
│
├── config/
│     ├── development.json
│     ├── production.json
│     └── default.json
│
├── app.js
└── package.json
```

---

## Example

### development.json

```json
{
  "MONGODB_URI": "mongodb://localhost:27017/shopify"
}
```

### production.json

```json
{
  "MONGODB_URI": "mongodb+srv://username:password@cluster.mongodb.net/shopify"
}
```

---

## Using Config

```js
const config = require("config");

const dbURI = config.get("MONGODB_URI");

console.log(dbURI);
```

---

## How Config Works

The package checks:

```js
process.env.NODE_ENV
```

If:

```bash
NODE_ENV=development
```

it loads:

```
config/development.json
```

If:

```bash
NODE_ENV=production
```

it loads:

```
config/production.json
```

If `NODE_ENV` is not set, it uses:

```
config/default.json
```

---

## Advantages

✅ Keeps sensitive data separate from code.

✅ Easy environment switching.

✅ No hardcoded URLs or keys.

✅ Better project organization.

✅ Supports development, testing, and production configurations.

---

## Common Usage

```js
const config = require("config");

mongoose.connect(config.get("MONGODB_URI"));

const jwtSecret = config.get("JWT_SECRET");

const port = config.get("PORT");
```

---

## Summary

`config` automatically loads configuration values based on the current environment (`development`, `production`, etc.), allowing the same code to run with different settings.