# MongoDB with Express and Mongoose

## 1. Create a Model

Define a schema and create a model.

```js
const userSchema = new mongoose.Schema({
    name: String,
    age: Number,
    email: String
})

const User = mongoose.model("User", userSchema)

module.exports = User
```

The model acts as a bridge between Node.js and the MongoDB collection.

---

## 2. Import the Model

```js
const User = require("./usermodel")
```

Now the model can be used inside routes.

---

## 3. Perform CRUD Operations

### Create

```js
await User.create({
    name: "Ravi",
    age: 20,
    email: "ravi@gmail.com"
})
```

### Read All

```js
await User.find()
```

### Read One

```js
await User.findById(req.params.id)
```

### Update

```js
await User.updateOne(
    { _id: req.params.id },
    { age: 21 }
)
```

### Delete

```js
await User.deleteOne({ _id: req.params.id })
```

---

## Flow

```
Client
   ↓
Express Route
   ↓
Mongoose Model
   ↓
MongoDB Collection
   ↓
Response
```

Routes receive requests, models perform database operations, and the results are sent back to the client.
