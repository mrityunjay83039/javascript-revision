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
