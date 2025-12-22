# Garbage Collection in JS  
---

## 1. Memory Management in JavaScript

### 1.1 What Is Memory?

**Definition**
Memory is a finite physical resource of a computer used to store data and instructions that are actively used by programs during execution.

In JavaScript, memory is used to store:

* Variables
* Objects
* Functions
* Closures
* Execution contexts

---

### 1.2 What Is Memory Management?

**Definition**
Memory management is the process of allocating memory when data is needed and releasing memory when data is no longer needed.

Two broad models exist:

1. Manual memory management
2. Automatic memory management

JavaScript uses **automatic memory management**.

---

## 2. What Is Garbage Collection?

### 2.1 Definition of Garbage Collection

**Definition**
Garbage collection is an automatic process by which the JavaScript engine identifies memory that is no longer reachable by the program and reclaims it so it can be reused.

Key points:

* Developers do not explicitly free memory
* The JavaScript engine decides when to reclaim memory
* The process runs periodically

---

### 2.2 Why Garbage Collection Is Necessary

JavaScript applications run:

* In browsers for long-lived sessions
* On servers for days or months

Without garbage collection:

* Memory usage would grow indefinitely
* Applications would eventually crash

---

## 3. JavaScript Memory Model

### 3.1 Stack Memory

**Definition**
Stack memory is a region of memory that stores primitive values and function execution contexts.

Stored on the stack:

* Numbers
* Strings
* Booleans
* Undefined
* Null
* Function call frames
* References to objects

Characteristics:

* Fast allocation and deallocation
* Follows Last-In-First-Out order
* Automatically cleaned when functions return

Example:

```js
function add(a, b) {
  return a + b;
}
```

Here:

* `a` and `b` are stored on the stack
* When `add` finishes, stack memory is cleared

---

### 3.2 Heap Memory

**Definition**
Heap memory is a large, unstructured region of memory used to store objects, arrays, and functions.

Stored on the heap:

* Objects
* Arrays
* Functions
* Closures

Example:

```js
let user = { name: "Alice" };
```

Here:

* The object `{ name: "Alice" }` is stored in heap
* `user` stores a reference to that object

---

## 4. Reachability: The Core Concept

### 4.1 What Is Reachability?

**Definition**
An object is considered reachable if it can be accessed or referenced starting from a set of known root references.

Garbage collection in JavaScript is fundamentally **reachability-based**, not usage-based.

---

### 4.2 Root References

**Definition**
Root references are references that are always accessible to the JavaScript engine.

Examples of roots:

* Global variables
* Variables in the current call stack
* Function parameters
* Local variables of executing functions
* Closures that are still referenced

If an object is reachable from a root, it is **not garbage**.

---

### 4.3 Example of Reachability

```js
let user = {
  name: "Alice"
};

user = null;
```

Step-by-step:

1. Object is created in heap
2. `user` references it
3. `user = null` removes the reference
4. No root references remain
5. Object becomes unreachable
6. Garbage collector may reclaim it

---

## 5. How Garbage Collection Works Internally

### 5.1 Mark-and-Sweep Algorithm

**Definition**
Mark-and-sweep is the primary garbage collection algorithm used by modern JavaScript engines.

It works in two phases:

1. Mark
2. Sweep

---

### 5.2 Mark Phase

During the mark phase:

* The garbage collector starts from root references
* Traverses all reachable objects
* Marks them as alive

This traversal explains why:

* Deep object graphs matter
* Circular references do not cause leaks by themselves

---

### 5.3 Sweep Phase

During the sweep phase:

* The engine scans heap memory
* Any unmarked object is considered garbage
* Memory is reclaimed

Important clarification:

* Memory is reclaimed, not necessarily returned to the OS
* It is often reused internally

---

## 6. Circular References and Garbage Collection

### 6.1 What Is a Circular Reference?

**Definition**
A circular reference occurs when two or more objects reference each other directly or indirectly.

