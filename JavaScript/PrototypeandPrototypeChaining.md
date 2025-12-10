
# Prototypes & Prototype Chain 

## 1. Prototype-Based Inheritance (Concept)

JavaScript uses **prototype-based inheritance**, where **objects inherit from other objects**.
Instead of copying properties like classical inheritance, JavaScript **links objects** using their **internal prototype**.

When accessing a property on an object:

1. JavaScript checks the **object itself**
2. If not found, it checks the **prototype**
3. Continues through the **prototype chain**
4. Stops at **null**

This lookup process is called the **Prototype Chain**.

---

## 2. Prototype (Definition)

A **prototype** is an **object** that acts as a **fallback source** for property and method lookup when they do not exist on the original object.

Every JavaScript object has an internal hidden link called **[[Prototype]]**, visible as `__proto__` in developer tools.

Standard API:

```javascript
Object.getPrototypeOf(obj)   // read prototype
Object.setPrototypeOf(obj, proto) // set prototype
```

---

## 3. Prototype Chain (Definition)

The **Prototype Chain** is a sequence of linked objects created through **[[Prototype]]** references.
It defines the **search path** for property resolution.

Example chain:

```
obj → obj.__proto__ → Object.prototype → null
```

Each arrow represents a **lookup level**.

---

## 4. How Objects Get Prototypes

### A) Using Object Literal

```javascript
const obj = { a: 1 }
// obj.__proto__ === Object.prototype
```

### B) Using Object.create()

```javascript
const proto = { b: 2 }
const obj = Object.create(proto)
console.log(obj.b) // 2 (from prototype)
```

### C) Using Constructor Functions

```javascript
function Person(name) {
  this.name = name
}

Person.prototype.sayHi = function() {
  console.log("Hi " + this.name)
}

const p = new Person("Alice")
p.sayHi() // method from prototype
```

The **new** operator sets:

```
p.__proto__ === Person.prototype
```

---

## 5. Modern Syntax: class (Syntactic Sugar)

ES6 **class** syntax uses prototype internally.
Methods defined in a class are added to **ClassName.prototype**, not copied to each instance.

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
  sayHi() {
    console.log("Hi " + this.name)
  }
}

const p = new Person("Bob")
p.sayHi()
```

This is equivalent to the constructor function example.

---

## 6. Understanding Property Lookup

When calling:

```javascript
p.sayHi()
```

Lookup process:

1. Check `p` for `sayHi`
2. Not found → check `Person.prototype`
3. Found → execute method
4. If still not found → check `Object.prototype`
5. If not found → return `undefined`

This lookup chain is the **Prototype Chain**.

---

## 7. Code Examples to Solidify Understanding

### Example 1: Basic Inheritance via Prototype

```javascript
const animal = { eats: true }
const dog = Object.create(animal)
dog.barks = true

console.log(dog.barks) // true (own property)
console.log(dog.eats) // true (inherited)
```

Prototype chain:

```
dog → animal → Object.prototype → null
```

---

### Example 2: Shared Methods via Prototype

```javascript
function User(name) {
  this.name = name
}

User.prototype.sayHello = function() {
  return "Hello " + this.name
}

const u1 = new User("Alice")
const u2 = new User("Bob")

console.log(u1.sayHello()) // Hello Alice
console.log(u2.sayHello()) // Hello Bob
```

`sayHello` is stored **once** and shared by all instances.

---

### Example 3: Checking Prototypes

```javascript
const obj = {}
console.log(Object.getPrototypeOf(obj) === Object.prototype) // true
console.log(Object.getPrototypeOf(Object.prototype)) // null
```

`Object.prototype` is the **last** step in the chain.

---

### Example 4: Custom Prototype Chain

```javascript
const A = { a: 1 }
const B = Object.create(A)
B.b = 2
const C = Object.create(B)
C.c = 3

console.log(C.a) // 1 (from A)
console.log(C.b) // 2 (from B)
console.log(C.c) // 3 (own)
```

Chain:

```
C → B → A → Object.prototype → null
```

---

## 8. Important Behavior Rules

* Only **missing properties** trigger prototype lookup.
* Assigning a property **always writes to the object**, not its prototype.
* Object methods defined on **prototype** are **shared**, not duplicated.
* `class` is **syntactic sugar**, prototype inheritance still applies.
* `__proto__` is **non-standard accessor**, use `Object.getPrototypeOf`.

---

## 9. Prototype vs Class Inheritance

| Concept              | Class (Java/C++) | JavaScript Prototype |
| -------------------- | ---------------- | -------------------- |
| Model                | Blueprint-based  | Object-based         |
| Behavior Sharing     | Copy/instance    | Link/chain           |
| Structure            | Inheritance tree | Prototype chain      |
| Method Storage       | Per type         | On prototype         |
| Runtime Modification | Limited          | Dynamic              |

JavaScript **does not copy methods** into objects; it **links objects to prototypes**.

---

## 10. Why Prototype Matters (Interview Focus)

Understanding prototypes is essential for:

* Explaining how **methods** like `toString` are available
* Understanding **object creation**
* Understanding **class internals**
* Avoiding **memory overhead**
* Debugging **property lookup**
* Understanding `new`, `super`, `instanceof`
* Writing **custom inheritance** patterns

---

# Interview Questions (High-Value)

### Q1: What is a prototype in JavaScript?

Expected points:

* Prototype is an object used as a fallback for property lookup
* Linked through **[[Prototype]]**
* Forms a **prototype chain**
* Used for **inheritance** and **method sharing**

---

### Q2: What is the prototype chain?

Expected points:

* Sequence of **linked objects**
* Property lookup moves upward until **null**
* Terminates at **Object.prototype**
* Implements inheritance in JavaScript

---

### Q3: How does the `new` operator work?

Expected steps:

1. Creates a new empty object
2. Sets its prototype to `Constructor.prototype`
3. Calls constructor with `this` binding
4. Returns created object

---

### Q4: How does `class` relate to prototypes?

Expected points:

* Class syntax uses prototype internally
* Methods live on **ClassName.prototype**
* Instances delegate to prototype chain
* Syntactic sugar over constructor functions

---

### Q5: What is the difference between `__proto__` and `prototype`?

Expected points:

* `__proto__` is the **prototype of an object**
* `prototype` is a **property on constructor functions**
* `new` links instance.**proto** = Constructor.prototype

---

