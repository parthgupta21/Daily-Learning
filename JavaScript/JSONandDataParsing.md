# 1. What Is Data Serialization?

### Definition: Serialization

**Serialization** is the process of converting in-memory data structures into a format that can be stored or transmitted and later reconstructed.

Why serialization is needed:

* Memory representations differ across programs and machines
* Data sent over a network must be plain text or bytes
* Programming languages use different internal formats

Serialization converts **language-specific structures** into **language-agnostic representations**.

---

# 3. What Is JSON?

### 3.1 Definition of JSON

**JSON (JavaScript Object Notation)** is a lightweight, text-based data interchange format used to represent structured data.

Formally:

> **JSON is a standardized textual format for representing hierarchical data using key-value pairs and ordered lists, derived from JavaScript object literal syntax but independent of JavaScript execution.**

Key clarifications:

* JSON is **data**, not code
* JSON is **text**
* JSON is **language-independent**
* JSON syntax is strictly defined

---

### 3.2 Why JSON Exists

JSON exists to solve the problem of:

* Transferring structured data between systems
* Doing so in a human-readable way
* Remaining easy for machines to parse

Before JSON:

* XML was dominant
* XML was verbose and complex

JSON is:

* Compact
* Predictable
* Easy to parse

---

# 4. JSON vs JavaScript Objects

This distinction is critical.

### JavaScript Object

A JavaScript object:

* Exists in memory
* Can contain functions
* Can contain symbols
* Can contain undefined
* Is executable context

Example:

```js
const obj = {
  name: "Alice",
  greet() {}
};
```

---

### JSON Object

A JSON object:

* Is plain text
* Cannot contain functions
* Cannot contain undefined
* Cannot contain comments
* Cannot execute

Example:

```json
{
  "name": "Alice"
}
```

JSON is a **description of data**, not behavior.

---

# 5. JSON Data Types

JSON supports **exactly six data types**.

### 5.1 String

**Definition**
A string is a sequence of Unicode characters enclosed in double quotes.

```json
"hello"
```

Rules:

* Must use double quotes
* No single quotes allowed

---

### 5.2 Number

**Definition**
A number represents numeric data and may be integer or floating-point.

```json
42
3.14
```

JSON has:

* No `NaN`
* No `Infinity`

---

### 5.3 Boolean

```json
true
false
```

---

### 5.4 Null

**Definition**
`null` represents the intentional absence of a value.

```json
null
```

---

### 5.5 Object

**Definition**
A JSON object is an unordered collection of key-value pairs.

```json
{
  "id": 1,
  "name": "Alice"
}
```

Rules:

* Keys must be strings
* Values must be valid JSON types

---

### 5.6 Array

**Definition**
A JSON array is an ordered list of values.

```json
[1, 2, 3]
```

---

# 6. What Is Data Parsing?

### Definition: Parsing

**Parsing** is the process of converting raw textual data into a structured, in-memory representation that a program can work with.

Parsing answers the question:

> “How do I turn this text into usable data?”

---

# 7. JSON Parsing in JavaScript

### 7.1 `JSON.parse`

**Definition**
`JSON.parse` converts a JSON-formatted string into a JavaScript value.

Example:

```js
const json = '{"name":"Alice","age":30}';
const obj = JSON.parse(json);
```

After parsing:

* `obj` is a real JavaScript object
* Properties can be accessed normally

---

### 7.2 What Happens Internally

When `JSON.parse` runs:

1. The engine validates the JSON syntax
2. The text is tokenized
3. A parse tree is built
4. JavaScript values are allocated in memory
5. References are returned

If syntax is invalid:

* A `SyntaxError` is thrown

---

### 7.3 Parsing Errors

Example:

```js
JSON.parse("{name: Alice}");
```

Error because:

* Keys are not quoted
* String value is not quoted

JSON is strict by design.

---

# 8. JSON Stringification

### 8.1 `JSON.stringify`

**Definition**
`JSON.stringify` converts a JavaScript value into a JSON-formatted string.

Example:

```js
const obj = { name: "Alice", age: 30 };
const json = JSON.stringify(obj);
```

Output:

```json
{"name":"Alice","age":30}
```

---

### 8.2 Serialization Rules

During stringification:

* Functions are omitted
* `undefined` is omitted
* Symbols are omitted
* Circular references throw an error

Example:

```js
const obj = {};
obj.self = obj;
JSON.stringify(obj); // TypeError
```

---

# 9. Data Exchange and JSON

### 9.1 Network Transmission

When data is sent over a network:

* It is sent as text or bytes
* JSON is used because it is text-based

Example flow:

1. Server sends JSON string
2. Client receives text
3. Client parses JSON
4. Data becomes usable objects

---

### 9.2 APIs and JSON

Most modern APIs:

* Accept JSON as request body
* Return JSON as response body

This standardization exists because:

* JSON is universally supported
* Parsing is deterministic
* Schema can be documented

---

# 10. Security and Parsing

### 10.1 Why JSON Is Safer Than `eval`

Incorrect approach:

```js
eval('(' + json + ')');
```

Problems:

* Executes arbitrary code
* Severe security risk

`JSON.parse`:

* Only parses data
* Never executes code

---
