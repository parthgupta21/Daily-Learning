````markdown
# Asynchronous JavaScript: Callbacks  

## 1. What Does “Asynchronous” Mean in JavaScript

JavaScript is a **single-threaded** language.  
This means JavaScript can execute **only one task at a time** using a single call stack.

However, real applications must handle:
- Timers
- User interactions
- Network requests
- File operations

These tasks take time and **cannot block the main thread**.

**Asynchronous programming** allows JavaScript to start a task and continue executing other code while waiting for that task to finish.

---

## 2. What Is a Callback

### Definition

A **callback** is a **function passed as an argument to another function**, which is **called later** after some operation is completed.

**Key idea:**  
The callback represents **“what to do next”**.

---

### Simple Example

```javascript
function greet(name, callback) {
  console.log("Hello " + name);
  callback();
}

greet("Alice", function () {
  console.log("Welcome!");
});
````

Here:

* The function passed is the **callback**
* It is executed **after** `greet` finishes its main work

---

## 3. Why Callbacks Are Needed in JavaScript

Because JavaScript is single-threaded:

* Long operations cannot block execution
* Slow tasks are handled by the **runtime environment** (browser or Node.js)

JavaScript says to the runtime:

> “I will continue executing.
> Call this function when you are done.”

That function is the **callback**.

---

## 4. Callback with Asynchronous Behavior

### Example: setTimeout

```javascript
console.log("Start");

setTimeout(function () {
  console.log("Timer finished");
}, 1000);

console.log("End");
```

### Output

```
Start
End
Timer finished
```

### Explanation

* `setTimeout` is handled by the runtime
* JavaScript does **not wait**
* The callback runs **after the call stack is empty**
* This is coordinated by the **event loop**

---

## 5. Important Interview Point

**Callbacks do not make code asynchronous by default.**

This is **synchronous**, even though it uses a callback:

```javascript
function run(callback) {
  callback();
}

run(() => console.log("Runs immediately"));
```

**Asynchronous behavior depends on who calls the callback and when.**

---

## 6. Callbacks and Events

Callbacks are heavily used in **event handling**.

### Example: Click Event

```javascript
button.addEventListener("click", function () {
  console.log("Button clicked");
});
```

Explanation:

* The callback is registered
* JavaScript finishes execution
* When the event occurs, the callback is queued
* The event loop schedules it

This is why JavaScript is called **event-driven**.

---

## 7. Callbacks in Real-World Asynchronous Tasks

Callbacks are used in:

* API calls
* File reading
* Database queries

### Example

```javascript
getUserData(function (user) {
  console.log(user);
});
```

Meaning:

* Start fetching data
* When data is ready, execute the callback

---

## 8. Callback Hell

When callbacks are nested inside other callbacks, code becomes difficult to read and maintain.

### Example

```javascript
getUser(function (user) {
  getOrders(user.id, function (orders) {
    getDetails(orders[0].id, function (details) {
      console.log(details);
    });
  });
});
```

Problems:

* Deep nesting
* Hard to understand execution flow
* Difficult error handling

This situation is called **callback hell** or **pyramid of doom**.

---

## 9. Inversion of Control

When using callbacks, you **give control** of your function to another function.

You trust that:

* The callback will be called
* It will be called only once
* It will be called at the right time

If this trust breaks, bugs occur.

This is a major reason why **Promises** were introduced.

---

## 10. Error Handling in Callbacks

Callbacks often handle errors manually.

### Node.js Style (Error-First Callback)

```javascript
readFile("file.txt", function (err, data) {
  if (err) {
    console.log("Error:", err);
    return;
  }
  console.log(data);
});
```

Convention:

* First argument is the error
* If error exists, handle it first

---

## 11. Relationship Between Callbacks, Promises, and async/await

* Callbacks are the **foundation**
* Promises are built **on top of callbacks**
* `async/await` is syntax sugar over promises

Understanding callbacks makes it easier to understand:

* Event loop
* Promises
* async/await

---

## 12. Key Interview Statements to Remember

* JavaScript is single-threaded but asynchronous
* Callbacks represent “what to do next”
* Callbacks are executed by the event loop
* Not all callbacks are asynchronous
* Callback hell is a readability problem
* Promises solve callback trust issues


