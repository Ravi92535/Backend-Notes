

forEach() is a built-in method specifically for Arrays. It also works on a few other specialized iterable collections in JavaScript, such as Maps, Sets, and NodeLists (when working with the DOM).


```javascript

var obj = { a: 1, b: 2, c: 3 };

// This will throw an error: "TypeError: obj.forEach is not a function"
obj.forEach(function(val) {
    console.log(val);
});

// Correct Way:- 
var obj = { a: 1, b: 2, c: 3 };

for (var key in obj) {
    console.log(obj[key] + " hello");
}
```



```javascript

var fruits = ["apple", "banana", "cherry"];

fruits.forEach(function(fruit) {
    console.log(fruit);
});


```


### The Analogy
Imagine you have a box of plain cookies. You want to add chocolate chips to all of them, but you want to put these new chocolate chip cookies into a **brand new box**, leaving the original plain cookies completely untouched. 

That is exactly what `.map()` does. It looks at every item in a list, changes it based on a rule you give it, and spits out a brand new list.

### Breaking down your code:

1. **`var arr = [1, 2, 3, 4];`**
   This is your original box. It holds four numbers.



```javascript

var newarr = arr.map(function(val) {
    return val * 3;
})

```
2. **`var newarr = arr.map(...)`**
   You are creating a new box called `newarr`. You use `.map()` to tell the computer: "Go through every single item in my original `arr`, do some math to it, and put the result in this `newarr`."

3. **`function(val) { return val * 3; }`**
   These are the instructions you give to `.map()`. 
   * `val` is just a temporary nickname for whatever number the computer is currently looking at. 
   * `return val * 3;` tells the computer exactly what to do: "Take the number you are looking at, multiply it by 3, and toss the result into the new box."
   
   *Step-by-step:*
   * It looks at `1`, multiplies by 3, and puts `3` in the new box.
   * It looks at `2`, multiplies by 3, and puts `6` in the new box.
   * It looks at `3`, multiplies by 3, and puts `9` in the new box.
   * It looks at `4`, multiplies by 3, and puts `12` in the new box.

4. **`console.log(newarr);`**
   When you look inside your new box, you will see `[3, 6, 9, 12]`. And because `.map()` is safe, your original `arr` is still `[1, 2, 3, 4]`!

```javascript


```


# JavaScript `filter()` Method

The `filter()` method creates a **new array** containing only the elements that satisfy a given condition.

## Syntax

```javascript
array.filter(callback(element, index, array))
```

* `element` → Current element being processed.
* `index` *(optional)* → Index of the current element.
* `array` *(optional)* → The original array.

The callback should return:

* `true` → Keep the element.
* `false` → Remove the element.

---

## Example 1: Filter Even Numbers

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const evenNumbers = numbers.filter(num => num % 2 === 0);

console.log(evenNumbers);
```

### Output

```javascript
[2, 4, 6]
```

---

## Example 2: Filter Objects

```javascript
const users = [
    { name: "Ravi", active: true },
    { name: "Aman", active: false },
    { name: "Amit", active: true }
];

const activeUsers = users.filter(user => user.active);

console.log(activeUsers);
```

### Output

```javascript
[
  { name: "Ravi", active: true },
  { name: "Amit", active: true }
]
```

---

## Important Points

* `filter()` does **not modify** the original array.
* It returns a **new array**.
* The callback function should return a boolean value.
* If no element satisfies the condition, an empty array is returned.

---

## How `filter()` Works Internally

```javascript
const arr = [1, 2, 3, 4];
const result = [];

for (let i = 0; i < arr.length; i++) {
    if (arr[i] % 2 === 0) {
        result.push(arr[i]);
    }
}

console.log(result);
```

-----------------------------------------------------------




# Async  Concept

`async` does **not** create a new thread.

An `async` function:

1. Runs synchronously until the first `await`.
2. At `await`, it pauses itself.
3. The main thread continues executing other code.
4. When the awaited promise resolves, the remaining part of the function is put into the **microtask queue**.
5. The event loop executes that continuation when the call stack becomes empty.

Example:

```javascript
async function foo() {
    console.log(1);

    await Promise.resolve();

    console.log(2);
}

console.log(0);
foo();
console.log(3);
```

Output:

```
0
1
3
2
```

because the code after `await` is executed later through the microtask queue.

1.Pauses foo().
2.Returns control to the main thread.
3.Executes remaining synchronous code (console.log(3)).
4.When the promise resolves, the rest of foo() (console.log(2)) is placed in the microtask queue.
5.Event loop runs that microtask.

-----------------------------------------------------------

# Queue


| Queue                          | Examples                                                              | Priority |
| ------------------------------ | --------------------------------------------------------------------- | -------- |
| **Microtask Queue**            | `Promise.then()`, `catch()`, `finally()`, `await`, `queueMicrotask()` | Higher   |
| **Macrotask (Callback) Queue** | `setTimeout()`, `setInterval()`, I/O, UI events                       | Lower    |



-----------------------------------------------------------
