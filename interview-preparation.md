# JavaScript Full-Stack Interview Preparation Guide

A step-by-step master revision guide compiled directly from the practical code examples, edge cases, polyfills, and architectural concepts across this repository.

---

## Table of Contents
1. [Step 1: Data Types & Memory Management](#step-1-data-types--memory-management)
2. [Step 2: Variables, Scope, Hoisting & Temporal Dead Zone (TDZ)](#step-2-variables-scope-hoisting--temporal-dead-zone-tdz)
3. [Step 3: Execution Context & The Event Loop Architecture](#step-3-execution-context--the-event-loop-architecture)
4. [Step 4: Closures, Lexical Scope & Function Currying](#step-4-closures-lexical-scope--function-currying)
5. [Step 5: The `this` Keyword & Execution Context Rules](#step-5-the-this-keyword--execution-context-rules)
6. [Step 6: Asynchronous JavaScript (Callbacks, Promises, Async/Await)](#step-6-asynchronous-javascript-callbacks-promises-asyncawait)
7. [Step 7: Core JavaScript Polyfills (Live Coding / Machine Coding)](#step-7-core-javascript-polyfills-live-coding--machine-coding)
8. [Step 8: DOM Events (Bubbling, Capturing, Propagation)](#step-8-dom-events-bubbling-capturing-propagation)
9. [Step 9: Performance Optimization (Debouncing vs Throttling)](#step-9-performance-optimization-debouncing-vs-throttling)
10. [Step 10: Object-Oriented JavaScript (Constructor Functions vs ES6 Classes)](#step-10-object-oriented-javascript-constructor-functions-vs-es6-classes)
11. [Step 11: Top Tricky Output & Rapid-Fire Interview Questions](#step-11-top-tricky-output--rapid-fire-interview-questions)

---

## Step 1: Data Types & Memory Management
*Reference: [`data-types.html`](./js%20fundamental/data-types.html)*

### 1.1 Overview
JavaScript is a **dynamically-typed** language (variable types are resolved at runtime based on the assigned value). Data types are divided into two primary categories:

| Category | Types | Storage Location | Passing Mechanism |
| :--- | :--- | :--- | :--- |
| **Primitive Types** | `Number`, `String`, `Boolean`, `undefined`, `null`, `BigInt`, `Symbol` | Stack memory | **Pass by Value** (Copied entirely) |
| **Non-Primitive (Reference)** | `Object`, `Array`, `Function`, `Date`, `RegExp` | Heap memory (Reference pointer stored in Stack) | **Pass by Reference** (Address pointer copied) |

### 1.2 Pass by Value vs Pass by Reference
```javascript
// --- PRIMITIVE: Pass by Value ---
let a = 26;
let b;
b = a;
b = 30; // Modifying 'b' does NOT affect 'a'
console.log(a); // Output: 26
console.log(b); // Output: 30

// --- NON-PRIMITIVE: Pass by Reference ---
let obj1 = { name: "Mrityunjay" };
let obj2 = obj1; // Copies memory address pointer
obj2.name = "Mishra";
console.log(obj1.name); // Output: "Mishra" (Both pointers reference the same heap object)
```

### 1.3 Key Interview Questions & Gotchas
- **Q: What is a `Symbol`?**
  - Introduced in ES6, a `Symbol` is an immutable, completely unique primitive value often used as private/unique property keys in objects:
    ```javascript
    const s1 = Symbol("id");
    const s2 = Symbol("id");
    console.log(s1 === s2); // false
    ```
- **Q: Why does `typeof null === "object"`?**
  - This is a historic bug in JS since the first version. In the initial implementation, values were represented with a type tag and a value. The type tag for objects was `0`. `null` was represented as the NULL pointer (`0x00`), hence returning `"object"`.

---

## Step 2: Variables, Scope, Hoisting & Temporal Dead Zone (TDZ)
*Reference: [`variables-hoisting-tdz.html`](./js%20fundamental/variables-hoisting-tdz.html)*

### 2.1 Comparison: `var` vs `let` vs `const`

| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Scope** | Function Scope | Block Scope (`{ ... }`) | Block Scope (`{ ... }`) |
| **Hoisting** | Yes (initialized with `undefined`) | Yes (uninitialized, stored in TDZ) | Yes (uninitialized, stored in TDZ) |
| **Re-declaration** | Allowed in the same scope | SyntaxError | SyntaxError |
| **Re-assignment** | Allowed | Allowed | TypeError |
| **Global Object Attachment** | Attaches to `window`/`global` | Does NOT attach to global object | Does NOT attach to global object |

### 2.2 Temporal Dead Zone (TDZ)
The **TDZ** is the time gap between entering scope and the actual variable declaration where the variable cannot be accessed.
```javascript
function test() {
    // a is hoisted and initialized to undefined
    console.log(a); // Output: undefined
    var a = 9;

    if (a) {
        // b enters TDZ here at start of block
        // console.log(b); // ReferenceError: Cannot access 'b' before initialization

        let b = 10; // b exits TDZ here
        const c = 2;

        // Re-declaration fails:
        // let b = 30; // SyntaxError: Identifier 'b' has already been declared
        // const c = 4; // SyntaxError: Identifier 'c' has already been declared

        // Re-assignment:
        b = 50;   // Valid
        // c = 100; // TypeError: Assignment to constant variable

        console.log(b); // Output: 50
    }
}
test();
```

### 2.3 Function Hoisting
- **Function Declarations** are hoisted with their **entire function body** into memory during the creation phase:
  ```javascript
  fn(); // Output: "test from fn" (runs successfully)

  function fn() {
      console.log('test from fn');
  }
  ```
- **Function Expressions / Arrow Functions** behave like variables:
  ```javascript
  // exp(); // TypeError: exp is not a function (if declared with var)
  var exp = function() { console.log("Expression"); };

  // arrow(); // ReferenceError: Cannot access 'arrow' before initialization (if let/const)
  const arrow = () => { console.log("Arrow"); };
  ```

---

## Step 3: Execution Context & The Event Loop Architecture
*Reference: [`execution-eventloop.html`](./js%20fundamental/execution-eventloop.html)*

### 3.1 Execution Context Lifecycle
JavaScript creates an **Execution Context** in two distinct phases:
1. **Memory Allocation (Creation) Phase:**
   - Allocates memory for variables (`var` gets `undefined`; `let`/`const` remain uninitialized in TDZ).
   - Functions are stored with their full definitions.
   - Sets up the Scope Chain and determines `this`.
2. **Code Execution Phase:**
   - Runs code line-by-line.
   - Assigns actual values to variables and executes function calls.

Two main types:
- **Global Execution Context (GEC):** Created when JS initializes; outer-most context pushed to Call Stack first.
- **Functional Execution Context (FEC):** Created on each function invocation and pushed to the Call Stack.

```
+-------------------------------------------------------------+
|                      Call Stack                             |
|  [ FEC: function z()  ] -> popped when done                 |
|  [ FEC: function y()  ]                                     |
|  [ FEC: function x()  ]                                     |
|  [ Global Execution Context ]                               |
+-------------------------------------------------------------+
```

### 3.2 The Event Loop, Call Stack, Microtask Queue & Callback Queue
JavaScript is **single-threaded** and **synchronous** at its core. Concurrency and asynchronous operations are managed via browser **Web APIs** and the **Event Loop**.

```
+---------------------------------------------------------------------------------+
|                                 BROWSER / RUNTIME                               |
|                                                                                 |
|  +-------------------+       +-----------------------------------------------+  |
|  |    CALL STACK     |       |                    WEB APIs                   |  |
|  |  (Single thread)  |       |  setTimeout, fetch(), DOM events, setInterval |  |
|  +--------+----------+       +-----------------------+-----------------------+  |
|           ^                                          |                          |
|           |                                          v                          |
|     [EVENT LOOP]                      +-----------------------------+           |
|     Pushes task when stack is empty   |      Microtask Queue        | (HIGH PRIORITY: Promises, MutationObserver, queueMicrotask)
|           ^                           +-----------------------------+           |
|           |                                          |                          |
|           +---------------------------+              v                          |
|                                       +-----------------------------+           |
|                                       |   Callback / Task Queue     | (LOWER PRIORITY: setTimeout, setInterval, I/O)
|                                       +-----------------------------+           |
+---------------------------------------------------------------------------------+
```

#### Event Loop Algorithm:
1. Executes all synchronous code in the **Call Stack**.
2. When the Call Stack is empty, checks the **Microtask Queue** first. Executes **ALL** microtasks until the queue is completely drained.
3. Takes the oldest callback from the **Callback Queue (Macrotask Queue)** and pushes it onto the Call Stack.
4. Repeats the cycle continuously.

---

## Step 4: Closures, Lexical Scope & Function Currying
*Reference: [`clouser.html`](./js%20fundamental/clouser.html)*

### 4.1 What is a Closure?
> **Definition:** A **closure** is a function bundled together with references to its surrounding lexical state (**Lexical Environment**). It gives an inner function access to an outer function's scope even **after** the outer function has finished executing and returned from the Call Stack.

```javascript
function fn1() {
    let a = 20;
    return function fn2() {
        console.log(a); // retains reference to 'a'
    };
}

let fn2 = fn1(); // fn1 execution context finishes and pops off Call Stack
fn2();           // Output: 20
```

### 4.2 Lexical Scope
Lexical scope means that variable access is determined by the physical placement of variables and blocks in the source code.
```javascript
function x() {
    let a = 10;
    function y() {
        console.log(a); // Output: 10 (from outer scope)
        let b = 5;
        function z() {
            console.log(a + b); // Output: 15 (accesses both parent scopes)
        }
        z();
    }
    y();
}
x();
```

### 4.3 Practical Interview Coding: Function Currying
**Currying** is a functional technique of transforming a function with multiple arguments into a sequence of functions each taking a single argument.

#### Question 1: Basic 3-argument Currying
```javascript
function add(a) {
    return function (b) {
        return function (c) {
            return a + b + c;
        };
    };
}
console.log(add(2)(3)(4)); // Output: 9
```

#### Question 2: Operator Dispatcher via Currying
```javascript
function calculate(operation) {
    return function (a) {
        return function (b) {
            if (operation === 'sum') return a + b;
            if (operation === 'substract') return a - b;
            if (operation === 'multiply') return a * b;
            if (operation === 'divide') return a / b;
            return "Invalid operation";
        };
    };
}

console.log(calculate('multiply')(3)(4)); // Output: 12
console.log(calculate('sum')(10)(5));      // Output: 15
console.log(calculate('substract')(10)(3));// Output: 7
console.log(calculate('divide')(10)(2));   // Output: 5
```

---

## Step 5: The `this` Keyword & Execution Context Rules
*Reference: [`this-keyword.md`](./js%20fundamental/this-keyword.md)*

### 5.1 Global Space
In global space, `this` refers to the global object:
- **Browser:** `window`
- **Node.js:** `global`
- **ES2020 Standard:** `globalThis` works uniformly everywhere.

```javascript
console.log(this); // window (in browser)
```

### 5.2 Strict vs Non-Strict Mode
```javascript
// Non-Strict Mode (default)
function testNonStrict() {
    console.log(this); // Output: Window object
}
testNonStrict();

// Strict Mode ("use strict")
function testStrict() {
    "use strict";
    console.log(this); // Output: undefined
}
testStrict();
```
*Why?* In strict mode, `this` substitution is disabled. If a function is called without an object reference (e.g. `fn()`), `this` remains `undefined`.

### 5.3 Function Invocation Context Summary
1. **Method Call:** `obj.method()` -> `this` points to `obj`.
2. **Plain Function Call:** `fn()` -> `window` (non-strict) or `undefined` (strict).
3. **Constructor Call:** `new Fn()` -> `this` points to the newly instantiated object.
4. **Arrow Function:** Does not have its own `this`; lexically inherits `this` from enclosing scope.
5. **Explicit Binding:** `.call()`, `.apply()`, `.bind()`.

---

## Step 6: Asynchronous JavaScript (Callbacks, Promises, Async/Await)
*References: [`callback.html`](./asynjs/callback.html), [`promise.html`](./asynjs/promise.html), [`async-await.html`](./asynjs/async-await.html)*

### 6.1 Callbacks and Their Pitfalls
A **callback** is a function passed as an argument to another function to be executed later.
```javascript
function nightTime(greeting, callback) {
    console.log(greeting);
    callback();
}
function sleep() {
    console.log('sleeping');
}
nightTime('Good Night', sleep);
```

#### Two Major Problems with Callbacks:
1. **Callback Hell (Pyramid of Doom):** Deeply nested callbacks make code hard to read, maintain, and debug:
   ```javascript
   createOrder(cart, function (orderId) {
       proceedPayment(orderId, function (paymentInfo) {
           proceedOrder(paymentInfo, function () {
               updateBalance();
           });
       });
   });
   ```
2. **Inversion of Control:** You hand over the execution of your callback to another function. You lose control over when, how many times, or if the callback will ever be called.

---

### 6.2 Promises: Anatomy & State Machine
A **Promise** is an object representing the eventual completion (fulfillment) or failure of an asynchronous operation.
- **States:** `pending`, `fulfilled`, `rejected`.
- **Properties:** Immutable once settled; guarantees execution of callbacks registered via `.then()`, `.catch()`, or `.finally()`.

#### Custom Promise Example (Coffee Shop):
```javascript
function orderCoffee(coffeeName) {
    return new Promise((resolve, reject) => {
        if (coffeeName === 'cappuccino' || coffeeName === 'latte') {
            resolve('Order placed successfully');
        } else {
            reject(new Error(`${coffeeName} is not available`));
        }
    });
}

orderCoffee('latte')
    .then((result) => console.log(result))   // Output: Order placed successfully
    .catch((error) => console.error(error.message));
```

---

### 6.3 Promise Combinators Cheat Sheet

| Method | Behavior | Resolves When | Rejects When | Error Output |
| :--- | :--- | :--- | :--- | :--- |
| **`Promise.all`** | "All or nothing" (Fail-fast) | **All** promises resolve | **Any** single promise rejects | Immediate first rejection error |
| **`Promise.allSettled`** | Comprehensive settlement | **All** promises settle (resolve or reject) | Never rejects | Array of `{ status, value/reason }` |
| **`Promise.race`** | First settled | **First** promise settles (resolve OR reject) | **First** promise settles with reject | Value or error of fastest promise |
| **`Promise.any`** | First success seeking | **First** promise resolves | **All** promises reject | `AggregateError: All promises were rejected` |

```javascript
const p1 = new Promise((resolve) => setTimeout(() => resolve('p1 resolved'), 3000));
const p2 = new Promise((_, reject) => setTimeout(() => reject('p2 rejected'), 2000));
const p3 = new Promise((resolve) => setTimeout(() => resolve('p3 resolved'), 1000));

// Promise.all rejects after 2s because p2 rejects
Promise.all([p1, p2, p3])
    .then(console.log)
    .catch((err) => console.error("Promise.all caught:", err)); // Output: Promise.all caught: p2 rejected

// Inspecting AggregateError with Promise.any:
Promise.any([p2])
    .catch((err) => {
        console.log(err.errors); // Array of rejection reasons
    });
```

---

### 6.4 `async` / `await` Under the Hood
- `async` keyword ensures that the function **always returns a Promise**. Non-promise returns are automatically wrapped in `Promise.resolve()`.
- `await` pauses execution inside the function body without blocking the main browser thread. The JavaScript engine suspends the async function, pops it from the Call Stack, and re-enqueues it when the awaited Promise resolves.

```javascript
let p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Promise resolved after 2 sec');
    }, 2000);
});

async function testAsync() {
    try {
        console.log('Async function started');
        let data = await p1; // Function suspends here for 2s
        console.log(data);   // Output: Promise resolved after 2 sec
    } catch (error) {
        console.error('Handled Error:', error);
    }
}
testAsync();
```

---

## Step 7: Core JavaScript Polyfills (Live Coding / Machine Coding)
*Reference: [`polyfills.html`](./functional%20programming/polyfills.html)*

Writing custom polyfills is one of the most common frontend machine coding rounds. Here are production-grade implementations of the core methods.

### 7.1 `Array.prototype.map`
- **Spec:** Takes a callback `(element, index, array)` and returns a new mapped array.
```javascript
Array.prototype.myMap = function(callback) {
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    let res = [];
    for (let i = 0; i < this.length; i++) {
        // Handle sparse arrays (holes)
        if (i in this) {
            res.push(callback(this[i], i, this));
        }
    }
    return res;
};

// Test
console.log([1, 2, 3].myMap(x => x * 2)); // [2, 4, 6]
```

### 7.2 `Array.prototype.filter`
- **Spec:** Takes a predicate callback; returns a new array with elements returning truthy.
```javascript
Array.prototype.myFilter = function(callback) {
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    let res = [];
    for (let i = 0; i < this.length; i++) {
        if (i in this && callback(this[i], i, this)) {
            res.push(this[i]);
        }
    }
    return res;
};

// Test
console.log([1, 2, 3, 4, 5].myFilter(x => x % 2 === 0)); // [2, 4]
```

### 7.3 `Array.prototype.forEach`
- **Spec:** Executes callback for each element; returns `undefined`.
```javascript
Array.prototype.myForEach = function(callback) {
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    for (let i = 0; i < this.length; i++) {
        if (i in this) {
            callback(this[i], i, this);
        }
    }
};
```

### 7.4 `Array.prototype.reduce`
- **Spec:** Takes `(callback, initialValue)`. If no initial value is passed, the first element becomes the accumulator and iteration starts at index 1.
```javascript
Array.prototype.myReduce = function(callback, initialValue) {
    if (typeof callback !== 'function') {
        throw new TypeError(callback + ' is not a function');
    }
    let acc;
    let firstIdx;

    if (arguments.length > 1) {
        acc = initialValue;
        firstIdx = 0;
    } else {
        if (this.length === 0) {
            throw new TypeError('Reduce of empty array with no initial value');
        }
        acc = this[0];
        firstIdx = 1;
    }

    for (let i = firstIdx; i < this.length; i++) {
        if (i in this) {
            acc = callback(acc, this[i], i, this);
        }
    }
    return acc;
};

// Test
console.log([1, 2, 3, 4].myReduce((acc, curr) => acc + curr, 10)); // 20
console.log([1, 2, 3, 4].myReduce((acc, curr) => acc + curr));     // 10
```

### 7.5 `Function.prototype.call`
- **Spec:** Binds `this` to the target context and executes immediately with comma-separated arguments.
```javascript
Function.prototype.myCall = function(context = {}, ...args) {
    if (typeof this !== 'function') {
        throw new TypeError(this + ' is not callable');
    }
    // Convert primitive context (string/number) to object, or fallback to globalThis
    context = (context !== null && context !== undefined) ? Object(context) : globalThis;

    // Use Symbol to avoid overwriting existing properties
    const fnKey = Symbol();
    context[fnKey] = this;
    const result = context[fnKey](...args);
    delete context[fnKey];
    return result;
};
```

### 7.6 `Function.prototype.apply`
- **Spec:** Same as `.call`, but accepts arguments as an Array.
```javascript
Function.prototype.myApply = function(context = {}, args = []) {
    if (typeof this !== 'function') {
        throw new TypeError(this + ' is not callable');
    }
    if (!Array.isArray(args)) {
        throw new TypeError('CreateListFromArrayLike called on non-object');
    }
    context = (context !== null && context !== undefined) ? Object(context) : globalThis;

    const fnKey = Symbol();
    context[fnKey] = this;
    const result = context[fnKey](...args);
    delete context[fnKey];
    return result;
};
```

### 7.7 `Function.prototype.bind`
- **Spec:** Returns a new function with bound context and partially applied arguments (currying support).
```javascript
Function.prototype.myBind = function(context = {}, ...args) {
    if (typeof this !== 'function') {
        throw new TypeError(this + ' is not callable');
    }
    const originalFunction = this;

    return function(...args2) {
        return originalFunction.apply(context, [...args, ...args2]);
    };
};

// Test
const user = { name: "Mrityunjay" };
function greet(greeting, punctuation) {
    return `${greeting}, ${this.name}${punctuation}`;
}
const boundGreet = greet.myBind(user, "Hello");
console.log(boundGreet("!")); // "Hello, Mrityunjay!"
```

### 7.8 `Promise.all`
- **Spec:** Resolves when all promises resolve. Rejects immediately if any promise rejects. Resolves with an empty array if an empty iterable is provided.
```javascript
Promise.myAll = function(promiseArr) {
    if (!Array.isArray(promiseArr)) {
        return Promise.reject(new TypeError(promiseArr + ' is not iterable'));
    }

    return new Promise((resolve, reject) => {
        let result = [];
        let completed = 0;

        if (promiseArr.length === 0) {
            return resolve(result);
        }

        for (let i = 0; i < promiseArr.length; i++) {
            // Wrap in Promise.resolve to handle primitive/non-promise items
            Promise.resolve(promiseArr[i])
                .then((data) => {
                    result[i] = data; // Maintain correct order
                    completed++;
                    if (completed === promiseArr.length) {
                        resolve(result);
                    }
                })
                .catch((error) => {
                    reject(error); // Fail fast
                });
        }
    });
};

// Test
const pa = Promise.resolve("A");
const pb = new Promise(res => setTimeout(() => res("B"), 100));
Promise.myAll([pa, pb]).then(console.log); // ["A", "B"]
```

---

## Step 8: DOM Events (Bubbling, Capturing, Propagation)
*Reference: [`event-handling.html`](./js%20fundamental/event-handling.html)*

### 8.1 The 3 Phases of Event Propagation
```
       | |  1. Capturing Phase (Trickling down from window -> document -> body -> parent)
       v |
    [ TARGET ] 2. Target Phase (Triggered on target element)
       | ^
       | |  3. Bubbling Phase (Bubbling up from child -> parent -> body -> window)
```

### 8.2 `addEventListener` Syntax
```javascript
target.addEventListener(eventType, callback, useCapture);
```
- `useCapture`: Boolean (default is `false`).
  - `false` -> Registers listener in the **Bubbling Phase**.
  - `true` -> Registers listener in the **Capturing Phase**.

### 8.3 Propagation Walkthrough
Consider the nested structure: `#grand-parent > #parent > #child`:
```javascript
document.querySelector('#grand-parent').addEventListener('click', () => {
    console.log('Grand Parent (Capturing)');
}, true); // Capturing phase

document.querySelector('#parent').addEventListener('click', () => {
    console.log('Parent (Bubbling)');
}, false); // Bubbling phase

document.querySelector('#child').addEventListener('click', () => {
    console.log('Child (Bubbling)');
}, false); // Bubbling phase
```

When clicking on `#child`, the console logs:
1. `"Grand Parent (Capturing)"` (captured first on way down)
2. `"Child (Bubbling)"` (reached target)
3. `"Parent (Bubbling)"` (bubbled upwards to parent)

### 8.4 Stopping Propagation
- `event.stopPropagation()` stops the event from propagating further in either direction (bubbling or capturing).
- `event.stopImmediatePropagation()` also prevents other listeners attached to the *same* element for the *same* event from executing.
- `event.preventDefault()` stops default browser behavior (e.g. form submission, link navigation), without stopping event propagation.

---

## Step 9: Performance Optimization (Debouncing vs Throttling)
*Reference: [`debouncing-throttling.html`](./js%20fundamental/debouncing-throttling.html)*

Both techniques limit the rate at which functions execute during frequent events (`scroll`, `resize`, `input`, `mousemove`).

```
Trigger stream:   ||||||||||||||||||||||||||||... (pause)
Debounce (delay): ............................|   (executes once after pause)
Throttle (limit): |----|----|----|----|----|      (executes once per regular interval)
```

| Technique | Mental Model | Best Use Case |
| :--- | :--- | :--- |
| **Debounce** | "Wait until user stops typing/resizing for X milliseconds before running." | Search autocomplete, window resize recalculations, form autosave. |
| **Throttle** | "Execute at most once every X milliseconds while events continue firing." | Infinite scrolling, mousemove games, video playback progress tracking. |

### 9.1 Debounce Implementation
```javascript
function debounce(fn, delay) {
    let timerId;
    return function (...args) {
        clearTimeout(timerId); // Reset timer on every trigger
        timerId = setTimeout(() => {
            fn.apply(this, args);
        }, delay);
    };
}

// Usage with input
const handleInput = debounce((event) => {
    console.log("Fetching search results for:", event.target.value);
}, 500);
```

### 9.2 Throttle Implementation
```javascript
function throttle(fn, limit) {
    let lastRan = 0;
    return function (...args) {
        const now = Date.now();
        if (now - lastRan >= limit) {
            fn.apply(this, args);
            lastRan = now;
        }
    };
}

// Usage with scroll
const handleScroll = throttle(() => {
    console.log("Scroll position checked at:", window.scrollY);
}, 200);
```

---

## Step 10: Object-Oriented JavaScript (Constructor Functions vs ES6 Classes)
*References: [`functionConstructor.html`](./oops/functionConstructor.html), [`class.html`](./oops/class.html)*

### 10.1 Constructor Functions & Prototypal Inheritance (ES5)
```javascript
// Base Constructor
function Person(fname, lname, age) {
    this.fname = fname;
    this.lname = lname;
    this.age = age;
}

// Attaching methods to Prototype prevents duplicating methods in memory
Person.prototype.greeting = function() {
    return `Hello ${this.fname} ${this.lname}`;
};

// Derived Constructor
function Student(fname, lname, age, psp, batch) {
    // Call parent constructor with current context (super call)
    Person.call(this, fname, lname, age);
    this.psp = psp;
    this.batch = batch;
}

// Inherit prototype methods
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

const student1 = new Student('Mrityunjay', 'Mishra', 27, 81.00, 'Full Stack');
console.log(student1.greeting()); // "Hello Mrityunjay Mishra"
```

### 10.2 ES6 Classes, Getters, Setters & Inheritance
ES6 classes are syntactical sugar over JavaScript's existing prototype-based inheritance model.

```javascript
class Person {
    constructor(fname, lname, age, occupation) {
        this.fname = fname;
        this.lname = lname;
        this.age = age;
        this.occupation = occupation;
    }

    walk() {
        console.log(`${this.fname} ${this.lname} is walking`);
    }

    // Getter
    get Fullname() {
        return `${this.fname} ${this.lname}`;
    }

    // Setter
    set newAge(age) {
        if (age <= 0) throw new Error("Invalid age");
        this.age = age;
    }
}

// Subclass inheritance using extends and super
class Students extends Person {
    constructor(fname, lname, age, occupation, psp, batch) {
        super(fname, lname, age, occupation); // Must call super before using 'this'
        this.psp = psp;
        this.batch = batch;
    }
}

const s1 = new Students('Mukesh', 'Singh', 28, 'Engineer', 99.00, 'Fullstack');
console.log(s1.Fullname); // Getter called: "Mukesh Singh"
s1.newAge = 29;           // Setter called
console.log(s1.age);      // 29
s1.walk();                // Method called: "Mukesh Singh is walking"
```

---

## Step 11: Top Tricky Output & Rapid-Fire Interview Questions

### Question 1: Async Task Order (Event Loop)
```javascript
console.log("1");

setTimeout(() => {
    console.log("2");
}, 0);

Promise.resolve().then(() => {
    console.log("3");
});

console.log("4");
```
**Output:** `1`, `4`, `3`, `2`
- **Explanation:**
  1. Synchronous code executes first: `"1"` and `"4"`.
  2. `setTimeout` callback goes to the Callback Queue (macrotask).
  3. `Promise.then` callback goes to the Microtask Queue.
  4. Microtasks take priority over macrotasks: `"3"` prints before `"2"`.

---

### Question 2: Closure inside `for` Loop with `var` vs `let`
```javascript
// With var:
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3 (var is function scoped; all timers refer to the same mutated 'i')

// With let:
for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log(j), 100);
}
// Output: 0, 1, 2 (let is block-scoped; each iteration creates a new binding for 'j')
```

---

### Question 3: Output of `this` in Arrow Function vs Regular Function
```javascript
const obj = {
    name: "JavaScript",
    regularFn: function() {
        console.log(this.name);
    },
    arrowFn: () => {
        console.log(this.name);
    }
};

obj.regularFn(); // Output: "JavaScript"
obj.arrowFn();   // Output: undefined (or window.name in browser)
```
- **Explanation:** Arrow functions do not bind their own `this`. They capture `this` from the enclosing lexical context where `obj` was defined (the global scope).

---

### Question 4: Object mutation through references
```javascript
let user1 = { name: "John", details: { age: 25 } };
let user2 = { ...user1 }; // Shallow copy

user2.name = "Alice";
user2.details.age = 30;

console.log(user1.name);        // "John" (primitive string was shallow copied)
console.log(user1.details.age); // 30 (nested object reference was shared!)
```
- **Follow-up:** How do you make a deep clone?
  - `structuredClone(user1)` (Modern standard built into browsers and Node 17+)
  - `JSON.parse(JSON.stringify(user1))` (Has limitations: loses functions, symbols, circular references)
