# This in Global Space 
This in Global space returns Global object which can be different depending on where you are running the code. 
For example, in a browser it will return Window object, inside Node JS it will return Global.
<pre> ``` javascript console.log(this) ``` </pre>

## This keyword works differently in strict and non strict mode
- In non strict mode :
  <pre> ```function test(){
   console.log(this)  // output: Window object
  }```</pre>
- In Strict mode :
  <pre> ```function test(){
   console.log(this)  // output: undefined
  }```</pre>

## Explicit Binding: call(), apply(), and bind()
In JavaScript, `call`, `apply`, and `bind` are methods on `Function.prototype` used to explicitly control and bind the `this` context to a specified object.

### 1. call()
Invokes the function immediately, binding `this` to the provided object. Arguments are passed individually (comma-separated).
```javascript
const person1 = { fname: "Mrityunjay", lname: "Mishra" };

function printDetails(hometown, state) {
    console.log(`${this.fname} ${this.lname} from ${hometown}, ${state}`);
}

printDetails.call(person1, "Varanasi", "UP");
// Output: Mrityunjay Mishra from Varanasi, UP
```

### 2. apply()
Invokes the function immediately like `call()`, but arguments are passed as an Array.
```javascript
printDetails.apply(person1, ["Varanasi", "UP"]);
// Output: Mrityunjay Mishra from Varanasi, UP
```

### 3. bind()
Does NOT invoke the function immediately. It returns a new copy of the function with `this` permanently bound, which can be called later.
```javascript
const printLater = printDetails.bind(person1, "Varanasi", "UP");
printLater();
// Output: Mrityunjay Mishra from Varanasi, UP
```

### Comparison Summary:
| Method | Immediate Invocation? | Argument Passing | Return Value |
| :--- | :--- | :--- | :--- |
| **`call()`** | Yes | Comma-separated (`arg1, arg2`) | Result of function |
| **`apply()`** | Yes | Array (`[arg1, arg2]`) | Result of function |
| **`bind()`** | No | Comma-separated | New bound function |
