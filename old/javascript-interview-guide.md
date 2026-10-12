# JavaScript Interview Guide: Senior → Junior Mentoring Style

> A comprehensive guide covering fundamental and advanced JavaScript concepts with practical examples and interview-focused insights.

---

## Table of Contents

1. [Variable Declarations: let, const, var](#1-variable-declarations-let-const-var)
2. [Closures](#2-closures)
3. [Hoisting](#3-hoisting)
4. [Event Loop](#4-event-loop)
5. [Promises](#5-promises)
6. [Async/Await](#6-asyncawait)
7. [Prototypes & Inheritance](#7-prototypes--inheritance)
8. [Event Delegation](#8-event-delegation)
9. [Shallow vs Deep Copy](#9-shallow-vs-deep-copy)
10. [call, apply, bind](#10-call-apply-bind)
11. [Map & Set](#11-map--set)
12. [WeakMap & WeakSet](#12-weakmap--weakset)
13. [Decorators](#13-decorators)
14. [Memory Management](#14-memory-management)
15. [Performance Optimizations](#15-performance-optimizations)

---

## 1. Variable Declarations: let, const, var

### Simple Explanation

Think of variable declarations as different ways to create boxes for your data. `var` is the old way (pre-ES6), while `let` and `const` are modern and safer. The key differences are scope, hoisting behavior, and mutability.

### Definition

- **var**: Function-scoped, hoisted, can be redeclared
- **let**: Block-scoped, not hoisted (temporal dead zone), cannot be redeclared
- **const**: Block-scoped, not hoisted, cannot be reassigned (but objects/arrays can be mutated)

### Code Examples

```javascript
// Example 1: Scope differences
function scopeTest() {
  if (true) {
    var varVariable = "I'm function-scoped";
    let letVariable = "I'm block-scoped";
    const constVariable = "I'm also block-scoped";
  }

  console.log(varVariable); // ✅ "I'm function-scoped"
  console.log(letVariable); // ❌ ReferenceError
  console.log(constVariable); // ❌ ReferenceError
}

// Example 2: Reassignment and mutation
let count = 1;
count = 2; // ✅ Works

const user = { name: "Alice" };
user = { name: "Bob" }; // ❌ TypeError: Assignment to constant variable
user.name = "Bob"; // ✅ Works - mutation is allowed

const numbers = [1, 2, 3];
numbers.push(4); // ✅ Works - array mutation is allowed
```

### Why this is correct

- var → function-scoped, so it’s accessible anywhere inside scopeTest
- let and const → block-scoped, so they exist only inside the if block
- Accessing letVariable or constVariable outside the block throws a ReferenceError

### Common Mistakes

- Using `var` in loops can cause unexpected behavior due to function scope
- Thinking `const` makes objects immutable (it only prevents reassignment)
- Forgetting about the temporal dead zone with `let`/`const`

### Interview Summary

Use `const` by default, `let` when you need reassignment, and avoid `var` in modern code. Understand that block scope prevents variable leakage and makes code more predictable.

---

## 2. Closures

### Simple Explanation

A closure is when a function "remembers" variables from its outer scope, even after that outer function has finished executing. It's like giving a function a backpack with variables it can access later.

### Definition

A closure is the combination of a function and the lexical environment within which that function was declared. It allows inner functions to access outer function variables.

### Code Examples

```javascript
// Example 1: Basic closure
function createCounter() {
  let count = 0; // Private variable

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
// count is not accessible from outside, but the inner function remembers it

// Example 2: Practical use - data privacy
function bankAccount(initialBalance) {
  let balance = initialBalance;

  return {
    deposit: function (amount) {
      balance += amount;
      return balance;
    },
    withdraw: function (amount) {
      if (amount > balance) return "Insufficient funds";
      balance -= amount;
      return balance;
    },
    getBalance: function () {
      return balance;
    },
  };
}

const myAccount = bankAccount(100);
console.log(myAccount.deposit(50)); // 150
console.log(myAccount.withdraw(30)); // 120
console.log(myAccount.balance); // undefined - private!
```

### Common Mistakes

- Creating closures in loops with `var` (classic interview trap)
- Not understanding that closures keep references, not copies
- Memory leaks from unintentional closures holding large objects

### Interview Summary

Closures enable data privacy, factory functions, and functional programming patterns. They're fundamental to JavaScript and used everywhere (callbacks, event handlers, React hooks).

---

## 3. Hoisting

### Simple Explanation

Hoisting is JavaScript's behavior of moving declarations to the top of their scope during compilation. Think of it as JavaScript reading through your code twice: first to register all declarations, then to execute.

### Definition

Hoisting is the process where variable and function declarations are moved to the top of their containing scope during the compilation phase, before code execution.

### Code Examples

```javascript
// Example 1: Function hoisting
sayHello(); // ✅ Works! Prints "Hello"

function sayHello() {
  console.log("Hello");
}

// But function expressions don't hoist the same way
sayGoodbye(); // ❌ TypeError: sayGoodbye is not a function

var sayGoodbye = function () {
  console.log("Goodbye");
};

// Example 2: Variable hoisting with var, let, const
console.log(varVariable); // undefined (hoisted, but not initialized)
console.log(letVariable); // ❌ ReferenceError: Cannot access before initialization

var varVariable = "I'm hoisted";
let letVariable = "I'm in the temporal dead zone";

// What actually happens:
// var varVariable; // Declaration hoisted
// console.log(varVariable); // undefined
// varVariable = "I'm hoisted"; // Assignment stays in place
```

### Common Mistakes

- Assuming `let` and `const` aren't hoisted (they are, but in the temporal dead zone)
- Relying on hoisting instead of declaring variables at the top
- Confusing function declarations with function expressions

### Interview Summary

Function declarations are fully hoisted, `var` declarations are hoisted but initialized as `undefined`, and `let`/`const` are hoisted but remain in the temporal dead zone. Always declare before use for cleaner code.

---

## 4. Event Loop

### How JavaScript Actually Runs Async Code (Event Loop Explained)

This part often gets asked in interviews.

JavaScript uses:

**1. Call Stack**
Where JS executes functions line by line.

**2. Web APIs (Browser or Node APIs)**
They handle async tasks like:

- `setTimeout`
- `fetch`
- DOM events
- database calls
- file system operations

When the async operation finishes, they send the callback to the callback queue.

**3. Callback Queue / Microtask Queue**
Two queues:

- **Macrotask Queue (Callback Queue)** e.g., `setTimeout`, `setInterval`
- **Microtask Queue** e.g., Promise `.then()`, `async/await`

**4. Event Loop**
The event loop constantly checks:
"Is the call stack empty?" If yes → push the next task from queues.

### Simple Explanation

JavaScript is single-threaded but can handle async operations through the event loop. Think of it as a restaurant with one chef (call stack) who takes orders from a queue (task queue) when they're free.

### Definition

The event loop is a mechanism that handles asynchronous operations by continuously checking if the call stack is empty and then pushing tasks from the task queue (macrotasks) or microtask queue to the call stack.

### Code Examples

```javascript
// Example 1: Understanding execution order
console.log("1: Synchronous");

setTimeout(() => {
  console.log("2: Macrotask (setTimeout)");
}, 0);

Promise.resolve().then(() => {
  console.log("3: Microtask (Promise)");
});

console.log("4: Synchronous");

// Output:
// 1: Synchronous
// 4: Synchronous
// 3: Microtask (Promise)
// 2: Macrotask (setTimeout)

// Example 2: Microtasks vs Macrotasks
console.log("Start");

setTimeout(() => console.log("Timeout 1"), 0);

Promise.resolve()
  .then(() => console.log("Promise 1"))
  .then(() => console.log("Promise 2"));

setTimeout(() => console.log("Timeout 2"), 0);

console.log("End");

// Output:
// Start
// End
// Promise 1
// Promise 2
// Timeout 1
// Timeout 2
```

### Common Mistakes

- Thinking `setTimeout(..., 0)` executes immediately
- Not understanding microtask priority over macrotasks
- Blocking the event loop with heavy synchronous operations

### Interview Summary

The event loop processes: Call Stack → Microtasks (Promises) → Macrotasks (setTimeout, setInterval). Microtasks always run before the next macrotask. Understanding this is crucial for async JavaScript.

---

## 5. Promises

### Simple Explanation

A Promise is an object representing the eventual completion or failure of an async operation. It's like a receipt you get when ordering food - it promises you'll get your food later, and it can either be fulfilled or rejected.

### Definition

A Promise is an object that represents a value that may not be available yet but will be resolved in the future. It has three states: pending, fulfilled, or rejected.

### Code Examples

```javascript
// Example 1: Creating and using Promises
const fetchUser = (id) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id, name: "Alice" });
      } else {
        reject(new Error("Invalid ID"));
      }
    }, 1000);
  });
};

fetchUser(1)
  .then((user) => console.log("User:", user))
  .catch((error) => console.error("Error:", error));

// Example 2: Promise chaining and error handling
fetch("https://api.example.com/users")
  .then((response) => {
    if (!response.ok) throw new Error("Network error");
    return response.json();
  })
  .then((data) => {
    console.log("Data:", data);
    return data.map((user) => user.name);
  })
  .then((names) => console.log("Names:", names))
  .catch((error) => console.error("Caught:", error))
  .finally(() => console.log("Cleanup"));

// Promise.all, Promise.race, Promise.allSettled
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve) => setTimeout(() => resolve(42), 100));
const promise3 = Promise.resolve("foo");

Promise.all([promise1, promise2, promise3]).then((values) =>
  console.log(values),
); // [3, 42, "foo"]

Promise.race([promise1, promise2, promise3]).then((value) =>
  console.log(value),
); // 3 (first to resolve)
```

### Common Mistakes

- Forgetting to return promises in `.then()` chains
- Not handling errors with `.catch()`
- Creating unnecessary promise wrappers (promise constructor anti-pattern)
- Confusing `Promise.all()` (fails fast) with `Promise.allSettled()` (waits for all)

### Interview Summary

Promises solve callback hell with chainable `.then()` methods. Always handle errors, return values in chains, and use `Promise.all()` for parallel operations. Modern async/await is built on promises.

---

## 6. Async/Await

### Simple Explanation

Async/await is syntactic sugar over Promises that makes asynchronous code look and behave like synchronous code. It's like writing a recipe step-by-step instead of nesting callbacks.

### Definition

`async` functions always return a Promise. `await` pauses execution until the Promise resolves, making async code easier to read and write.

### Code Examples

```javascript
// Example 1: Basic async/await
async function fetchUserData(userId) {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Error fetching user:", error);
    throw error;
  }
}

// Using the async function
fetchUserData(1)
  .then((user) => console.log(user))
  .catch((error) => console.error(error));

// Example 2: Parallel vs Sequential execution
// ❌ Sequential (slow) - takes 3 seconds
async function sequentialFetch() {
  const user = await fetch("/api/user"); // 1 second
  const posts = await fetch("/api/posts"); // 1 second
  const comments = await fetch("/api/comments"); // 1 second
  return { user, posts, comments };
}

// ✅ Parallel (fast) - takes 1 second
async function parallelFetch() {
  const [user, posts, comments] = await Promise.all([
    fetch("/api/user"),
    fetch("/api/posts"),
    fetch("/api/comments"),
  ]);
  return { user, posts, comments };
}

// Example 3: Error handling patterns
async function robustFetch(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    if (error.name === "TypeError") {
      console.error("Network error:", error);
    } else {
      console.error("Fetch error:", error);
    }
    return null;
  }
}
```

### Common Mistakes

- Forgetting `await` (returns a Promise instead of the value)
- Not using `try/catch` for error handling
- Sequential awaits when parallel execution is possible
- Using `await` in non-async functions
- Forgetting that `async` functions always return Promises

### Interview Summary

Async/await makes async code readable and maintainable. Always use `try/catch`, parallelize independent operations with `Promise.all()`, and remember that async functions return Promises.

---

## 7. Prototypes & Inheritance

### Simple Explanation

Every JavaScript object has a hidden link to another object called its prototype. When you try to access a property, JavaScript looks at the object first, then its prototype, then the prototype's prototype, forming a "prototype chain."

### Definition

Prototypes are the mechanism by which JavaScript objects inherit features from one another. Every object has an internal `[[Prototype]]` property that references another object or `null`.

### Code Examples

```javascript
// Example 1: Prototype chain
const animal = {
  eats: true,
  walk() {
    console.log("Animal walks");
  },
};

const dog = Object.create(animal);
dog.barks = true;

console.log(dog.eats); // true (inherited from animal)
console.log(dog.barks); // true (own property)
dog.walk(); // "Animal walks"

// Example 2: Constructor functions and prototypes
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function () {
  return `Hi, I'm ${this.name}`;
};

Person.prototype.species = "Homo sapiens";

const alice = new Person("Alice", 30);
const bob = new Person("Bob", 25);

console.log(alice.greet()); // "Hi, I'm Alice"
console.log(alice.species); // "Homo sapiens"
console.log(alice.greet === bob.greet); // true (shared method)

// Example 3: ES6 Classes (syntactic sugar over prototypes)
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    console.log(`${this.name} barks`);
  }
}

const myDog = new Dog("Rex", "Labrador");
myDog.speak(); // "Rex barks"
```

### Common Mistakes

- Modifying built-in prototypes (Array.prototype, Object.prototype)
- Confusing `__proto__` (deprecated) with `Object.getPrototypeOf()`
- Not understanding that methods on prototype are shared, not copied
- Forgetting to call `super()` in derived class constructors

### Interview Summary

Prototypes enable inheritance in JavaScript. Methods on prototypes are memory-efficient (shared across instances). ES6 classes are cleaner syntax but still use prototypes under the hood.

---

## 8. Event Delegation

### Simple Explanation

Instead of attaching event listeners to many child elements, attach one listener to a parent element and use event bubbling to handle events. It's like having one receptionist for a building instead of one at every office door.

### Definition

Event delegation is a pattern that leverages event bubbling to handle events at a higher level in the DOM than the element on which the event originated.

### Code Examples

```javascript
// Example 1: Without delegation (inefficient)
// ❌ Bad: Adding listeners to each item
const items = document.querySelectorAll(".list-item");
items.forEach((item) => {
  item.addEventListener("click", (e) => {
    console.log("Clicked:", e.target.textContent);
  });
});

// ✅ Good: Using event delegation
const list = document.querySelector(".list");
list.addEventListener("click", (e) => {
  if (e.target.classList.contains("list-item")) {
    console.log("Clicked:", e.target.textContent);
  }
});

// Example 2: Practical use case - dynamic content
document.querySelector("#todo-list").addEventListener("click", (e) => {
  // Handle delete button
  if (e.target.classList.contains("delete-btn")) {
    e.target.closest(".todo-item").remove();
  }

  // Handle checkbox
  if (e.target.classList.contains("todo-checkbox")) {
    e.target.closest(".todo-item").classList.toggle("completed");
  }

  // Handle edit button
  if (e.target.classList.contains("edit-btn")) {
    const todoItem = e.target.closest(".todo-item");
    const text = todoItem.querySelector(".todo-text");
    text.contentEditable = true;
    text.focus();
  }
});

// Works even for dynamically added items!
const newTodo = document.createElement("li");
newTodo.className = "todo-item";
newTodo.innerHTML = `
  <input type="checkbox" class="todo-checkbox">
  <span class="todo-text">New task</span>
  <button class="delete-btn">Delete</button>
`;
document.querySelector("#todo-list").appendChild(newTodo);
```

### Common Mistakes

- Not checking if the clicked element matches your target selector
- Forgetting about event bubbling and capturing phases
- Not using `e.stopPropagation()` when needed
- Attaching too many individual listeners instead of delegating

### Interview Summary

Event delegation improves performance, reduces memory usage, and automatically handles dynamically added elements. Use it for lists, tables, and any repeating elements.

---

## 9. Shallow vs Deep Copy

### Simple Explanation

Shallow copy duplicates the top level of an object, but nested objects are still referenced. Deep copy creates completely independent copies of everything, including nested structures. Think of it like photocopying a folder: shallow copy copies the folder but references the same documents inside; deep copy duplicates everything.

### Definition

- **Shallow Copy**: Creates a new object with copies of primitive values and references to nested objects
- **Deep Copy**: Creates a new object with copies of all values, recursively copying nested objects

### Code Examples

```javascript
// Example 1: Shallow copy methods
const original = {
  name: "Alice",
  age: 30,
  address: { city: "NYC", zip: "10001" },
};

// Method 1: Spread operator (shallow)
const copy1 = { ...original };
copy1.name = "Bob"; // ✅ Doesn't affect original
copy1.address.city = "LA"; // ❌ Affects original!

console.log(original.address.city); // "LA" (mutated!)

// Method 2: Object.assign (shallow)
const copy2 = Object.assign({}, original);

// Method 3: Array shallow copy
const arr = [1, 2, [3, 4]];
const arrCopy = [...arr];
arrCopy[2][0] = 99;
console.log(arr[2][0]); // 99 (nested array is referenced!)

// Example 2: Deep copy methods
// Method 1: JSON (limitations: no functions, dates, undefined, symbols)
const deepCopy1 = JSON.parse(JSON.stringify(original));
deepCopy1.address.city = "Boston";
console.log(original.address.city); // "NYC" (not affected!)

// Method 2: structuredClone (modern, recommended)
const deepCopy2 = structuredClone(original);

// Method 3: Custom recursive deep copy
function deepClone(obj) {
  if (obj === null || typeof obj !== "object") return obj;
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof Array) return obj.map((item) => deepClone(item));

  const clonedObj = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      clonedObj[key] = deepClone(obj[key]);
    }
  }
  return clonedObj;
}

const deepCopy3 = deepClone(original);
```

### Common Mistakes

- Using spread operator for nested objects and expecting deep copy
- Using JSON methods with functions, dates, or circular references
- Not understanding that arrays are objects (same rules apply)
- Mutating nested objects thinking you made a copy

### Interview Summary

Use spread/Object.assign for shallow copies. For deep copies, use `structuredClone()` (modern) or JSON methods (with limitations). Always consider whether you need shallow or deep copying based on your data structure.

---

## 10. call, apply, bind

### Simple Explanation

These methods let you control what `this` refers to inside a function. `call` and `apply` invoke the function immediately with a specified `this`, while `bind` creates a new function with `this` permanently set.

### Definition

- **call()**: Invokes a function with a given `this` value and arguments provided individually
- **apply()**: Invokes a function with a given `this` value and arguments provided as an array
- **bind()**: Returns a new function with a permanently bound `this` value

### Code Examples

```javascript
// Example 1: Basic usage
const person = {
  name: "Alice",
  greet: function (greeting, punctuation) {
    return `${greeting}, I'm ${this.name}${punctuation}`;
  },
};

const anotherPerson = { name: "Bob" };

// call: arguments passed individually
console.log(person.greet.call(anotherPerson, "Hello", "!"));
// "Hello, I'm Bob!"

// apply: arguments passed as array
console.log(person.greet.apply(anotherPerson, ["Hi", "."]));
// "Hi, I'm Bob."

// bind: returns a new function
const bobGreet = person.greet.bind(anotherPerson);
console.log(bobGreet("Hey", "?"));
// "Hey, I'm Bob?"

// Example 2: Practical use cases
// Use case 1: Borrowing methods
const numbers = [5, 6, 2, 3, 7];
const max = Math.max.apply(null, numbers); // 7
// Modern alternative: Math.max(...numbers)

// Use case 2: Function currying with bind
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);
console.log(double(5)); // 10
console.log(double(10)); // 20

// Use case 3: Event handlers
class Button {
  constructor(label) {
    this.label = label;
    this.clicks = 0;
  }

  handleClick() {
    this.clicks++;
    console.log(`${this.label} clicked ${this.clicks} times`);
  }

  render() {
    const btn = document.createElement("button");
    btn.textContent = this.label;
    // Without bind, 'this' would be the button element
    btn.addEventListener("click", this.handleClick.bind(this));
    return btn;
  }
}
```

### Common Mistakes

- Confusing the order of arguments (call vs apply)
- Forgetting that bind returns a new function (doesn't invoke immediately)
- Not understanding that arrow functions can't have their `this` changed
- Using bind in loops without understanding it creates new functions

### Interview Summary

Use `call` for immediate invocation with individual args, `apply` for array args, and `bind` to create a new function with fixed `this`. Modern JavaScript often uses arrow functions instead, which lexically bind `this`.

---

## 11. Map & Set

### Simple Explanation

`Map` is like an object but with superpowers: any type can be a key, it maintains insertion order, and has better performance for frequent additions/deletions. `Set` is a collection of unique values - no duplicates allowed.

### Definition

- **Map**: A collection of key-value pairs where keys can be any type, with guaranteed insertion order
- **Set**: A collection of unique values of any type

### Code Examples

```javascript
// Example 1: Map basics
const userMap = new Map();

// Any type can be a key
const objKey = { id: 1 };
const funcKey = function () {};

userMap.set("name", "Alice");
userMap.set(objKey, { role: "admin" });
userMap.set(funcKey, "callback");
userMap.set(42, "number key");

console.log(userMap.get("name")); // "Alice"
console.log(userMap.get(objKey)); // { role: "admin" }
console.log(userMap.size); // 4
console.log(userMap.has("name")); // true

// Iteration maintains insertion order
for (const [key, value] of userMap) {
  console.log(key, value);
}

// Map vs Object
const obj = {};
obj[objKey] = "value"; // Key becomes "[object Object]" string!
console.log(obj); // { "[object Object]": "value" }

// Example 2: Set basics
const numbers = new Set([1, 2, 3, 3, 4, 4, 5]);
console.log(numbers); // Set(5) { 1, 2, 3, 4, 5 }

numbers.add(6);
numbers.add(3); // Ignored (already exists)
numbers.delete(2);
console.log(numbers.has(3)); // true
console.log(numbers.size); // 5

// Practical: Remove duplicates from array
const arr = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(arr)]; // [1, 2, 3, 4, 5]

// Practical: Track unique visitors
const visitors = new Set();
visitors.add("user1");
visitors.add("user2");
visitors.add("user1"); // Ignored
console.log(`Unique visitors: ${visitors.size}`); // 2
```

### Common Mistakes

- Using objects as Map keys in plain objects (they become strings)
- Forgetting that Set comparison is by value for primitives, by reference for objects
- Not using Map when you need non-string keys
- Trying to access Map values with bracket notation (use `.get()`)

### Interview Summary

Use `Map` when you need any type as keys, guaranteed order, or frequent add/delete operations. Use `Set` for unique collections and duplicate removal. Both have better performance than objects/arrays for their use cases.

---

## 12. WeakMap & WeakSet

### Simple Explanation

WeakMap and WeakSet are like Map and Set, but with "weak" references that don't prevent garbage collection. If the only reference to an object is in a WeakMap/WeakSet, that object can be garbage collected. Perfect for metadata and caching without memory leaks.

### Definition

- **WeakMap**: A Map where keys must be objects and are weakly referenced (can be garbage collected)
- **WeakSet**: A Set where values must be objects and are weakly referenced

### Code Examples

```javascript
// Example 1: WeakMap for private data
const privateData = new WeakMap();

class User {
  constructor(name, ssn) {
    this.name = name; // Public
    privateData.set(this, { ssn }); // Private
  }

  getSSN() {
    return privateData.get(this).ssn;
  }
}

let user = new User("Alice", "123-45-6789");
console.log(user.name); // "Alice"
console.log(user.getSSN()); // "123-45-6789"
console.log(user.ssn); // undefined (private!)

// When user is garbage collected, the WeakMap entry is too
user = null; // privateData entry can now be garbage collected

// Example 2: WeakMap for caching/memoization
const cache = new WeakMap();

function processData(obj) {
  if (cache.has(obj)) {
    console.log("Returning cached result");
    return cache.get(obj);
  }

  console.log("Computing result");
  const result = { processed: true, data: obj.value * 2 };
  cache.set(obj, result);
  return result;
}

let data = { value: 10 };
processData(data); // "Computing result"
processData(data); // "Returning cached result"

data = null; // Cache entry automatically cleaned up

// Example 3: WeakSet for tracking
const visitedNodes = new WeakSet();

function traverseDOM(node) {
  if (visitedNodes.has(node)) {
    return; // Already visited
  }

  visitedNodes.add(node);
  console.log("Visiting:", node.tagName);

  for (let child of node.children) {
    traverseDOM(child);
  }
}
```

### Common Mistakes

- Trying to use primitives as keys/values (only objects allowed)
- Expecting to iterate over WeakMap/WeakSet (not possible - no `.size`, `.keys()`, etc.)
- Using WeakMap when you need to iterate or get size
- Not understanding the garbage collection benefit

### Interview Summary

Use WeakMap for private data, caching, or metadata associated with objects. Use WeakSet for tracking objects without preventing garbage collection. They prevent memory leaks in scenarios where regular Map/Set would hold references indefinitely.

---

## 13. Decorators

### Simple Explanation

Decorators are a way to modify or enhance classes and their members (methods, properties) by wrapping them with additional functionality. Think of them as gift wrapping - the gift (function/class) stays the same, but you add extra layers around it.

### Definition

Decorators are special functions that can be attached to classes, methods, accessors, properties, or parameters to modify their behavior. They use the `@` syntax and are currently a Stage 3 proposal (experimental).

### Code Examples

```javascript
// Example 1: Method decorator (logging)
function log(target, propertyKey, descriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args) {
    console.log(`Calling ${propertyKey} with args:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };

  return descriptor;
}

class Calculator {
  @log
  add(a, b) {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(2, 3);
// Logs: "Calling add with args: [2, 3]"
// Logs: "Result: 5"

// Example 2: Class decorator (singleton pattern)
function singleton(constructor) {
  let instance;

  return new Proxy(constructor, {
    construct(target, args) {
      if (!instance) {
        instance = new target(...args);
      }
      return instance;
    },
  });
}

@singleton
class Database {
  constructor() {
    this.connection = "Connected";
  }
}

const db1 = new Database();
const db2 = new Database();
console.log(db1 === db2); // true (same instance)

// Example 3: Practical decorators
// Memoization decorator
function memoize(target, propertyKey, descriptor) {
  const originalMethod = descriptor.value;
  const cache = new Map();

  descriptor.value = function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = originalMethod.apply(this, args);
    cache.set(key, result);
    return result;
  };

  return descriptor;
}

// Debounce decorator
function debounce(delay) {
  return function (target, propertyKey, descriptor) {
    const originalMethod = descriptor.value;
    let timeoutId;

    descriptor.value = function (...args) {
      clearTimeout(timeoutId);
      timeoutId = setTimeout(() => {
        originalMethod.apply(this, args);
      }, delay);
    };

    return descriptor;
  };
}

class SearchBox {
  @debounce(300)
  search(query) {
    console.log("Searching for:", query);
  }
}
```

### Common Mistakes

- Using decorators without enabling experimental features in TypeScript/Babel
- Not understanding decorator execution order (bottom to top)
- Forgetting to return the descriptor in method decorators
- Losing `this` context when not using arrow functions or `.apply()`

### Interview Summary

Decorators provide a clean way to add cross-cutting concerns (logging, validation, caching) without modifying the original code. They're widely used in frameworks like Angular and NestJS. Currently experimental in JavaScript but stable in TypeScript.

---

## 14. Memory Management

### Simple Explanation

JavaScript automatically manages memory through garbage collection, but you can still cause memory leaks if you're not careful. Understanding how memory works helps you write efficient code that doesn't slow down or crash.

### Definition

Memory management in JavaScript involves allocation (creating variables), usage (reading/writing), and deallocation (garbage collection). The garbage collector automatically frees memory that's no longer reachable.

### Code Examples

```javascript
// Example 1: Common memory leaks
// Leak 1: Global variables
function createLeak() {
  // Missing 'var', 'let', or 'const' creates global variable
  leakedVariable = "I'm global!"; // ❌ Memory leak
}

// Leak 2: Forgotten timers
function startTimer() {
  const data = new Array(1000000).fill("data");
  setInterval(() => {
    console.log(data[0]); // ❌ 'data' never released
  }, 1000);
}

// ✅ Fix: Clear the timer
function startTimerCorrectly() {
  const data = new Array(1000000).fill("data");
  const timerId = setInterval(() => {
    console.log(data[0]);
  }, 1000);

  // Clear when done
  setTimeout(() => clearInterval(timerId), 10000);
}

// Leak 3: Closures holding large objects
function createClosure() {
  const largeData = new Array(1000000).fill("data");

  return function () {
    // Even if we don't use largeData, it's kept in memory
    console.log("Hello");
  };
}

// ✅ Fix: Only capture what you need
function createClosureCorrectly() {
  const largeData = new Array(1000000).fill("data");
  const needed = largeData[0];

  return function () {
    console.log(needed); // Only captures 'needed', not entire array
  };
}

// Example 2: Proper cleanup patterns
class EventManager {
  constructor() {
    this.listeners = [];
  }

  addListener(element, event, handler) {
    element.addEventListener(event, handler);
    this.listeners.push({ element, event, handler });
  }

  // ✅ Always provide cleanup
  cleanup() {
    this.listeners.forEach(({ element, event, handler }) => {
      element.removeEventListener(event, handler);
    });
    this.listeners = [];
  }
}

// Example 3: WeakMap for automatic cleanup
const metadata = new WeakMap();

function attachMetadata(obj, data) {
  metadata.set(obj, data); // Automatically cleaned when obj is GC'd
}

let user = { name: "Alice" };
attachMetadata(user, { lastLogin: Date.now() });
user = null; // Metadata is automatically cleaned up
```

### Common Mistakes

- Creating accidental global variables
- Not removing event listeners when elements are removed
- Keeping references to DOM nodes that have been removed
- Circular references in older browsers (modern engines handle this)
- Not clearing timers and intervals

### Interview Summary

JavaScript's garbage collector removes unreachable objects. Prevent leaks by: clearing timers, removing event listeners, avoiding accidental globals, and using WeakMap for object metadata. Always clean up resources in component unmount/destroy methods.

---

## 15. Performance Optimizations

### Simple Explanation

Performance optimization is about making your code run faster and use less resources. Focus on the biggest bottlenecks first - premature optimization is the root of all evil, but knowing these techniques helps you write efficient code from the start.

### Definition

Performance optimization involves techniques to reduce execution time, memory usage, and improve user experience through faster rendering, efficient algorithms, and smart resource management.

### Code Examples

```javascript
// Example 1: Debouncing and Throttling
// Debounce: Execute after user stops typing
function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
}

const searchInput = document.querySelector("#search");
const debouncedSearch = debounce((query) => {
  console.log("Searching:", query);
  // API call here
}, 300);

searchInput.addEventListener("input", (e) => debouncedSearch(e.target.value));

// Throttle: Execute at most once per interval
function throttle(func, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

const throttledScroll = throttle(() => {
  console.log("Scroll position:", window.scrollY);
}, 100);

window.addEventListener("scroll", throttledScroll);

// Example 2: Efficient DOM manipulation
// ❌ Bad: Multiple reflows
function inefficientDOM() {
  for (let i = 0; i < 1000; i++) {
    const div = document.createElement("div");
    div.textContent = `Item ${i}`;
    document.body.appendChild(div); // Reflow on each append!
  }
}

// ✅ Good: Single reflow with DocumentFragment
function efficientDOM() {
  const fragment = document.createDocumentFragment();
  for (let i = 0; i < 1000; i++) {
    const div = document.createElement("div");
    div.textContent = `Item ${i}`;
    fragment.appendChild(div);
  }
  document.body.appendChild(fragment); // Single reflow
}

// Example 3: Memoization for expensive calculations
function memoize(fn) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const fibonacci = memoize(function (n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(40)); // Fast with memoization

// Example 4: Lazy loading and code splitting
// Lazy load modules
async function loadFeature() {
  const module = await import("./heavy-feature.js");
  module.initialize();
}

// Intersection Observer for lazy loading images
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      imageObserver.unobserve(img);
    }
  });
});

document.querySelectorAll("img[data-src]").forEach((img) => {
  imageObserver.observe(img);
});

// Example 5: Web Workers for heavy computation
// main.js
const worker = new Worker("worker.js");

worker.postMessage({ data: largeArray });

worker.onmessage = (e) => {
  console.log("Result from worker:", e.data);
};

// worker.js
self.onmessage = (e) => {
  const result = heavyComputation(e.data);
  self.postMessage(result);
};
```

### Common Mistakes

- Optimizing before measuring (premature optimization)
- Not using browser DevTools to identify actual bottlenecks
- Ignoring network performance (focus only on code)
- Over-engineering solutions for small datasets
- Not considering mobile devices and slower connections

### Interview Summary

Key optimizations: debounce/throttle user input, batch DOM updates, memoize expensive calculations, lazy load resources, use Web Workers for heavy tasks. Always measure first with DevTools, then optimize the biggest bottlenecks.

---

## Final Interview Tips

1. **Understand the "why"**: Don't just memorize - understand why these concepts exist
2. **Practice explaining**: Being able to teach a concept shows true understanding
3. **Code examples**: Always be ready to write code, not just talk about it
4. **Real-world context**: Connect concepts to practical use cases
5. **Ask questions**: Interviews are conversations - clarify requirements before coding
6. **Trade-offs**: Discuss pros/cons of different approaches
7. **Stay current**: JavaScript evolves - know what's modern vs legacy

Good luck with your interviews! 🚀
