# JavaScript Revision & Interview Preparation Notes

This markdown file compiles all the revision notes, concepts, and code directly from your files across `js fundamental`, `asynjs`, `functional programming`, and `oops` without changing your original text and explanations.

---

## Table of Contents
- [1. JS Fundamental](#1-js-fundamental)
  - [1.1 Variables, Hoisting & TDZ (`variables-hoisting-tdz.html`)](#11-variables-hoisting--tdz)
  - [1.2 Data Types (`data-types.html`)](#12-data-types)
  - [1.3 Execution Context & Event Loop (`execution-eventloop.html`)](#13-execution-context--event-loop)
  - [1.4 Closure (`clouser.html`)](#14-closure)
  - [1.5 This Keyword (`this-keyword.md`)](#15-this-keyword)
  - [1.6 Event Handling: Bubbling & Capturing (`event-handling.html`)](#16-event-handling-bubbling--capturing)
  - [1.7 Debouncing & Throttling (`debouncing-throttling.html`)](#17-debouncing--throttling)
- [2. Asynchronous JavaScript (`asynjs`)](#2-asynchronous-javascript-asynjs)
  - [2.1 CallBack (`callback.html`)](#21-callback)
  - [2.2 Promise (`promise.html`)](#22-promise)
  - [2.3 Async Await (`async-await.html`)](#23-async-await)
- [3. Functional Programming](#3-functional-programming)
  - [3.1 Important Polyfills (`polyfills.html`)](#31-important-polyfills)
- [4. Object-Oriented Programming (`oops`)](#4-object-oriented-programming-oops)
  - [4.1 Function Constructor and Inheritance (`functionConstructor.html`)](#41-function-constructor-and-inheritance)
  - [4.2 Classes (`class.html`)](#42-classes)

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

## 1.5 This Keyword
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

## 1.6 Event Handling: Bubbling & Capturing
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

### Code Example:
```javascript
document.querySelector('#grand-parent').addEventListener('click',()=>{
    console.log('grand parent clicked')
},true)

document.querySelector('#parent').addEventListener('click',()=>{
    console.log('parent clicked')
})

document.querySelector('#child').addEventListener('click',()=>{
    console.log('Child clicked')
})
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

# 3. Functional Programming

## 3.1 Important Polyfills
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
