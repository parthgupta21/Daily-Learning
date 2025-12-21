````markdown
# Garbage Collection in JavaScript  


## 1. What Is Garbage Collection

**Garbage Collection (GC)** is the automatic process by which JavaScript **reclaims memory that is no longer needed** by the program.

JavaScript is a **memory-managed language**, which means:
- Developers do not manually allocate memory
- Developers do not manually free memory
- The JavaScript engine handles memory automatically

Without garbage collection:
- Memory would keep increasing
- Applications would slow down
- Programs would eventually crash

---

## 2. Why Garbage Collection Is Needed

Every time you create:
- Objects
- Arrays
- Functions

Memory is allocated.

Example:
```javascript
let user = { name: "Alice" };
````

If unused objects are not removed:

* Memory leaks occur
* Performance degrades

Garbage collection ensures **unused memory is returned to the system**.

---

## 3. JavaScript Memory Areas

JavaScript mainly uses **two memory regions**:

### Stack Memory

* Stores primitive values
* Stores references to objects
* Stores function execution contexts

Examples:

* Numbers
* Booleans
* Function calls

### Heap Memory

* Stores objects, arrays, and functions
* Used for dynamic memory allocation

Example:

```javascript
let obj = { age: 25 };
```

* `obj` is stored on the stack
* `{ age: 25 }` is stored in the heap
* Stack variable holds a **reference** to heap memory

---

## 4. Key Concept: Reachability

**JavaScript garbage collection is based on reachability, not usage.**

An object is:

* **Reachable** → kept in memory
* **Unreachable** → eligible for garbage collection

---

## 5. What Are Roots

**Roots** are values that are always reachable.

Common roots include:

* Global variables
* Local variables in currently executing functions
* Function parameters
* Variables captured by closures

If an object can be accessed **starting from a root**, it stays in memory.

---

## 6. Reachability Example

```javascript
let user = {
  name: "Alice"
};

user = null;
```

Explanation:

* Initially, the object is reachable through `user`
* After `user = null`, no reference exists
* The object becomes unreachable
* It is eligible for garbage collection

Important:

* Garbage collection does **not** happen immediately
* It runs periodically

---

## 7. Mark and Sweep Algorithm

JavaScript engines use the **Mark and Sweep** algorithm.

### Step 1: Mark Phase

* Start from root objects
* Follow all references
* Mark all reachable objects as alive

### Step 2: Sweep Phase

* Scan the heap
* Remove unmarked objects
* Free their memory

This algorithm ensures only unreachable objects are collected.

---

## 8. Reference Chains

An object is not garbage collected if **any reference chain exists**.

Example:

```javascript
let a = { value: 10 };
let b = { ref: a };

a = null;
```

Even though `a` is set to null:

* The object is still reachable through `b.ref`
* It will not be garbage collected

---

## 9. Circular References

Circular references do **not** cause memory leaks in JavaScript.

Example:

```javascript
let a = {};
let b = {};

a.ref = b;
b.ref = a;

a = null;
b = null;
```

Explanation:

* Objects reference each other
* But no root references them
* They become unreachable
* Garbage collector removes both

JavaScript does **not** use reference counting.

---

## 10. Closures and Garbage Collection

Closures can keep memory alive longer than expected.

Example:

```javascript
function outer() {
  let data = new Array(1000000).fill("x");

  return function inner() {
    console.log("Hello");
  };
}

let fn = outer();
```

Explanation:

* `inner` forms a closure
* It keeps access to `data`
* As long as `fn` exists, `data` remains reachable

This is **expected behavior**, not a bug.

---

## 11. Common Memory Leaks

### Event Listeners

```javascript
element.addEventListener("click", function () {
  console.log("Clicked");
});
```

If:

* The element is removed
* The listener is not cleaned up

The object may stay in memory.

---

### Global Variables

```javascript
data = { large: true };
```

Without `let`, `const`, or `var`, the variable becomes global and stays reachable.

---

### Forgotten Timers

```javascript
setInterval(() => {
  console.log("Running");
}, 1000);
```

Intervals keep references alive until cleared.

---

## 12. What Garbage Collection Does NOT Do

* It does not delete variables
* It does not run immediately
* It does not guarantee exact timing
* It does not free memory still reachable

Garbage collection only frees **unreachable heap memory**.

---

## 13. Important Interview Statements

* JavaScript uses automatic garbage collection
* Memory is managed using reachability
* Mark and Sweep is the core algorithm
* Circular references are handled correctly
* Closures affect object lifetime
* Memory leaks happen due to unintended references

---

## 14. One-Line Interview Definition

> Garbage collection in JavaScript is an automatic process that frees memory by removing objects that are no longer reachable from program roots.

---

## 15. Beginner Mental Model

* Objects live in memory
* References connect objects
* Roots start the reference graph
* No path from roots means garbage
* Garbage collector cleans it up later


