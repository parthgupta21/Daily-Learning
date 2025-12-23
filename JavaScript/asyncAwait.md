## 1. The Problem `async / await` Solves

Before defining `async / await`, we must be precise about **what problem it exists to solve**.

JavaScript already had:

* Callbacks
* Promises

Both solved *asynchrony*, but neither solved **cognitive complexity**.

### 1.1 Asynchrony vs Readability

**Asynchrony**
Asynchrony is the ability of a program to initiate an operation and continue executing without waiting for that operation to complete.

**Readability problem**
Even with Promises, asynchronous code:

* Executes out of order
* Requires chaining
* Breaks linear reasoning

Example with Promises:

```js
fetchData()
  .then(processData)
  .then(saveData)
  .catch(handleError);
```

This is better than callbacks, but still:

* Control flow is fragmented
* Error handling is indirect
* Debugging stack traces is difficult

---

## 2. What Is `async / await`?

### 2.1 Formal Definition

**`async / await` is syntax sugar over Promises that allows asynchronous code to be written and reasoned about in a synchronous, linear style without changing its asynchronous execution semantics.**

Important implications:

* It does **not** make code synchronous
* It does **not** block the thread
* It does **not** replace Promises internally

---

## 3. The `async` Keyword

### 3.1 Definition of `async`

**Definition**
The `async` keyword declares a function as asynchronous and guarantees that the function will always return a Promise.

Even if you return a non-Promise value.

Example:

```js
async function foo() {
  return 42;
}
```

Actual behavior:

```js
foo(); // Promise resolved with value 42
```

This is equivalent to:

```js
function foo() {
  return Promise.resolve(42);
}
```

---

### 3.2 Return Semantics of `async` Functions

Rules:

* Returning a value → Promise is fulfilled with that value
* Throwing an error → Promise is rejected with that error
* Returning a Promise → returned as-is

Example:

```js
async function test() {
  throw new Error("fail");
}
```

Equivalent to:

```js
function test() {
  return Promise.reject(new Error("fail"));
}
```

---

## 4. The `await` Keyword

### 4.1 Definition of `await`

**Definition**
`await` pauses the execution of the current `async` function until a Promise settles, then resumes execution with the resolved value or throws the rejection reason.

Critical clarification:

* `await` pauses **only the async function**
* It does **not** block the call stack
* It yields control back to the event loop

---

### 4.2 What Can Be Awaited?

You can `await`:

* A Promise
* Any value (which is automatically wrapped in `Promise.resolve`)

Example:

```js
await 5; // resolves immediately
```

---

## 5. Execution Model: What Actually Happens

This is the most important section.

### 5.1 Desugaring Mental Model

Code:

```js
async function example() {
  const data = await fetchData();
  console.log(data);
}
```

Internally behaves like:

```js
function example() {
  return fetchData().then(data => {
    console.log(data);
  });
}
```

Key insight:

* `await` splits the function into **continuation points**
* Execution resumes via the Promise microtask queue

---

### 5.2 Interaction with the Event Loop

When execution hits `await`:

1. The Promise is evaluated
2. The async function exits temporarily
3. JavaScript continues executing other code
4. When the Promise settles:

   * A microtask is scheduled
   * The function resumes execution

This is why `async / await` is **non-blocking**.

---

## 6. Error Handling with `async / await`

### 6.1 try / catch with Asynchronous Code

One of the primary motivations for `async / await` is **synchronous-style error handling**.

Example:

```js
async function load() {
  try {
    const data = await fetchData();
    process(data);
  } catch (err) {
    handleError(err);
  }
}
```

This works because:

* Promise rejections are re-thrown at the `await` point
* Control jumps to `catch` exactly like synchronous code

This is **not syntactic trickery**. It is a direct mapping of Promise rejection to exception semantics.

---

### 6.2 Unhandled Rejections

If an awaited Promise rejects and:

* There is no `try / catch`
* The returned Promise is not handled

Then:

* The runtime reports an unhandled rejection
* In production, this can terminate Node.js processes or break browser behavior

---

## 7. Sequential vs Parallel Awaiting

### 7.1 Sequential Await (Common Mistake)

```js
const a = await taskA();
const b = await taskB();
```

This executes:

* `taskA`
* waits
* then executes `taskB`

Total time = sum of both durations.

---

### 7.2 Parallel Execution with `Promise.all`

Correct pattern:

```js
const [a, b] = await Promise.all([
  taskA(),
  taskB()
]);
```

Key point:

* Both tasks start immediately
* Await waits for both
* Total time = max(duration)

Understanding this distinction is critical for performance.

---

## 8. Async Functions and Stack Traces

### 8.1 Logical vs Physical Stack

With `async / await`:

* The **logical call stack** appears continuous
* The **physical call stack** is split across microtasks

Modern engines reconstruct async stack traces, but:

* Stack traces can still be truncated
* Debugging requires async awareness

---

## 9. Async / Await Does Not Replace Everything

### 9.1 Top-Level Await

Historically:

* `await` could only be used inside `async` functions

Modern environments allow:

```js
await fetchData();
```

But only in:

* ES modules
* Not in classic scripts

---

### 9.2 Async Callbacks Still Exist

You cannot eliminate callbacks entirely.

Examples:

* Event handlers
* Stream APIs
* Observers

Async functions **return Promises**, but someone still calls them.


---


## 10. Conceptual Summary (No Shortcuts)

* `async` guarantees a Promise return
* `await` suspends function execution, not the thread
* Execution resumes via the microtask queue
* Errors propagate like synchronous exceptions
* Performance depends on how awaits are structured
* `async / await` is a **control-flow abstraction**, not a concurrency model