Example:

```js
let a = {};
let b = {};

a.b = b;
b.a = a;
```

---

### 6.2 Why Circular References Are Not a Problem in JavaScript

In older memory models, circular references caused leaks.

In JavaScript:

* Reachability is the only criterion
* If the cycle is unreachable from roots, it is collected

Thus:

```js
a = null;
b = null;
```

The cycle becomes unreachable and is garbage collected.

---

## 7. Closures and Memory Retention

### 7.1 What Is a Closure?

**Definition**
A closure is a function that retains access to variables from its lexical scope even after the outer function has finished executing.

---

### 7.2 Closures and Garbage Collection

Example:

```js
function outer() {
  let bigData = new Array(1000000);
  return function inner() {
    console.log(bigData.length);
  };
}

let fn = outer();
```

Here:

* `bigData` remains in memory
* Because `inner` references it
* Even though `outer` has finished

Garbage collection will not reclaim:

* Variables captured by live closures

This is a frequent source of unintended memory retention.

---

## 8. Common Sources of Memory Leaks

### 8.1 Global Variables

**Definition**
A global variable is a variable accessible from anywhere in the program.

Example:

```js
leakedData = "I am global";
```

Problems:

* Globals are root references
* Objects referenced by globals are never collected

---

### 8.2 Forgotten Timers and Callbacks

```js
setInterval(function() {
  console.log("Running");
}, 1000);
```

If not cleared:

* Callback remains referenced
* Captured variables remain in memory

---

### 8.3 Detached DOM Elements

**Definition**
A detached DOM element is an element removed from the document but still referenced in JavaScript.

Example:

```js
let element = document.getElementById("box");
document.body.removeChild(element);
```

If `element` is still referenced:

* Memory cannot be reclaimed

---

### 8.4 Event Listeners Not Removed

```js
element.addEventListener("click", handler);
```

If `element` lives long:

* `handler` and its closure stay in memory

---

## 9. When Does Garbage Collection Run?

### 9.1 Non-Deterministic Nature

**Definition**
Garbage collection is non-deterministic, meaning its exact timing is not predictable.

Reasons:

* Depends on engine heuristics
* Depends on memory pressure
* Depends on allocation rate

Important consequence:

* You cannot force garbage collection in standard JavaScript

---

## 10. Performance Implications

### 10.1 Stop-the-World Pauses

**Definition**
A stop-the-world pause is a moment when JavaScript execution is paused while garbage collection runs.

Modern engines:

* Use incremental GC
* Use generational GC
* Minimize pause duration

---

### 10.2 Generational Garbage Collection

**Definition**
Generational GC is based on the observation that most objects die young.

Heap is divided into:

* Young generation
* Old generation

Young objects:

* Collected frequently
* Cheap to clean

Old objects:

* Collected less often
* More expensive

---

## 11. Manual Memory Management Myths

Common misconceptions:

* Setting variables to `null` forces GC
* Calling functions cleans memory immediately
* Garbage collection happens after every function call

Reality:

* Setting to `null` only removes a reference
* GC timing is engine-controlled
* Memory cleanup is probabilistic

---

## 12. Garbage Collection in Production Systems

### 12.1 Why Understanding GC Matters

* Memory leaks degrade performance over time
* High GC pressure causes latency spikes
* Long-running services depend on stable memory behavior

---

### 12.2 Debugging Memory Issues

Common strategies:

* Heap snapshots
* Memory profiling
* Monitoring allocation patterns

Garbage collection is not about:

* Manually freeing memory

It is about:

* Avoiding unintended references
* Designing lifetimes consciously

---

## 13. Summary of Core Principles (Conceptual, Not a Checklist)

* JavaScript uses automatic garbage collection
* Reachability determines object lifetime
* Closures extend memory lifetime
* Circular references are safe if unreachable
* Leaks come from unintended root references
* Garbage collection is invisible but impactful

