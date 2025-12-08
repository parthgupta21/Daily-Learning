
# Execution Context & Call Stack 

## 1. Execution Context

### Definition

An **Execution Context** is the environment in which JavaScript code is executed. It contains everything required to run the code, including:

* Variables and function references
* The value of `this`
* The scope chain
* Memory allocation for identifiers
* Arguments passed into a function

A new execution context is created each time:

* The script starts (Global Execution Context)
* A function is called (Function Execution Context)

Each execution context has two phases:

### 1. Creation Phase

* Memory is allocated
* `this` value is set
* Scope chain is prepared
* Variables are hoisted

### 2. Execution Phase

* Code runs line by line
* Variable assignments happen
* Logic executes

**In short:**

> Execution Context is the container that holds everything needed to execute code.

---

## 2. Call Stack

### Definition

The **Call Stack** is a stack data structure (LIFO) that tracks the **execution order** of functions.

* When a function is called, a new execution context is **pushed** to the stack.
* When the function completes, its execution context is **popped** from the stack.
* The function at the **top of the stack** is always the one currently executing.

**In short:**

> The Call Stack decides which function runs now and where to return when it finishes.

---

## 3. Relationship Between Execution Context & Call Stack

* Every function call creates a **new execution context**.
* The **Call Stack** stores those contexts in order.
* Nested function calls create multiple contexts stacked on top of each other.

Think of it as:

* **Execution Context = what runs**
* **Call Stack = the order it runs in**

---

## 4. Example

```javascript
function add(a, b) {
  return a + b
}

function printResult() {
  const result = add(2, 3)
  console.log(result)
}

printResult()
```

### Call Stack Flow

1. Global Execution Context is created

```
[ Global Execution Context ]
```

2. `printResult()` is called

```
[ printResult() Execution Context ]
[ Global Execution Context ]
```

3. Inside `printResult()`, `add()` is called

```
[ add() Execution Context ]
[ printResult() Execution Context ]
[ Global Execution Context ]
```

4. `add()` returns value

```
[ printResult() Execution Context ]
[ Global Execution Context ]
```

5. `printResult()` returns

```
[ Global Execution Context ]
```

