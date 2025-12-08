# Scope and Hoisting 

## Scope

### Definition

**Scope** determines **where identifiers (variables, functions)** are **accessible** within the code. JavaScript uses **lexical scope** (static scope), meaning the **location of declaration** in the source code defines its scope, not the runtime call location.

### Types of Scope

1. **Global Scope**

   * Declared outside any function/block.
   * Accessible everywhere.

2. **Function Scope**

   * Declared inside a function.
   * Accessible only within that function.
   * Applies to `var` and function declarations.

3. **Block Scope**

   * Declared inside `{ }` (if, for, while, etc.).
   * Applies to `let` and `const`.
   * Inaccessible outside the block.

### Scoping Rules

* `var` is **function-scoped**, ignores block boundaries.
* `let` and `const` are **block-scoped**.
* Nested scopes form a **scope chain** for identifier resolution.
* Inner scopes can **shadow** outer variables.

### Example

```javascript
function f() {
  var a = 1
  if (true) {
    let b = 2
    var c = 3
  }
  console.log(a) // 1
  console.log(c) // 3
  console.log(b) // ReferenceError
}
```

`var c` escapes the block, `let b` does not.

---

## Hoisting

### Definition

**Hoisting** is the JavaScript behavior where **declarations are processed before code execution**, during the **Creation Phase** of the **Execution Context**. Declarations are conceptually moved to the **top of their scope**, making them available before their written position. Only **declarations** hoist, **initializations** do not.

### Execution Context Phases

1. **Creation Phase**

   * Memory allocated for variable and function declarations.
   * `var` = initialized to **undefined**.
   * `let` and `const` = **uninitialized**, create **Temporal Dead Zone (TDZ)** until assignment.
   * Function declarations stored with **full definition**.
   * Scope and `this` binding created.

2. **Execution Phase**

   * Code runs line by line.
   * Assignments and logic executed.
   * TDZ ends at the initializer.

### Hoisting Behavior

* `var`: hoisted, initialized to **undefined**.
* `let`/`const`: hoisted, but **in TDZ** until initialized; access before assignment throws **ReferenceError**.
* **Function Declarations**: hoisted with complete body; callable before definition.
* **Function Expressions**: variable hoisting rules apply (`var` = undefined, `let/const` = TDZ).

### Examples

#### var hoisting

```javascript
console.log(a) // undefined
var a = 10
```

#### let/const hoisting with TDZ

```javascript
console.log(a) // ReferenceError
let a = 10
```

#### function declaration hoisting

```javascript
f() // ok
function f() {}
```

#### function expression hoisting

```javascript
f() // TypeError
var f = function() {}
```

---

## Scope + Hoisting Interaction

* Scope determines **visibility**; hoisting determines **availability timing**.
* During Creation Phase, JavaScript builds the **scope chain**, allocates memory for declarations, and sets initial values based on declaration type.
* Nested scopes resolve identifiers by **walking the scope chain upward**.
* Hoisting explains why identifiers appear to exist before their textual declaration.
* TDZ enforces **temporal correctness** for `let/const`, preventing usage before initialization.

---

## Is This JavaScript-Specific?
### Hoisting

* **Observed behavior is JS-specific** due to the ECMAScript Execution Context design.
* Other languages also allocate declarations before execution but do **not expose** hoisting to the programmer.
* Accessing variables before declaration is typically **invalid** (compile-time error) in other languages.
* JavaScript exposes hoisting due to **interpreted execution model** and **function promotion** logic.

---

## Key Mechanisms to Internalize

* **Lexical Scope**: controlled by source location, not caller.
* **Scope Chain**: lookup path for identifiers.
* **Execution Context**: controls hoisting behavior.
* **Temporal Dead Zone (TDZ)**: prevents early access to `let/const`.
* **Declaration vs Initialization**: hoist only declarations.
* **var pitfalls**: hoisting to undefined + function scope can introduce bugs.
* **Function promotion**: declarations usable before definition.

