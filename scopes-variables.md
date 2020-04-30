## Scopes and Variables

[**Go back to Summary**](README.md#summary);

- [Let and const](#let-and-const);
- [Hoisting](#hoisting);
- [Scope Chain](#scope-chain);
- [IIFE and closures](#iife-and-closures).

---------------

- [Scopes and Closures - CSS Tricks](https://css-tricks.com/javascript-scope-closures/)

A global variable is declared outside functions and is accessible throughout the script. The `window` object is also global.

In the example below, both the variable `one` and the property `moo` are global.
```javascript
var one = 'one';

window.moo = 'two';
```

Local, block or function scope variables only exist within the scope they were declared.

```javascript
function moo() {
  var foo = 1;
  console.log(foo) // prints 1
}

console.log(foo) // error, the foo variable does not exist outside the function scope
```

Variables initiated with `var` in for loops are not local, though. They are declared in the global scope. That does not happen with `let` and `const`.

### Let and const
To declare new variables, the word `var` can be used, but two new keywords were introduced: `let` and `const`.

These are called block scope variables, so they exists only within the block they were created. That improves performance and prevents data leakage.

`let` lets the variable to be reassigned and `const` does not. Although objects properties declared in `const` can be changed.

In for loops, if the variable is declared with `let`, its value exists only within that block.

```javascript
for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i) // prints 5

for (let j = 0; j < 5; j++) {
  console.log(j);
}

console.log(j) // error, because i doesn't exist outside the for loop block
```

### Hoisting
When using `"use strict"`, the variable and function needs to be declared and initialized before using. Variable or function hoisting is what JS does in the compile phase, moving their declarations to the top of the file.

When we write this code:
```javascript
"use strict";

console.log(a); // undefined
var a = 1;
```

What JS do in the compile phase is:
```javascript
"use strict";
var a; // declaration

console.log(a); // undefined
a = 1; // initialization
```

Function declarations work, as the both declaration and initialization are hoisted. That is not true with functions expressions.

```javascript
sum(1, 3); // prints 4

function sum(a, b) {
  return a + b;
}

subtract(4, 2); // error

const subtract = function (a, b) {
  return a - b;
}
```

### Scope Chain
Javascript will try to find a variable in the innermost scope first and then goes outwards until it finds it. That is possible because of lexical scope, meaning that variables declared outside a function can be read in another function defined after that, but the opposite is not true.

- [Scope](https://spin.atomicobject.com/2014/10/20/javascript-scope-closures/);

```javascript
var myvar = 1;
function foo() {
  function goo() {
    console.log(myvar); // after looking into goo() and foo() scopes, it find the global scope and prints 1
  }
  foo();
}
goo();
```

### IIFE and closures
IIFE Is short for Immediately Invoked Function Expression. It is an expression for an anonymous function that is automatically executed.

It prevents variable scope leakage into other scopes and scripts (if multiple scripts are used).

```javascript
(function() {
  var thing = { hello: 'world' }; // this variable exists only within this script and block
})()
```

Closure is a function that exists within an lexical environment (its surrounding state), and can access its references. In simpler terms, it is a function inside another function.

- [Closure](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36);

```javascript
function sayHello(name) {
  var text = "Hello" + name;
  return function() {
    console.log(text);
  }
}

var sayMell = sayHello("Mell");
sayMell(); // prints "Hello Mell"
```

It is important to know that it is passed down the value of the variable, not its refecence. A tricky example happens in a for loop:

```javascript
var foo = [];
for (var i = 0; i < 10; i++) {
  foo[i] = function() { return i };
}

console.log(foo[0]()); // prints 10
console.log(foo[1]()); // prints 10
console.log(foo[2]()); // prints 10
```

The reason why it only prints 10 is that, by the time the closure is created, the for loop is already done. And `i` final value is 10.

To solve this, we can use IIFE to keep the variables within that scope and passing a parameter to it, to make sure its value changes every time.

```javascript
var foo = [];
for (var i = 0; i < 10; i++) {
  (function(y) {
    foo[y] = function() { return y };
  })(i); // this makes sure the value is changed every iteration
}

console.log(foo[0]()); // prints 0
console.log(foo[1]()); // prints 1
console.log(foo[2]()); // prints 2
```
