# Promises 

## What is a Promise?

A **Promise** is an object that represents the **eventual result** of an asynchronous operation. It is a placeholder for a value that will be available in the future once the operation completes.

A Promise has three states:

* **Pending**: operation has started, value not yet available.
* **Fulfilled**: operation completed successfully and produced a value.
* **Rejected**: operation failed and produced an error.

Once a Promise is fulfilled or rejected, it is **settled** and cannot change its state again.

## Why Promises?

Promises provide a **clean, composable** way to work with asynchronous code, without nested callbacks. They allow chaining of operations and centralized error handling.

## Basic Usage

```javascript
function fetchUser() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ name: "Alice", id: 1 })
      // reject(new Error("Network error"))
    }, 2000)
  })
}

fetchUser()
  .then(user => {
    console.log("User received:", user)
  })
  .catch(error => {
    console.error("Error:", error)
  })
```


## How it Works (Flow)

1. Call `fetchUser()` → returns a Promise in **pending** state.
2. Async task runs (2 seconds delay).
3. If successful → call `resolve(...)` → Promise becomes **fulfilled** → `.then()` runs.
4. If failed → call `reject(...)` → Promise becomes **rejected** → `.catch()` runs.

## Related Concepts

* **then()** – handle fulfilled value.
* **catch()** – handle rejected error.
* **finally()** – run cleanup code regardless of success or failure.
* **async/await** – syntax built on top of Promises.