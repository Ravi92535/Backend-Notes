# 👤 User Roles: Admin vs Normal User

## 1. Add a Role to User Schema

```js
const userSchema = new mongoose.Schema({
    fullname: String,
    email: String,
    password: String,
    role: {
        type: String,
        enum: ["user", "admin"],
        default: "user"
    }
});
```

---

## 2. Create an Admin

```js
await User.create({
    fullname: "Ravi",
    email: "ravi@gmail.com",
    password: hashedPassword,
    role: "admin"
});
```

Normal users:

```js
await User.create({
    fullname: "Ram",
    email: "ram@gmail.com",
    password: hashedPassword
});
```

Since no role is specified, it defaults to:

```js
role: "user"
```

---

## 3. Include Role in JWT

During login:

```js
const token = jwt.sign(
{
    userId: user._id,
    role: user.role
},
"secretkey"
);
```

---

## 4. Authentication Middleware

```js
function isLoggedIn(req, res, next) {

    const token = req.cookies.token;

    if (!token)
        return res.status(401).send("Login first");

    const decoded = jwt.verify(token, "secretkey");

    req.user = decoded;

    next();
}
```

---

## 5. Admin Middleware

```js
function isAdmin(req, res, next) {

    if (req.user.role !== "admin") {
        return res.status(403).send("Access denied");
    }

    next();
}
```

---

## 6. Protect Routes

Admin only:

```js
app.post("/product/create",
    isLoggedIn,
    isAdmin,
    createProduct
);
```

Normal users can access:

```js
app.get("/products",
    isLoggedIn,
    getProducts
);
```

---

## Flow

Login
↓
JWT contains:

{
    userId: "...",
    role: "admin"
}

↓
isLoggedIn()

↓
req.user.role

↓
isAdmin()

↓
Allow or deny access