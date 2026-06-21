# Authentication Basics with Express.js

## 1. Cookies 🍪

Cookies are small pieces of data stored in the browser.

### Set Cookie

```js
res.cookie("name", "Ravi")
```

### Read Cookie

```js
req.cookies.name
```

Use `cookie-parser` to read cookies.

```js
const cookieParser = require("cookie-parser")
app.use(cookieParser())
```

---

## 2. bcrypt 🔐

bcrypt is used to securely store passwords.

### Hash Password

```js
const bcrypt = require("bcrypt")

const hash = await bcrypt.hash("mypassword", 10)
```

Store `hash` in the database instead of the original password.

### Compare Password

```js
const result = await bcrypt.compare(
    "mypassword",
    hash
)
```

Returns:

* `true` → Password is correct.
* `false` → Password is wrong.

---

## 3. JWT (JSON Web Token) 🪪

JWT is used for authentication.

### Create Token

```js
const jwt = require("jsonwebtoken")

const token = jwt.sign(
    { id: user._id },
    "secretKey"
)
```

Store the token in a cookie:

```js
res.cookie("token", token)
```

---

### Verify Token

```js
const data = jwt.verify(
    token,
    "secretKey"
)
```

After verification, you get the data stored inside the token.

```js
{
    id: "68745abc..."
}
```

---

# Combined 
```js
app.get("/", async (req,res)=>{
    const password= "Hellow";
    const hashValue = await bcrypt.hash(password, 10);
    console.log(hashValue)
    const token = jwt.sign({ hash: hashValue, name: "Ravi" }, "secretkey",{expiresIn:"15s"}) 
    res.cookie("token", token);
    res.send("Hellow Im done");
})

use Try Catch for jwt bcz it throw error if token is expired or invalid


app.get("/info/:password", async (req, res) => {
    const password = req.params.password;

    try {
        const token = req.cookies.token;

        const decodedToken = jwt.verify(token, "secretkey");
        console.log(decodedToken)

        const isMatch = await bcrypt.compare(password, decodedToken.hash);

        if (isMatch) {
            res.send("Password is correct");
        } else {
            res.send("Password is incorrect");
        }
    }
    catch (err) {
        res.send("Invalid token");
    }
});
```

# Complete Flow

## Signup

```text
User Signup
      ↓
Password hashed using bcrypt
      ↓
Hash stored in MongoDB
```

## Login

```text
User enters password
      ↓
bcrypt.compare()
      ↓
Password matched
      ↓
JWT created
      ↓
Token stored in cookie
```

## Protected Route

```text
Request comes
      ↓
Read cookie
      ↓
Extract JWT
      ↓
jwt.verify()
      ↓
User authenticated
      ↓
Send response
```

---

## Packages Used

```bash
npm install bcrypt
npm install jsonwebtoken
npm install cookie-parser
```
