
# DOM Manipulation Basics 

## 1. Document Object Model (DOM)

The **Document Object Model** is a **tree-structured representation** of the HTML document created by the browser during page parsing. Each HTML element becomes a **node** in this tree, represented as a **JavaScript object**. DOM provides a **programmatic interface** for reading, modifying, creating, and deleting elements, attributes, and styles.

JavaScript interacts with the DOM through the global **`document`** object, which serves as the **entry point** to the DOM tree.

---

## 2. DOM Tree Structure

The DOM represents a webpage as a **hierarchical tree**:

```
document
 └── html
      ├── head
      └── body
           ├── h1
           ├── p
           └── div
```

Each element in this tree is a **DOM Node**.
Key node types:

* **Element Nodes** (tags like `<div>`, `<p>`)
* **Text Nodes**
* **Comment Nodes**
* **Document Node** (root)

Understanding this structure is critical for selecting and manipulating nodes correctly.

---

## 3. Selecting DOM Elements

DOM manipulation begins with **selecting elements**. Frequently used selection APIs:

### a. `document.getElementById`

Selects a single element by ID.

```javascript
const title = document.getElementById("main-title")
```

### b. `document.getElementsByClassName`

Returns a live HTMLCollection.

```javascript
const items = document.getElementsByClassName("item")
```

### c. `document.querySelector`

Uses CSS selectors and returns the **first matching element**.

```javascript
const btn = document.querySelector(".btn")
```

### d. `document.querySelectorAll`

Returns a **static NodeList** of all matching elements.

```javascript
const paragraphs = document.querySelectorAll("p")
```

`querySelector` and `querySelectorAll` are preferred due to flexibility and familiarity with CSS selectors.

---

## 4. Manipulating Content

### a. Updating Text Content

```javascript
element.textContent = "Updated Text"
```

Modifies only the textual content.

### b. Updating HTML Structure

```javascript
element.innerHTML = "<strong>Bold Text</strong>"
```

Injects HTML; should be used carefully to avoid XSS vulnerabilities.

### c. Reading Content

```javascript
console.log(element.textContent)
console.log(element.innerHTML)
```

---

## 5. Manipulating Styles

DOM elements expose a **style** property that mirrors inline CSS.

### a. Inline Style Updates

```javascript
element.style.color = "blue"
element.style.backgroundColor = "yellow"
```

### b. Managing CSS Classes

```javascript
element.classList.add("active")
element.classList.remove("hidden")
element.classList.toggle("selected")
element.classList.contains("highlight") // boolean result
```

Using classList is the preferred method for style manipulation because it keeps structure and style concerns separate.

---

## 6. Manipulating Attributes

HTML attributes can be read or updated programmatically.

### a. Direct Property Access

```javascript
img.src = "banner.png"
img.alt = "Promotional Banner"
```

### b. Attribute Methods

```javascript
element.setAttribute("role", "button")
element.getAttribute("data-id")
element.removeAttribute("disabled")
```

Attribute methods allow access to attributes not directly mapped to element properties.

---

## 7. Creating and Inserting Elements

### a. Creating Nodes

```javascript
const div = document.createElement("div")
div.textContent = "Hello"
```

### b. Appending Elements

```javascript
document.body.appendChild(div)
```

### c. Inserting Before Another Element

```javascript
container.insertBefore(newElement, container.firstChild)
```

### d. Appending Multiple Elements

```javascript
list.append(item1, item2, item3)
```

### e. Cloning Nodes

```javascript
const copy = element.cloneNode(true) // deep clone
```

---

## 8. Removing Elements

### a. Direct Removal

```javascript
element.remove()
```

### b. Removing via Parent Node

```javascript
element.parentNode.removeChild(element)
```

---

## 9. Event Handling

Events allow scripts to respond to user actions.

### a. Adding an Event Listener

```javascript
button.addEventListener("click", function() {
  console.log("Button clicked")
})
```

### b. Removing an Event Listener

```javascript
function handleClick() {}
button.addEventListener("click", handleClick)
button.removeEventListener("click", handleClick)
```

### c. Accessing Event Object

```javascript
input.addEventListener("input", function(event) {
  console.log(event.target.value)
})
```

---

## 10. DOM Manipulation Performance Considerations

* Repeated DOM updates are expensive; prefer **batch updates**.
* Use **document fragments** for inserting multiple elements.
* Accessing layout-related properties like `offsetHeight` triggers **reflow**.
* Avoid unnecessary use of `innerHTML` when updating large structures.
* Minimize deep DOM traversals and querySelector calls inside loops.

---

## 11. Is DOM Manipulation JavaScript-Specific?

The DOM is a **browser-provided API**, not part of JavaScript itself.
However, JavaScript is the primary language used to manipulate the DOM because:

* Browsers embed JavaScript engines.
* JavaScript is the default scripting language for the web.
* DOM APIs are designed to work seamlessly with JavaScript.

Other languages can manipulate the DOM if compiled or transpiled to JS (e.g., TypeScript, WebAssembly bindings).

---

# Interview Questions

## Q1: What is the DOM and how is it different from HTML?

**Answer:**
HTML is the static markup written by the developer.
The DOM is the **runtime, in-memory representation** of that markup created by the browser.
JavaScript manipulates the DOM, not the HTML file itself.
DOM is a **tree of nodes** with methods and properties enabling dynamic interaction.

---

## Q2: What is the difference between `innerHTML` and `textContent`?

**Answer:**
`textContent` sets or retrieves **raw text** without parsing HTML.
`innerHTML` sets or retrieves **HTML markup**, and the browser parses it into DOM nodes.
`textContent` is safer and faster; `innerHTML` should be used cautiously due to potential XSS.

---

## Q3: What is the difference between `querySelector` and `querySelectorAll`?

**Answer:**
`querySelector` returns the **first matching element**.
`querySelectorAll` returns a **static NodeList** containing all matching elements.
NodeList does not update automatically when DOM changes.

---

## Q4: Why is `classList` preferred over modifying `className`?

**Answer:**
`classList` provides structured methods (`add`, `remove`, `toggle`, `contains`) and avoids the need to manipulate the entire class string manually. It reduces errors and improves readability.

---

## Q5: What is a DocumentFragment and why is it used?

**Answer:**
A **DocumentFragment** is a lightweight container for DOM nodes.
Manipulating nodes inside a fragment avoids repeated reflows and repaints.
After preparation, the fragment is inserted into the DOM in a single operation, improving performance.

Example:

```javascript
const fragment = document.createDocumentFragment()
for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li")
  li.textContent = "Item " + i
  fragment.appendChild(li)
}
list.appendChild(fragment)
```

---

## Q6: Explain event bubbling and event capturing in the DOM.

**Answer:**

* **Capturing phase:** event travels **from root down to target**
* **Bubbling phase:** event travels **from target back up to root**
  JavaScript event listeners listen in bubbling phase by default.
  You can enable capturing via:

```javascript
element.addEventListener("click", handler, true)
```

---

## Q7: Why should you avoid heavy DOM manipulation inside loops?

**Answer:**
Each DOM modification triggers expensive recalculations (reflow, repaint).
Better approach: build elements in memory (via fragment or arrays) and insert once.

---

## Q8: What does it mean that the DOM is “live”?

**Answer:**
Some DOM collections (e.g., `getElementsByClassName`) automatically update when the DOM changes.
This can cause unexpected behavior if not handled carefully.
`querySelectorAll` is **static**, which avoids such issues.


