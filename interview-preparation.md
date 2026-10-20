# JavaScript Revision & Interview Preparation Notes

This markdown file compiles all the revision notes, concepts, and code directly from your files across `js fundamental`, `asynjs`, `functional programming`, and `oops` without changing your original text and explanations.

---

## Table of Contents
- [1. JS Fundamental](#1-js-fundamental)
  - [1.1 Variables, Hoisting & TDZ (`variables-hoisting-tdz.html`)](#11-variables-hoisting--tdz)
  - [1.2 Data Types (`data-types.html`)](#12-data-types)
  - [1.3 Execution Context & Event Loop (`execution-eventloop.html`)](#13-execution-context--event-loop)
  - [1.4 Closure (`clouser.html`)](#14-closure)
  - [1.5 This Keyword & Explicit Binding (`call`, `apply`, `bind`) (`this-keyword.md`)](#15-this-keyword--explicit-binding-call-apply-bind)
  - [1.6 Event Handling: Bubbling, Capturing & Event Delegation (`event-handling.html`)](#16-event-handling-bubbling-capturing--event-delegation)
  - [1.7 Debouncing & Throttling (`debouncing-throttling.html`)](#17-debouncing--throttling)
- [2. Asynchronous JavaScript (`asynjs`)](#2-asynchronous-javascript-asynjs)
  - [2.1 CallBack (`callback.html`)](#21-callback)
  - [2.2 Promise (`promise.html`)](#22-promise)
  - [2.3 Async Await (`async-await.html`)](#23-async-await)
  - [2.4 AbortController & Fetch API: Canceling Requests & Race Conditions (`abort-controller.html`)](#24-abortcontroller--fetch-api-canceling-in-flight-requests-and-handling-race-conditions)
- [3. Functional Programming](#3-functional-programming)
  - [3.1 Higher-Order Functions (HOFs): Array Transformations & Immutability Methods (`hof-transformations.html`)](#31-higher-order-functions-hofs-array-transformations--immutability-methods)
  - [3.2 Important Polyfills (`polyfills.html`)](#32-important-polyfills)
  - [3.3 Metaprogramming & Advanced Primitives: Proxy, Reflect, Symbol, Iterators & Generators (`metaprogramming-iterators.html`)](#33-metaprogramming--advanced-primitives-proxy-reflect-symbol-iterators--generators)
- [4. Object-Oriented Programming (`oops`)](#4-object-oriented-programming-oops)
  - [4.1 Function Constructor and Inheritance (`functionConstructor.html`)](#41-function-constructor-and-inheritance)
  - [4.2 Classes (`class.html`)](#42-classes)
- [5. DOM, Browser APIs & Performance Optimization](#5-dom-browser-apis--performance-optimization)
  - [5.1 Browser Storage & Web Workers (`storage-and-workers.html`)](#51-browser-storage--web-workers)
  - [5.2 Observer APIs: Intersection, Resize & Mutation (`observer-apis.html`)](#52-observer-apis-intersection-resize--mutation)

---

# 1. JS Fundamental

## 1.1 Variables, Hoisting & TDZ
*Source file: `js fundamental/variables-hoisting-tdz.html`*

### Notes:
1. **Variable -** In JavaScript variables can be declared using `var`, `let` and `const`. `var` is not the recommended way. it should be rarely used.

2. **Difference Between Var, Let, Const -**
   - **Var -** `var` has functional scope. Means it can be accessed anywhere in the function it is defined. It can be re declared and re assigned also.
     If we declare a variable with `var` in global scope, means not inside any function it will work as global variable.
     The reason why it is not recommended is that, it can be re-declared and that can be a problem if multiple developers working on the same project.
     variables declared with `var` are hoisted to the top of their scope during the compilation phase, which means you can use a variable before it's declared in the code.
   - **Let -** `let` has block scope (enclosed by curly braces). It is introduced in ES6. We can not re-declare it but can re-assign. variables are hoisted to the top of their block but are not initialized until the actual declaration is encountered during runtime.
   - **Const -** Similar to `let`, `const` has block level scope. it is used for variables that should not be reassigned after initialization. However, it does not make the variable itself immutable; it just prevents reassignment. variables are hoisted to the top of their block but are not initialized untill the actual declaration is encountered during run time.

3. **Temporal Dead Zone -** The Temporal Dead Zone (TDZ) in JavaScript refers to the period between the start of the scope and the point where a variable is declared. During this period, attempting to access the variable will result in a `ReferenceError`. This concept is associated with variables declared using `let` and `const` declarations.

4. **Hoisting -** Whenever a code runs in javascript, it creates a global execution context. It has two phases, 1st Memory Allocation phase and the second is code execution phase.
   In memory allocation phase, first all variables and functions are stored in main memory with `key:value` pair. while storing the variables in main memory, first all variables are assigned a value `'undefined'`. functions are stored with complete body.
   So if we try to access a variable before its initialization, it will return `undefined`. It works same way for `let` and `const` also but because of the TDZ, it throw a reference error.

### Code:
```javascript
// lets try to access var a outside of the function test.
// console.log(a);    // ReferenceError: a is not defined

function test(){
    // try to see if var a is hoisted 
    console.log(a);       // Output : undefined

    var a = 9;
    // try to access let and const outside of block. Here b and c are consoled outside the block scope.
    // console.log(b);         // ReferenceError: b is not defined
    // console.log(c);         // ReferenceError: c is not defined

    if(a){

        // console.log(b); // console b before declaration. Output: ReferenceError: Cannot access 'b' before initialization

        let b = 10;
        const c = 2;
        // try to re-declare b and c 
        // let b = 30;   // SyntaxError: Identifier 'b' has already been declared
        // const c = 4;     // SyntaxError: Identifier 'c' has already been declared

        // lets try to re-assign both b and c
        b = 50;
        // c = 100;           //TypeError: Assignment to constant variable.

        console.log(b)        // Output: 50
       
    }
}

test()

// try to call fn before declaration
fn()

// test hoisting for function
function fn(){
    console.log('test from fn ');
}
```

---

## 1.2 Data Types
*Source file: `js fundamental/data-types.html`*

### Notes:
1. **Data Types in JS -** JavaScript is a dynamically-typed language, which means we don't need to define data type of a variable while declaring it. data type is assigned depending on value at run time.
   However, javascript has many data types which can be categorised mainly in two category:
   1. Primitive Data Type
   2. Non Primitive(Object) Data Type

2. **Primitive Data Type -** This is pass by value. Means if we assing one variable to other, it will copy the same value. For example -
   ```javascript
   let a = 26;
   let b;
   b = a;
   console.log(b); // output will be 26
   ```

   Number, String, BigInt, undefined, null, Boolean and Symbol are primitive data types. Symbol is introduced in ES6, symbols are unique and immutable data types. primary purpose of Symbol is to create guaranteed unique identifiers. Every time you call Symbol(), a completely unique value is generated, even if you pass the exact same description to it.

   ```javascript
   const sym1 = Symbol("id");
   const sym2 = Symbol("id");

   console.log(sym1 === sym2); // false
   ```

   #### **Important Notes:**
   - **A. `null` as an Object -** In JavaScript, **null** behaves like a primitive value (e.g., undefined, string, number), but it is actually an **object**. This is widely recognized as a historical bug in the language that has never been fixed to avoid breaking existing code.
   - **B. BigInt -** Introduced in ES2020, BigInt is a special numeric type in JavaScript that can represent whole numbers of arbitrary size—beyond the limits of the standard Number type (`Number.MAX_SAFE_INTEGER`). You create BigInt values by appending `n` to an integer literal or by calling the `BigInt()` function.
   - **C. `typeof null` -** The `typeof null` returning `'object'` is a long-standing bug in JavaScript. It was introduced in the early days of the language and has been maintained for backward compatibility. It does not reflect the actual internal structure of `null`, which is a primitive value.
   - **D. `typeof undefined` -** The `typeof undefined` returning `'undefined'` is correct. Undefined is a primitive data type in JavaScript. It is used to represent the absence of a value.
   - **E. `typeof BigInt` -** The typeof operator returning 'bigint' is correct. BigInt is a primitive data type in JavaScript used to represent integers with arbitrary precision that are too large to be represented by the normal Number data type
   - **F. `typeof Symbol` -** typeof operator returning 'symbol' is correct. Symbol is a primitive data type in JavaScript used to create unique and immutable identifiers for object properties.
   - **G. Auto-boxing -** Primitives don't have methods, yet you can call methods on them (like `"hello".toUpperCase()`). When you do this, JavaScript temporarily wraps (auto-boxes) the primitive value into its corresponding Object wrapper, runs the method, and then discards the wrapper.

3. **Non-Primitive(Object) Data Type -** Non-primitive are pass by reference. Means variables are stored in the main memory while values are stored in the heap memory. Both variable and values are connected to pointer. So if we assing one variable to other, it only assign the pointer, don't copy the value.
   Object, Array and functions are non-primitive data type.

---

## 1.3 Execution Context & Event Loop
*Source file: `js fundamental/execution-eventloop.html`*

### Execution Context Notes:
1. **Execution Context -** Execution context is a concept which represents an invironment in which javascript codes are executed.
   Every execution context has 2 phase - 1st Memory allocation and second Code execution.
   There are mainly two type of execution context - 1. Global Execution Context and 2. Functional Execution Context.

2. **Global Execution Context -** As soon as a javascript code starts running, a global execution context is created and added in the call stack or execution context.
   The outer most context is Global execution context. variables declared outside of functions are part of this context.

3. **Functional Execution Context -** in globa execution context, while code execution, if any function invocation is found, a functional execution context is crated.

![Execution Context Diagram](https://www.freecodecamp.org/news/content/images/2022/12/1.png)

### Event Loop, Call Stack and Callback Queue Notes:
javascript is single threaded and synchronous programming language. It means, it has only one execution stack which is also called as callstack to execute codes.
However, javascript also handle asynchronous functions. For that, it need to run all codes without blocking main thread.
event loop helps to handle these asynchronous operations.

![Runtime Environment](https://res.cloudinary.com/slawinski-dev/image/upload/v1648757381/javascript-runtime-environment.png)

1. **Call Stack or Execution Stack -** call stack handles multiple execution contexts. so as soon as an execution context is crated, it is pushed into the call stack. It is poped out after completion.
   If we have multiple execution context inside one another, then it is stacked on top of each other in the call stack.

2. **Callback Queue(Task Queue) -** Callback queue is a queue data structure. Queue works on FIFO(First in First Out) concept. Whenever a callback function found in the code, it is registered in the Web API environment and time start as given.
   As soon as timer finish and callback is ready to execute, it will be added in the callback queue.

3. **Micro Task Queue -** Similar to callback queue, all callback related to promises and mutation observer are added in Micro task queue. Micro task queue has high preority than the callback queue.

4. **Event Loop -** Only taks of the event loop is that it keeps checking callstack, micro task queue and callback queue. As soon as callstack is empty, it checks for microtask queue first, it get callback and push it to callstack for execution.
   once micro task queue is empty, it will check the callback queue and start pushing callbacks one by one to callstack for execution.

---

## 1.4 Closure
*Source file: `js fundamental/clouser.html`*

### Notes:
Function along with its lexical scope forms a closure. or in another word -
A closure is a function having access to the parent scope, even after the parent function has closed.

Clouser is used in - currying, memoize, state in the async, setTimeout.

```javascript
function fn1(){
    let a = 20;
    return function fn2(){
        console.log(a);
    }
}

let fn2 = fn1();   // here i returned the function fn2 out of function fn1. it has functin
                    // definition of fn2. 
fn2()              //now when i call the fn2, it wll still have the value of a:20
```

**Lexical Scope -**
Lexical scope is the ability for a function to access variables from its parent scope.

Example of lexical scope:
```javascript
function x(){
    let a = 10;

    function y(){
        console.log(a)   // output: 10
        let b = 5;

        function z(){
            console.log(a+b); // output: 15
        }
        z()
    }
    y()
}
x()
```

### Complete Code & Questions:
```javascript
// example of lexical scope
function x() {
    let a = 10;

    function y() {
        console.log(a)   // output: 10
        let b = 5;

        function z() {
            console.log(a + b); // output: 15
        }
        z()
    }
    y()
}
x()

// example of closure

function fn1(){
    let a = 20;
    return function fn2(){
        console.log(a);
    }
}

let fn2 = fn1();   // here i returned the function fn2 out of function fn1. it has functin
                    // definition of fn2. 
fn2()              //now when i call the fn2, it wll still have the value of a:20

/**
 * 
 * One use of closure is currying functions. let see -
 * 
 * */

// currying functions takes only one argument at a time.
function add(a){
    return function x(b){
        return function y(c){
            return a+b+c;
        }
    }
}

let sum = add(2)(3)(4);
console.log('currying sum - '+ sum)

/**
 * Q.- create a function if sum passed, it add next two arguments
 *     if subtract pass, subtract those and similarly for multiply and division
 * 
 * Example - add('sum')(2)(3) // output: 5
 *           add('substract')(5)(3) // output: 2
 *          add('multiply')(2)(3) // output: 6
 *          add('divide')(10)(5) // output: 2
 * 
 * */

 function add(operation){
    return function x(a){
        return function y(b){
            if(operation === 'sum'){
                return a+b
            }else if(operation === 'substract'){
                return a-b;
            }else if(operation === 'multiply'){
                return a*b;
            }else if(operation === 'divide'){
                return a/b;
            }
        }
    }
 }

 let result = add('multiply')(3)(4);
 console.log('currying question first-  '+result) // output: 12
```

---

## 1.5 This Keyword & Explicit Binding (`call`, `apply`, `bind`)
*Source file: `js fundamental/this-keyword.md`*

### # This in Global Space
This in Global space returns Global object which can be different depending on where you are running the code. 
For example, in a browser it will return Window object, inside Node JS it will return Global.

```javascript
console.log(this)
```

### ## This keyword works differently in strict and non strict mode
- In non strict mode:
```javascript
function test(){
 console.log(this)  // output: Window object
}
```

- In Strict mode:
```javascript
function test(){
 console.log(this)  // output: undefined
}
```

---

### ## Explicit Binding: `call()`, `apply()`, and `bind()`

In JavaScript, `call`, `apply`, and `bind` are built-in methods on `Function.prototype` used to explicitly control and set what the `this` keyword refers to inside a function. This technique is often referred to as **Function Borrowing**.

#### 1. `call()`
- **Concept:** Invokes the function immediately, binding `this` to the passed object. Arguments are passed **individually** (comma-separated).
- **Syntax:** `fn.call(thisContext, arg1, arg2, ...)`

```javascript
const person1 = {
    fname: "Mrityunjay",
    lname: "Mishra"
};

function printDetails(hometown, state) {
    console.log(`${this.fname} ${this.lname} from ${hometown}, ${state}`);
}

// Function borrowing using call
printDetails.call(person1, "Varanasi", "UP");
// Output: Mrityunjay Mishra from Varanasi, UP
```

#### 2. `apply()`
- **Concept:** Exactly the same as `call()`, invokes the function immediately with the bound `this` context, but arguments must be passed as an **Array** (or array-like object).
- **Syntax:** `fn.apply(thisContext, [arg1, arg2, ...])`

```javascript
// Function borrowing using apply (arguments as array)
printDetails.apply(person1, ["Varanasi", "UP"]);
// Output: Mrityunjay Mishra from Varanasi, UP
```

#### 3. `bind()`
- **Concept:** Does **NOT** invoke the function immediately. Instead, it returns a **brand new function** permanently bound to the specified `this` context and preset arguments, which can be called later.
- **Syntax:** `const boundFn = fn.bind(thisContext, arg1, arg2, ...)`

```javascript
// Returns a new function copy with 'this' permanently bound
const printLater = printDetails.bind(person1, "Varanasi", "UP");

printLater(); 
// Output: Mrityunjay Mishra from Varanasi, UP
```

#### Quick Comparison Table:
| Method | Immediate Invocation? | Argument Passing | Return Value |
| :--- | :--- | :--- | :--- |
| **`call()`** | **Yes** | Individual comma-separated arguments (`arg1, arg2`) | Returns the result of the function execution |
| **`apply()`** | **Yes** | Arguments bundled inside an Array (`[arg1, arg2]`) | Returns the result of the function execution |
| **`bind()`** | **No** (Deferred) | Individual comma-separated arguments | Returns a **new function** bound to `this` |

*(Note: Custom implementations for these methods are available under [Section 3.2: Important Polyfills](#32-important-polyfills): `myCall`, `myApply`, and `myBind`)*

---

## 1.6 Event Handling: Bubbling, Capturing & Event Delegation
*Source file: `js fundamental/event-handling.html`*

### Notes:
Event listner accepts 3 parameters -
1. **Event Name -** for example: click, mouseover,etc
2. **Callback Function -** A callback function which executes on event
3. **useCapture flag -** it takes true/false. By default its false means bubbling. if you want to use capturing, then make it true.

To stop event propogation, we can use `event.stopPropagation()`

1. **Event Bubbling -** event Bubbling is the concept where event flow from the clicked element to upward direction through its ansesstors. Example:
```javascript
document.querySelector('#child').addEventListener('click',function(){
    console.log('child clicked');
},false) // add false or leave blank
```

2. **Event Capturing -** In event capturing, event flow from root element to the click element. Example:
```javascript
document.querySelector('#child').addEventListener('click',function(){
    console.log('child clicked');
},true)
```

3. **Event Delegation -** Attaching a single listener to a parent node to manage dynamic children efficiently.
   - **How it works:** Instead of attaching event listeners to every individual child element, we attach a single listener to their common ancestor. Because of **Event Bubbling**, any event triggered on a child bubbles up to the parent.
   - **Key Advantages:**
     - **Memory Optimization:** 1 listener on the parent container instead of 1,000+ listeners on individual items.
     - **Handles Dynamic Elements:** Elements added to the DOM dynamically (via AJAX, infinite scroll, or user actions) work automatically without needing new listeners attached.
     - **Less Cleanup:** Fewer listeners reduce the risk of memory leaks when child nodes are removed.

### Code Example:
```javascript
// Bubbling vs Capturing playground
document.querySelector('#grand-parent').addEventListener('click',()=>{
    console.log('grand parent clicked')
},true)

document.querySelector('#parent').addEventListener('click',()=>{
    console.log('parent clicked')
})

document.querySelector('#child').addEventListener('click',()=>{
    console.log('Child clicked')
})

// -------------------------------------------------------------
// Event Delegation Example
// -------------------------------------------------------------
// Attaching a single listener to the parent <ul> container
document.querySelector('#item-list').addEventListener('click', (event) => {
    // event.target is the actual clicked element
    // event.target.closest('li') ensures clicks on nested <span> or <strong> tags still select the <li>
    const listItem = event.target.closest('li');

    if (listItem && event.currentTarget.contains(listItem)) {
        console.log('Handled item click:', listItem.textContent, 'ID:', listItem.dataset.id);
    }
});
```

---

## 1.7 Debouncing & Throttling
*Source file: `js fundamental/debouncing-throttling.html`*

### Notes:
It is used for performance optimization. it can be used where we have frequent events like resizing, input events or scrolling.
It is used to control the rate at which a particular function is executed.

1. **Debouncing -** In Debouncing we set a particular time, if no event is triggered within the time, function will be called.
   So what it does is, it will not contineously call the function if event is triggering frequently. It will call the fun only if event is not triggered till the time set.

2. **Throttling -** In Throttling, instead of waiting for a particular time to call the function, we call function on a specific interval if contineous event triggering is happening.

- `=>` Use debouncing when you want to delay the execution of a function until a pause in events occurs.
- `=>` Use throttling when you want to limit the rate at which a function is executed, ensuring it doesn't happen more frequently than a specified interval.

### Code:
```javascript
// Debounce implementation: executes only after user pauses typing for 'delay' ms
function debounce(callback, delay = 500) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      callback.apply(this, args);
    }, delay);
  };
}

// Throttle implementation: executes at most once every 'limit' ms interval
function throttle(callback, limit = 500) {
  let lastRan = 0;
  return function (...args) {
    let now = Date.now();
    if (now - lastRan >= limit) {
      callback.apply(this, args);
      lastRan = now;
    }
  };
}

// Create debounced and throttled update functions (closures maintain state across events)
const updateDebounce = debounce(function (val) {
  document.getElementById('debounce').innerHTML = `<strong>Debouncing</strong>: ${val}`;
}, 500);

const updateThrottle = throttle(function (val) {
  document.getElementById('throttle').innerHTML = `<strong>Throttling</strong>: ${val}`;
}, 500);

document.addEventListener('input', function (event) {
  let val = event.target.value;

  // Normal: updates immediately on every keystroke
  document.getElementById('normal').innerHTML = `<strong>Normal</strong>: ${val}`;

  // Debounced: updates after pause in typing
  updateDebounce(val);

  // Throttled: updates at regular intervals while typing
  updateThrottle(val);
});
```

---

# 2. Asynchronous JavaScript (`asynjs`)

## 2.1 CallBack
*Source file: `asynjs/callback.html`*

### Notes:
1. **Callback -** A callback function is a function passed into another function as an argument, which is then invoked inside the outer function to complete some kind of routine or action.
2. **Problem in Callback -** 1st problem with callback function is the callback hell and the second problem is inversion of control.
   In inversion of control, we loose our control from the code because we are passing the callback function in another function and don't know what inside that function, when that will execute, execute or not. so we are blindly trusting on that.
3. **Callback Hell -** When we have nested callbacks, the code structure grows horizontally. This type of code is know as callack hell or piramid of doom. Callback hell can be fixed with promise chaining.

### Code:
```javascript
// example of callback fun
function moringTime() {
    console.log('its morning time');
    function wakeup() {
        console.log('Good Morning');
    }
    wakeup()
}

moringTime()


// second example of callback

function nightTime(greeting, callback) {
    console.log(greeting);
    callback()
}

function sleep() {
    console.log('sleeping')
}

nightTime('Good Night', sleep)


// Simulated async functions using callbacks
function getUser(id, callback) {
    setTimeout(() => {
        console.log("Fetched user:", id);
        callback({ id: id, name: "Mrityunjay" });
    }, 1000);
}

function getPosts(userId, callback) {
    setTimeout(() => {
        console.log("Fetched posts for userId:", userId);
        callback([
            { id: 1, title: "JavaScript Revision" },
            { id: 2, title: "Understanding Closures" }
        ]);
    }, 1000);
}

function getComments(postId, callback) {
    setTimeout(() => {
        console.log("Fetched comments for postId:", postId);
        callback(["Great explanation!", "Very helpful article!"]);
    }, 1000);
}

// Creating Callback Hell (Pyramid of Doom)
getUser(101, (user) => {
    getPosts(user.id, (posts) => {
        getComments(posts[0].id, (comments) => {
            console.log("First comment on latest post:", comments[0]);
        });
    });
});

// -------------------------------------------------------------
// How to fix Callback Hell:
// -------------------------------------------------------------

// Step A: Convert the operations to return Promises
function getUserPromise(id) {
    return new Promise((resolve) => {
        setTimeout(() => resolve({ id: id, name: "Mrityunjay" }), 1000);
    });
}

function getPostsPromise(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([
                { id: 1, title: "JavaScript Revision" },
                { id: 2, title: "Understanding Closures" }
            ]);
        }, 1000);
    });
}

function getCommentsPromise(postId) {
    return new Promise((resolve) => {
        setTimeout(() => resolve(["Great explanation!", "Very helpful article!"]), 1000);
    });
}

// Solution 1: Fixing Callback Hell using Promise Chaining
getUserPromise(101)
    .then((user) => getPostsPromise(user.id))
    .then((posts) => getCommentsPromise(posts[0].id))
    .then((comments) => {
        console.log("First comment on latest post:", comments[0]);
    })
    .catch((error) => {
        console.error(error);
    });

// Solution 2: Modern fix using Async / Await
async function displayLatestComment() {
    try {
        const user = await getUserPromise(101);
        const posts = await getPostsPromise(user.id);
        const comments = await getCommentsPromise(posts[0].id);
        console.log("First comment on latest post:", comments[0]);
    } catch (error) {
        console.error(error);
    }
}
// displayLatestComment();
```

---

## 2.2 Promise
*Source file: `asynjs/promise.html`*

### Notes:
1. **Promise -** A promise is an object which represents an eventual completion or failure of an asynchronus operation. It has three state - pending, fulfilled and rejected. when we create promise, it returns an object which status pending and a value of undefined.
2. **Why Promise -** Before promise, callback functions were used for asynchronous operations. The major problem with that is:
   1. If we chain multiple callback functions inside one another it creates a piramid like structure which is called callback hell or piramid of doom.
   2. Inversion of Control means we hand over our callback function to an external API and blindly trust it to execute our code properly—it might call our callback twice, never call it, or swallow errors.
3. **Promise.all -** Promise.all APi takes an iterable or we can say array of promises and execute them parallely. It waits for all promise to resolve and returns an array of result. If any promise is rejected, it immediately throws an error.
   it is also known as fail fast and all or none.
4. **Promise.allSetteled -** It is similar to Promise.all but the difference is that even if a promise reject, it still wait for all other promises to settle and at the end it returns an array with all results. So it is not going to throw an error and stop if any promise rejected. It will wait for all to finish and settle.
5. **Promise.race -** It works like a race. So it will give the result of the promise which settled first. No matter whether it is rejected or resolved.
6. **Promise.any -** it is first success seeking API. it waits for the first resolved promise and return the result. If all promise rejected, it returns an agreegate error.
   if you want to see all errors, do like this -
   `catch((err)=>{console.log(err.errors)})`

### Code:
```javascript
// creating a program for coffeesho using promise

const coffeeshop = {
    coffee: ['capachino', 'late'],
    isOpen: true
}

function orderCoffee(coffeeName) {
    let order = new Promise((resolve, reject) => {
        if (coffeeName === 'capachino' || coffeeName === 'late') {
            resolve('order placed');
        } else {
            const err = new Error(`${coffeeName} is not available now`);
            reject(err)
        }
    })
    return order;
}

let placedOrder = orderCoffee('ed');
placedOrder.then((result) => {
    console.log(result)
})
    .catch((error) => {
        console.log(error.message);
    })


// code for promise API(promise.all, allSetteled, any, reace)

//lets create some dummy promises first
let p1 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('p1 resolved')
    },3000)
})

let p2 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        reject('p2 resolved')
    },2000)
})

let p3 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('p3 resolved')
    },1000)
})

// lets test Promise.all with the above dummy promises

Promise.all([p1,p2,p3]).then((result)=>{
    console.log(result)
})
.catch((err)=>{
    console.error(err)
})
```

---

## 2.3 Async Await
*Source file: `asynjs/async-await.html`*

### Notes:
1. **Asyn Function -** Asyn function always return a promise. if you hava a promise written inside the fun then it will return that. If its a simple code, it will wrap that in a promise and then return it.
   `await` can only be used inside async function.
2. **How async and await works behind the scene -** whenever js engine see an await inside async function, it suspends the function execution from callstack. once promise is resolved, that function will come again in call stack and execute from where it left.
3. **Error Handeling -** We use try catch to handle error while using async and await we can also use `.catch` as we were doing with Promise.

### Code:
```javascript
// create asyn function and handle error using try catch

let p1 = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        reject('Promise resolved after 2 sec');
    },2000)
})


async function Test(){
    try {
        console.log('asyn function resolved');
        let data = await p1;
        console.log(data)
    } catch (error) {
        console.error(error)
    }
    
}
Test()
```

---

## 2.4 AbortController & Fetch API: Canceling In-Flight Requests and Handling Race Conditions
*Source file: `asynjs/abort-controller.html`*

### Notes:
1. **AbortController Overview -**
   `AbortController` is a built-in browser Web API that allows you to abort one or more asynchronous Web operations (such as `fetch()` network requests) on demand.
   - It exposes a `signal` property (`AbortSignal`) which is passed into the options of `fetch(url, { signal })`.
   - Calling `controller.abort()` immediately cancels the request and causes the fetch promise to reject with a `DOMException` named `'AbortError'`.

2. **Why is it critical in Modern Web Apps? -**
   - **1. Canceling In-Flight Network Requests:** If a user navigates away from a page, closes a modal, or cancels an action, pending background requests waste network bandwidth, battery, and server resources unless canceled.
   - **2. Preventing Race Conditions in Live Search / Autocomplete:**
     - *The Problem:* A user types `"cat"` (Request A fired, slow network, takes 1500ms). The user then quickly types `"dog"` (Request B fired, fast network, resolves in 300ms). Request B finishes first and displays `"dog"` results. A moment later, slow Request A finishes and overwrites the UI with `"cat"` results!
     - *The Solution:* Store the active `AbortController`. Before firing a new search request, abort the previous in-flight controller (`searchController.abort()`). Only the latest request is allowed to settle and update the UI.
   - **3. Request Timeouts:** Automatically cancel requests that exceed a specified duration using `AbortSignal.timeout(ms)` or a fallback `setTimeout`.

3. **Handling the Abort Error -**
   When a fetch request is aborted, it rejects with `error.name === 'AbortError'`. Always check for this in the `catch` block so intentional cancellations are not logged as actual network failures:
   ```javascript
   catch (error) {
       if (error.name === 'AbortError') {
           console.log('Request was aborted intentionally');
       } else {
           console.error('Real network/runtime error:', error);
       }
   }
   ```

### Code:
```javascript
// Example 1: Basic Cancellation with AbortController
let controller = new AbortController();

async function fetchPosts() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
            signal: controller.signal
        });
        const data = await response.json();
        console.log('Data:', data);
    } catch (error) {
        if (error.name === 'AbortError') {
            console.log('Fetch request was aborted by the user.');
        } else {
            console.error('Fetch failed:', error);
        }
    }
}

// Cancel the request at any time:
// controller.abort();


// Example 2: Handling Race Conditions in Live Search / Autocomplete
let searchController = null;

async function handleSearch(query) {
    // 1. Cancel previous pending request if still in-flight
    if (searchController) {
        searchController.abort();
    }

    // 2. Create fresh controller for the current request
    searchController = new AbortController();

    try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?q=${encodeURIComponent(query)}`, {
            signal: searchController.signal
        });
        const results = await response.json();
        console.log(`Results for "${query}":`, results);
    } catch (err) {
        if (err.name === 'AbortError') {
            console.log(`Previous search request for "${query}" canceled.`);
        } else {
            console.error('Search error:', err);
        }
    }
}


// Example 3: Modern Request Timeout (ES2022+ AbortSignal.timeout)
async function fetchWithTimeout(url, duration = 3000) {
    try {
        const response = await fetch(url, {
            signal: AbortSignal.timeout(duration) // Automatically aborts after duration ms
        });
        return await response.json();
    } catch (err) {
        if (err.name === 'TimeoutError' || err.name === 'AbortError') {
            console.error('Request timed out after', duration, 'ms');
        } else {
            console.error('Network error:', err);
        }
    }
}
```

---

# 3. Functional Programming

## 3.1 Higher-Order Functions (HOFs): Array Transformations & Immutability Methods
*Source file: `functional programming/hof-transformations.html`*

### Notes:
1. **Higher-Order Function (HOF) -**
   A Higher-Order Function is a function that either takes one or more functions as arguments (callbacks), returns a function as its result, or does both.
   - Built-in array methods like `map`, `filter`, `reduce`, `flatMap`, and `forEach` are classic HOFs.
   - Utilities like `debounce` and `throttle` are also HOFs because they accept a function and return an optimized function.

2. **Mastering Array Transformations -**
   - **`map(callback)`:** Transforms every element in an array and returns a brand-new array of identical length without mutating the original.
   - **`filter(callback)`:** Evaluates each element against a predicate function and returns a new array with items returning truthy.
   - **`reduce(callback, initialValue)`:** Iterates through the array and accumulates values down to a single output (number, object, grouped arrays).
   - **`flat(depth)`:** Flattens nested multi-dimensional arrays recursively up to the specified `depth` (default is 1; pass `Infinity` to deeply flatten arbitrary nesting).
   - **`flatMap(callback)`:** Maps each element using a transformation function and then flattens the result by 1 level. More performant and concise than chaining `.map().flat()`.

3. **Modern Immutability Methods (ES2023 / ES14) -**
   - **The Mutation Trap:** Traditional methods like `sort()`, `reverse()`, and `splice()` mutate the original array in place. In functional programming and modern UI frameworks like React, mutating state directly causes missed re-renders and difficult-to-trace bugs.
   - **The Non-Mutating Alternatives:**
     - **`toSorted(compareFn)`:** Returns a sorted copy without altering the original array.
     - **`toReversed()`:** Returns a reversed copy without altering the original array.
     - **`toSpliced(start, deleteCount, ...items)`:** Returns a new copy with elements inserted, removed, or replaced without altering the original array.
     - **`with(index, value)`:** Returns a new array with the element at `index` replaced by `value`.

### Code:
```javascript
// -------------------------------------------------------------
// 1. Array Transformations: map, filter, reduce
// -------------------------------------------------------------
const products = [
    { id: 1, name: "Laptop", category: "Electronics", price: 60000 },
    { id: 2, name: "Shirt", category: "Clothing", price: 1500 },
    { id: 3, name: "Phone", category: "Electronics", price: 40000 },
    { id: 4, name: "Jeans", category: "Clothing", price: 2500 }
];

// Pipeline: Filter electronics -> Apply 10% discount -> Sum total price
const totalDiscountedElectronics = products
    .filter(item => item.category === "Electronics")
    .map(item => item.price * 0.9)
    .reduce((total, price) => total + price, 0);

console.log("Total discounted electronics:", totalDiscountedElectronics); // 90000


// -------------------------------------------------------------
// 2. flat() and flatMap()
// -------------------------------------------------------------
// flat: flattens multi-dimensional nested arrays
const nested = [1, [2, [3, [4, 5]]]];
console.log(nested.flat());          // [1, 2, [3, [4, 5]]] (depth 1)
console.log(nested.flat(2));         // [1, 2, 3, [4, 5]]
console.log(nested.flat(Infinity));  // [1, 2, 3, 4, 5] (deep flatten)

// flatMap: transform each item and flatten by 1 level
const orders = [
    { orderId: 101, items: ["Laptop", "Mouse"] },
    { orderId: 102, items: ["Keyboard"] },
    { orderId: 103, items: ["Monitor", "HDMI Cable"] }
];

// Extracting all individual items across all orders into a single flat list:
const allItems = orders.flatMap(order => order.items);
console.log("All items flatMapped:", allItems);
// Output: ["Laptop", "Mouse", "Keyboard", "Monitor", "HDMI Cable"]


// -------------------------------------------------------------
// 3. Immutability Methods (ES2023): toSorted, toReversed, toSpliced, with
// -------------------------------------------------------------
const numbers = [5, 2, 9, 1, 7];

// --- A. toSorted() vs sort() ---
// numbers.sort() mutates numbers!
const sortedNumbers = numbers.toSorted((a, b) => a - b);
console.log("toSorted:", sortedNumbers);             // [1, 2, 5, 7, 9]
console.log("Original untouched:", numbers);         // [5, 2, 9, 1, 7]

// --- B. toReversed() vs reverse() ---
// numbers.reverse() mutates numbers!
const reversedNumbers = numbers.toReversed();
console.log("toReversed:", reversedNumbers);         // [7, 1, 9, 2, 5]
console.log("Original untouched:", numbers);         // [5, 2, 9, 1, 7]

// --- C. toSpliced() vs splice() ---
// numbers.splice(1, 2, 99) mutates numbers!
// toSpliced(startIndex, deleteCount, ...itemsToAdd)
const splicedNumbers = numbers.toSpliced(1, 2, 99);
console.log("toSpliced:", splicedNumbers);           // [5, 99, 1, 7]
console.log("Original untouched:", numbers);         // [5, 2, 9, 1, 7]

// --- D. with() for single element update ---
const updatedNumbers = numbers.with(2, 999);
console.log("with(index, val):", updatedNumbers);    // [5, 2, 999, 1, 7]
console.log("Original untouched:", numbers);         // [5, 2, 9, 1, 7]
```

---

## 3.2 Important Polyfills
*Source file: `functional programming/polyfills.html`*

### 1. Polyfill of map function
- 1. it takes an array
- 2. it returns an array
- 3. runs a callback function for each element of the array

```javascript
Array.prototype.myMap = function(callback){
    let res = [];
    for(let i=0; i<this.length; i++){
        let op = callback(this[i]);
        res.push(op);
    }

    return res;
}
```

### 2. Polyfill of filter function
- 1. it takes an array
- 2. it returns an array
- 3. runs a callback function for each element of the array

```javascript
Array.prototype.myFilter = function(callback){
    let res = [];
    for(let i=0; i<this.length; i++){
        if(callback(this[i])){
            res.push(this[i]);
        }
    }
    return res;
}
```

### 3. Polyfill of forEach function
- 1. it takes an array
- 2. it returns element one by one
- 3. runs a callback function for each element of the array

```javascript
Array.prototype.myForEach = function(callback){

    for(let i=0; i<this.length; i++){
        callback(this[i]);
    }
}
```

### 4. Polyfill of reduce function
- 1. it takes an array
- 2. it returns a single element
- 3. runs a callback function for each element of the array
- 4. it takes an initial value which is optional
- 5. in the callback it takes - accumulator and current value

```javascript
Array.prototype.myReduce = function(callback, initial){
    let acc;
    let firstIdx;

    if(arguments.length === 1){
        acc = this[0];
        firstIdx = 1;
    }else{
        acc = initial;
        firstIdx = 0
    }

    for(let i=firstIdx; i<this.length; i++){
        acc = callback(acc,this[i]);
    }

    return acc;
}
```

### 5. Polyfill of call function
- 1. it takes an object, arguments as iterable
- 2. it call the functions for provided object

```javascript
Function.prototype.myCall = function(context={}, ...args){
    if(typeof this !== 'function'){
        throw new Error(this+'is not callable');
    }
    context.fn = this;
    context.fn(...args)
}
```

### 6. Polyfill of apply function
- 1. it takes an object,
- 2. it takes array as argument
- 3. it call the functions for provided object

```javascript
Function.prototype.myApply = function(context={}, arg=[]){
    if(typeof this !== 'function'){
        throw new Error(this+' is not callable')
    }
    if(!Array.isArray(arg)){
        throw new Error(arg + ' is not an array');
    }

    context.fn = this;
    context.fn(...arg)
}
```

### 7. Polyfill of bind function
- 1. it takes an object,
- 2. it takes multiple arguments
- 3. it returns a function binded with the object

```javascript
Function.prototype.myBind = function(context = {}, ...args) {
    if (typeof this !== 'function') {
        throw new Error(this + ' is not callable');
    }

    const originalFunction = this;

    return function (...args2) {
        return originalFunction.apply(context, [...args,...args2]);
    };
};
```

### 8. Polyfill of promise.all function
- 1. it takes an an array of promise,
- 2. if all promises are resolved, it returns array with output
- 3. if any promise rejects, it returns error with rejected promise

```javascript
Promise.myAll = function(promiseArr){
    if(!Array.isArray(promiseArr)){
        throw new Error(promiseArr + ' is not an array');
    }

    return new Promise((resolve, reject)=>{
        let result = [];
        let count = 0;

        for(let i=0; i<promiseArr.length; i++){
            promiseArr[i].then((data)=>{
                result[i] = data;
                count++;

                if(count === promiseArr.length){
                    resolve(result);
                }

            }).catch((error)=>{
                reject(error);
            })
        }

        if(promiseArr.length === 0){
            resolve(result)
        }
    })
}

// Test promises
let p1 = new Promise((resolve, reject)=>{
    resolve('first promise resolved');
})
let p2 = new Promise((resolve, reject)=>{
    resolve('second promise resolved');
})
let p3 = new Promise((resolve, reject)=>{
    resolve('third promise resolved');
})

Promise.myAll([]).then((data)=>{
    console.log(data)
}).catch(err=>{
    console.error(err)
})
```

---

## 3.3 Metaprogramming & Advanced Primitives: Proxy, Reflect, Symbol, Iterators & Generators
*Source file: `functional programming/metaprogramming-iterators.html`*

### Notes:

#### 1. Metaprogramming with Proxy & Reflect
- **What is Metaprogramming?** Writing code that inspects, intercepts, or modifies the language's fundamental runtime behaviors.
- **`Proxy`:** Wraps a target object and intercepts internal operations through "traps" (e.g. `get`, `set`, `has`, `deleteProperty`, `apply`).
- **`Reflect`:** Companion object that provides the default underlying language operations. Always use `Reflect.get()` and `Reflect.set()` inside Proxy traps because they properly pass the `receiver` argument, ensuring prototype chain lookups and `this` bindings remain correct.
- **Under the Hood in State Management Libraries (Vue 3, MobX, Immer):**
  - **`get` trap:** Used for **Dependency Tracking (Subscriber Collection)** — records which component/computed property accessed the state.
  - **`set` trap:** Used for **Triggering Updates** — detects mutations and automatically triggers batch re-renders of dependent components without requiring `this.setState()`.

#### 2. Symbol & Well-Known Symbols
- `Symbol()` creates an immutable, completely unique primitive value. Even two symbols created with the same description are strictly non-equal (`Symbol("key") !== Symbol("key")`).
- **Hidden Object Keys:** Symbols do not appear in `for...in` loops or `Object.keys()`, making them ideal for internal/private metadata.
- **Well-Known Symbols:** JavaScript exposes built-in hooks to override standard language behavior:
  - `Symbol.iterator`: Defines the default iterator consumed by `for...of` loops, the spread operator `...`, and destructuring.
  - `Symbol.toPrimitive`: Customizes how an object converts to a primitive (number/string).
  - `Symbol.hasInstance`: Customizes the behavior of the `instanceof` operator.

#### 3. Iterators & The Iterable Protocol
JavaScript defines an iteration contract with two protocols:
1. **The Iterable Protocol:** An object must have a method at `[Symbol.iterator]()` that returns an iterator object.
2. **The Iterator Protocol:** The returned iterator object must implement a `next()` method that returns an object with:
   - `value`: The current iteration value.
   - `done`: Boolean indicating if iteration is complete (`true` when finished).
- Arrays, Strings, Sets, and Maps implement this protocol natively.

#### 4. Generators (`function*` and `yield`)
- A generator is a special function that can pause execution midway (using `yield`) and be resumed later on demand (using `.next()`).
- **Lazy Evaluation:** Generates values one at a time as requested, avoiding loading massive or infinite data sequences into memory all at once.
- Calling a generator function does not run its body immediately; it returns a **Generator Object** conforming to both the Iterable and Iterator protocols.

### Code:
```javascript
// =============================================================
// 1. Proxy & Reflect: Reactive State Management Pattern
// =============================================================
function createObservable(targetObj, onStateChange) {
    return new Proxy(targetObj, {
        get(target, prop, receiver) {
            console.log(`[READ] Accessed: "${String(prop)}"`);
            return Reflect.get(target, prop, receiver);
        },
        set(target, prop, value, receiver) {
            const oldValue = target[prop];
            const success = Reflect.set(target, prop, value, receiver);
            if (success && oldValue !== value) {
                console.log(`[MUTATION] "${String(prop)}" changed:`, oldValue, "->", value);
                onStateChange(prop, value); // Notify UI to re-render
            }
            return success;
        }
    });
}

// Usage:
const appState = createObservable({ counter: 0, user: "Mrityunjay" }, (prop, val) => {
    console.log(`UI Update Triggered: ${prop} is now ${val}`);
});

console.log(appState.counter); // Trigger get trap
appState.counter = 5;          // Trigger set trap and notify subscriber


// =============================================================
// 2. Custom Iterable with Symbol.iterator
// =============================================================
const range = {
    start: 1,
    end: 3,
    [Symbol.iterator]() {
        let current = this.start;
        const last = this.end;
        return {
            next() {
                if (current <= last) {
                    return { value: current++, done: false };
                } else {
                    return { value: undefined, done: true };
                }
            }
        };
    }
};

// Consumed by for...of and spread operator:
for (let num of range) {
    console.log("Range item:", num); // 1, 2, 3
}
console.log("Spread range into array:", [...range]); // [1, 2, 3]


// =============================================================
// 3. Generators: Infinite ID Generator & Lazy Sequences
// =============================================================
function* idGenerator() {
    let id = 1001;
    while (true) {
        yield `ID_${id++}`; // Pauses execution until next() is requested
    }
}

const idGen = idGenerator();
console.log(idGen.next().value); // "ID_1001"
console.log(idGen.next().value); // "ID_1002"
console.log(idGen.next().value); // "ID_1003"
```

---

# 4. Object-Oriented Programming (`oops`)

## 4.1 Function Constructor and Inheritance
*Source file: `oops/functionConstructor.html`*

### Notes:
1. **Constructor Function -**
   - A Constructor Function is a regular function used as a blueprint to create multiple objects with similar properties and methods.
   - By convention, constructor function names start with a capital letter (e.g., `Person`, `Students`).
   - It is always invoked using the `new` keyword.

2. **What the `new` Keyword Does Behind the Scenes -**
   When a constructor function is invoked with `new`, JavaScript performs 4 steps:
   1. **Creates an empty object:** An empty object `{}` is created in memory.
   2. **Links Prototype:** Sets the internal `__proto__` of this new object to the constructor's `prototype` (`newObj.__proto__ = Person.prototype`).
   3. **Binds `this`:** Binds `this` inside the constructor to point to the newly created object.
   4. **Executes and Returns:** Executes the code inside the constructor function (attaching properties to `this`), and returns the new object automatically (unless another object is explicitly returned).

3. **Inheritance using `call()` (Constructor Stealing / Borrowing) -**
   - To achieve inheritance with constructor functions, we use `Person.call(this, fname, lname, age)`.
   - `call(this, ...)` executes the `Person` constructor by explicitly passing the `this` context of the newly created `Student` instance.
   - This initializes `fname`, `lname`, and `age` on the student object, allowing `Student` to reuse `Person`'s initialization logic.
   - `Students` then attaches its own specific properties (`this.psp`, `this.batch`).

4. **Important Interview Concept: Methods inside Constructor vs on Prototype -**
   - In the snippet below, `this.greeting = function() { ... }` is defined **inside** the constructor.
   - **Drawback:** Every time a new student instance is created, a brand new copy of the `greeting` function is allocated in memory. If you have 10,000 instances, there will be 10,000 separate function copies.
   - **Optimization (Prototypal Inheritance):** Shared methods should be placed on `Person.prototype` so that all instances share a single function reference via the prototype chain:
     ```javascript
     // Memory-efficient: Defined once, shared across all instances
     Person.prototype.greeting = function() {
         return `hello ${this.fname}`;
     };

     // Linking prototypes for full prototypal inheritance:
     Students.prototype = Object.create(Person.prototype);
     Students.prototype.constructor = Students;
     ```

### Code:
```javascript
function Person(fname, lname, age){
    this.fname = fname,
    this.lname = lname,
    this.age = age

    this.greeting = function(){
       return `hello ${fname}`
    }
}

function Students(fname,lname,age,psp,batch){
    Person.call(this,fname,lname,age)
    this.psp = psp,
    this.batch = batch
}

let student1 = new Students('Mrityunjay','Mishra',27,81.00,'full stack')
console.log(student1.greeting())
```

---

## 4.2 Classes
*Source file: `oops/class.html`*

### Code:
```javascript
//Creating class
class Person{
    constructor(fname,lname,age,occupation){
        this.fname = fname,
        this.lname = lname,
        this.age = age,
        this.occupation = occupation
    }
    walk(){
        console.log(`${this.fname} ${this.lname} is walking`);
    }

    // Defining getter function 
    get Fullname(){
        console.log(`Full Name -> ${this.fname} ${this.lname}`)
    }

    // Defining setter function
    set newAge(age){
        this.age = age;
    }
}

// creating object
let Mrityunjay = new Person('Mrityunjay','Mishra',28, 'web developer');

console.log(Mrityunjay.age)
Mrityunjay.Fullname;

Mrityunjay.newAge = 27;
console.log(Mrityunjay.age)

// Inheritance of class

class Students extends Person{
    constructor(fname,lname,age,occupation,psp,batch){
        super(fname,lname,age,occupation);
        this.psp = psp;
        this.batch = batch;
    }
}

let student1 = new Students('Mukesh','Singh',28, null, 99.00, 'fullstack')
console.log(student1)
student1.walk()
```

---

# 5. DOM, Browser APIs & Performance Optimization

## 5.1 Browser Storage & Web Workers
*Source file: `browser-apis-performance/storage-and-workers.html`*

### Notes:

#### 1. Browser Storage Comparison
| Storage Type | Capacity | Lifetime | Sent with HTTP Requests? | Best Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **`localStorage`** | ~5MB - 10MB | Persistent until explicitly deleted | No | User UI preferences, themes, non-sensitive cached data |
| **`sessionStorage`** | ~5MB | Scoped to active browser tab (cleared on tab close) | No | Multi-step form wizards, temporary single-tab state |
| **`Cookies`** | ~4KB | Manually set via `Expires` / `Max-Age` | **Yes** (Sent automatically on every matching HTTP request header) | Authentication session tokens, tracking IDs |
| **`IndexedDB`** | >250MB+ (Gigabytes) | Persistent | No | Large structured datasets, binary files, offline PWA database |

#### 2. Cookie Security Flags (Essential Interview Topic)
- **`HttpOnly`:** Prevents client-side JavaScript from accessing the cookie via `document.cookie`. Crucial defense against session token theft via **Cross-Site Scripting (XSS)**.
- **`Secure`:** Guarantees that the cookie is transmitted only over encrypted **HTTPS** connections, protecting it from eavesdropping and Man-in-the-Middle (MitM) attacks.
- **`SameSite`:** Governs whether cookies are dispatched with cross-site requests to mitigate **Cross-Site Request Forgery (CSRF)**:
  - `SameSite=Strict`: The cookie is never sent with cross-site requests (e.g. following a link from an external email).
  - `SameSite=Lax` *(Modern Browser Default)*: The cookie is sent only on top-level safe user navigations (e.g. clicking an external link), but omitted on cross-site subrequests (embedded `<img>`, `<iframe>`, or `POST` requests).
  - `SameSite=None`: The cookie is sent on all cross-site requests (requires the `Secure` flag to be set).

#### 3. Offloading Computation: Web Workers vs Service Workers
- **Single-Threaded Bottleneck:** JavaScript execution in the browser runs on a single main thread alongside rendering and DOM manipulation. Running heavy CPU computations (sorting millions of records, image filtering, cryptography) blocks the main thread, resulting in frozen UI and dropped frames.
- **Web Workers:** Run scripts in background OS-level threads completely isolated from the main thread. They do **not** have direct access to the DOM or the `window` object. They communicate with the main thread asynchronously via `postMessage()` and the `message` event.
- **Service Workers:** Act as a programmable network proxy between the web application, browser, and the network. They operate in the background to handle Progressive Web App (PWA) capabilities: offline caching of assets (`Cache API`), background sync, and push notifications.

### Code:
```javascript
// --- 1. localStorage and sessionStorage ---
localStorage.setItem("theme", "dark");
console.log("localStorage theme:", localStorage.getItem("theme"));

sessionStorage.setItem("currentWizardStep", "2");
console.log("sessionStorage step:", sessionStorage.getItem("currentWizardStep"));


// --- 2. IndexedDB Basics (Asynchronous Browser Database) ---
const request = indexedDB.open("AppDatabase", 1);

request.onupgradeneeded = (e) => {
    const db = e.target.result;
    if (!db.objectStoreNames.contains("users")) {
        db.createObjectStore("users", { keyPath: "id" });
    }
};

request.onsuccess = (e) => {
    const db = e.target.result;
    const tx = db.transaction("users", "readwrite");
    const store = tx.objectStore("users");
    store.put({ id: 101, name: "Mrityunjay", role: "Developer" });
};


// --- 3. Web Worker: Offloading Heavy CPU Computation ---
// In a real project, this would be a separate file: const worker = new Worker('worker.js');
const workerCode = `
    self.onmessage = function(e) {
        console.log("Worker thread received limit:", e.data);
        // Heavy computation offloaded from the main thread
        let sum = 0;
        for (let i = 0; i < e.data; i++) {
            sum += i;
        }
        self.postMessage(sum); // Send computed result back to main thread
    };
`;

const blob = new Blob([workerCode], { type: "application/javascript" });
const worker = new Worker(URL.createObjectURL(blob));

// Main thread listens for result without UI freezing
worker.onmessage = function(e) {
    console.log("Heavy calculation result from Worker:", e.data);
};

// Start background task
worker.postMessage(10000000);
```

---

## 5.2 Observer APIs: Intersection, Resize & Mutation
*Source file: `browser-apis-performance/observer-apis.html`*

### Notes:

#### Why use Observer APIs over Event Listeners?
Traditional implementations relied on binding listeners to window `scroll`, `resize`, or DOM polling. These events fire dozens of times per second on the main thread, causing **forced synchronous layouts** and **layout thrashing**.
Observer APIs run asynchronously outside the critical rendering path, batched by the browser engine for maximum performance.

1. **`IntersectionObserver` -**
   - Monitors when a target element enters, exits, or intersects with the browser viewport (or an ancestor element).
   - **Key Configurations:** `threshold` (0.0 to 1.0 indicating percentage visibility) and `rootMargin` (pre-fetching buffer).
   - **Top Use Cases:** Image lazy loading, Infinite scrolling (sentinel element at the bottom of a list), ad viewability tracking.

2. **`ResizeObserver` -**
   - Reports changes to the dimensions (content-box, border-box) of specific individual DOM elements.
   - **Top Use Cases:** Container-query-like component responsive behavior, dynamic charting canvas re-rendering when parent container resizes.

3. **`MutationObserver` -**
   - Watches for direct mutations to the DOM tree (adding/removing child nodes, updating element attributes, or modifying character data).
   - **Top Use Cases:** Third-party widget integrations, Microfrontend state syncing, accessibility auditors, undo/redo DOM history.

### Code:
```javascript
// =============================================================
// 1. IntersectionObserver: Infinite Scroll / Lazy Loading
// =============================================================
const sentinel = document.getElementById("sentinel");

const intersectionObserver = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            console.log("Sentinel is in viewport! Triggering next page fetch...");
            // loadNextPage();
        }
    });
}, {
    root: null,         // Default: browser viewport
    rootMargin: "100px",// Pre-fetch 100px before user reaches the bottom
    threshold: 0.1      // Fires as soon as 10% of sentinel is visible
});

if (sentinel) {
    intersectionObserver.observe(sentinel);
}


// =============================================================
// 2. ResizeObserver: Element Dimension Tracking
// =============================================================
const chartContainer = document.getElementById("chart-container");

const resizeObserver = new ResizeObserver((entries) => {
    for (let entry of entries) {
        const { width, height } = entry.contentRect;
        console.log(`Container resized -> Width: ${width}px, Height: ${height}px`);
        // Redraw responsive canvas/SVG chart
    }
});

if (chartContainer) {
    resizeObserver.observe(chartContainer);
}


// =============================================================
// 3. MutationObserver: Monitoring DOM Tree Changes
// =============================================================
const targetNode = document.getElementById("app-root");

const mutationObserver = new MutationObserver((mutationsList, observer) => {
    for (let mutation of mutationsList) {
        if (mutation.type === "childList") {
            console.log("Child nodes added or removed:", mutation.addedNodes);
        } else if (mutation.type === "attributes") {
            console.log(`Attribute "${mutation.attributeName}" was modified!`);
        }
    }
});

if (targetNode) {
    mutationObserver.observe(targetNode, {
        attributes: true,  // Monitor attribute changes
        childList: true,   // Monitor direct children addition/removal
        subtree: true      // Monitor all descendant nodes
    });
}
```

