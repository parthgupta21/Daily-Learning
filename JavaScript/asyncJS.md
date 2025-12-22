## 1. Why Asynchronous JavaScript Exists

### 1.1 JavaScript Execution Model

**JavaScript is single-threaded.**

**Definition**
A *single-threaded language* is a programming language that can execute only one piece of code at a time on a single call stack.

This means:

* JavaScript cannot execute two functions simultaneously.
* Every operation must finish before the next one starts.

If JavaScript were purely synchronous, the following would be true:

* A long-running operation would block everything.
* The browser UI would freeze.
* Servers written in JavaScript would be unusable under load.

---

### 1.2 Blocking vs Non-Blocking Code

**Blocking Code**
Blocking code prevents further execution until the current task completes.

Example:

```js
while (true) {}
```

This blocks the thread forever.

**Non-Blocking Code**
Non-blocking code allows JavaScript to continue executing while waiting for an operation to complete.

Asynchronous JavaScript enables **non-blocking behavior**.

---

## 2. What “Asynchronous” Means

**Definition**
Asynchronous execution is a programming model where an operation is initiated, and the program continues executing without waiting for that operation to complete. When the operation finishes, a predefined function is executed.

Key idea:

> “Start now, finish later, notify me when done.”

---

## 3. What Is a Callback?

### 3.1 Definition of a Callback

**Definition**
A callback is a function that is passed as an argument to another function, with the intention that it will be invoked later, usually after an asynchronous operation completes.

Formally:

* A callback is not executed immediately.
* It is executed at a later time.
* The execution is controlled by another function or system.

---

### 3.2 Simple Synchronous Callback

Callbacks are not inherently asynchronous.

Example:

```js
function greet(name, callback) {
  callback(name);
}

greet("Alice", function(name) {
  console.log("Hello " + name);
});
```

This is a **synchronous callback**.

The callback:

* Is executed immediately.
* Does not involve any delay or external system.

---

### 3.3 Asynchronous Callback

An **asynchronous callback** is executed after an asynchronous task finishes.

Example:

```js
setTimeout(function() {
  console.log("Executed later");
}, 1000);
```

Here:

* `setTimeout` initiates a timer.
* JavaScript continues execution.
* After 1000 milliseconds, the callback is executed.

---

## 4. Why Callbacks Are Needed in JavaScript

### 4.1 External Operations Are Slow

Operations that are slow relative to CPU speed include:

* Network requests
* File system access
* Database queries
* Timers
* User interactions

JavaScript does not perform these operations directly.

---

### 4.2 What Is a Web API?

**Definition**
A Web API is a set of interfaces provided by the browser that allow JavaScript to interact with features outside the JavaScript engine.

Examples of Web APIs:

* `setTimeout`
* `fetch`
* DOM events
* Geolocation
* Local storage

Important clarification:

* Web APIs are **not part of JavaScript itself**.
* They are provided by the browser environment.

---

## 5. JavaScript Engine, Web APIs, and Callbacks

### 5.1 JavaScript Engine

**Definition**
A JavaScript engine is a program that executes JavaScript code.

Examples:

* V8 in Chrome and Node.js
* SpiderMonkey in Firefox

The engine consists of:

* Call Stack
* Heap

---

### 5.2 Call Stack

**Definition**
The call stack is a data structure that tracks function execution order.

Rules:

* Functions are pushed onto the stack when called.
* Functions are popped off when they return.

---

### 5.3 How Asynchronous Callbacks Work Internally

Example:

```js
console.log("Start");

setTimeout(function cb() {
  console.log("Callback");
}, 0);

console.log("End");
```

Execution order:

1. `"Start"` is logged
2. `setTimeout` registers the callback with the browser timer API
3. `"End"` is logged
4. After the stack is empty, the callback is queued
5. Callback is executed

Output:

```
Start
End
Callback
```

---

## 6. Event Loop (Essential for Callbacks)

### 6.1 Definition of Event Loop

**Definition**
The event loop is a mechanism that continuously checks whether the call stack is empty and, if so, pushes queued callback functions onto the call stack.

Key responsibilities:

* Coordinates asynchronous execution
* Enables non-blocking behavior

---

### 6.2 Callback Queue

**Definition**
The callback queue is a data structure that stores callback functions waiting to be executed after asynchronous operations complete.

Flow:

1. Async task completes
2. Callback is placed into the queue
3. Event loop moves it to the call stack when possible

---

## 7. Callbacks in Real-World APIs

### 7.1 Timers

```js
setTimeout(function() {
  console.log("Timer finished");
}, 2000);
```

The callback:

* Is registered
* Executed later
* Does not block execution

---

### 7.2 DOM Event Callbacks

```js
button.addEventListener("click", function() {
  console.log("Clicked");
});
```

Here:

* The callback is executed only when the event occurs
* It may never execute at all

---

### 7.3 Network Requests (Older Style)

```js
function getData(callback) {
  const xhr = new XMLHttpRequest();
  xhr.onload = function() {
    callback(xhr.responseText);
  };
  xhr.open("GET", "/data");
  xhr.send();
}
```

The callback:

* Handles the response
* Is executed when the request completes

---

## 8. Error Handling with Callbacks

### 8.1 Callback Error Convention

In many JavaScript APIs, especially Node.js:

**Definition**
The error-first callback convention requires the first argument of a callback to represent an error, and the second argument to represent success data.

Example:

```js
fs.readFile("file.txt", function(err, data) {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});
```

This convention exists because:

* Exceptions do not cross async boundaries
* Errors must be explicitly passed

---

## 9. Callback Hell

### 9.1 Definition

**Definition**
Callback hell is a situation where callbacks are nested within callbacks multiple levels deep, resulting in code that is difficult to read, maintain, and reason about.

Example:

```js
doTask1(function(result1) {
  doTask2(result1, function(result2) {
    doTask3(result2, function(result3) {
      doTask4(result3, function(result4) {
        console.log(result4);
      });
    });
  });
});
```

Problems:

* Poor readability
* Hard error handling
* Difficult refactoring

---

### 9.2 Why Callback Hell Happens

Root causes:

* Sequential async dependencies
* Lack of abstraction
* No structured async control flow

---

## 10. Inversion of Control

### 10.1 Definition

**Definition**
Inversion of control occurs when you give control of your program’s execution to an external system or function.

With callbacks:

* You pass your logic to someone else
* You trust them to call it correctly

Risks:

* Callback may be called multiple times
* Callback may never be called
* Callback may be called with wrong data

This is one of the main reasons modern JavaScript moved beyond callbacks.

---

## 11. Production Implications

### 11.1 Performance

* Callbacks are lightweight
* No additional memory structures required
* Still widely used internally

---

### 11.2 Maintainability

* Complex callback chains are fragile
* Hard to debug stack traces
* Error propagation is manual

---

