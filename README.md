# Advanced Javascript
Comments and notes from Advanced Javascript course.

## Basics

### "Use Strict"
It is a rule that turns Javascript into _strict_ mode, a feature introduced in ECMAScript 5.

It is a string, so it doesn't invoke an error in older browsers running other versions of JS.

**Usage**
Add the `"use strict";` string at the beginning of the script or inside a function.

```javascript
"use strict";
// global strict mode
function newFunction() {
  "use strict";
  // local strict mode
}
```

**Motivation**
Strict mode helps to debug code, invoking erros which were silent before.
- Prevents variables to be called if they were not declared before.

This causes an error:
```javascript
"use strict";

newVar = 0; // this causes an error: "newVar is not defined"
```

This does not cause an error:
```javascript
"use strict";

var newVar = 1; // var declaration

newVar = 0; // var redefinition
```

- Prevents use of [reserved keywords](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)
- Prevents deletion of functins, variables and function arguments

```javascript
"use strict";

var foo = 1;
delete foo; // error: Delete of an unqualified identifier in strict mode.
```

- Makes use of `eval` safer, by not leaking its instructions out of it and prevents of it being used as a variable name


### Reference vs. Value
In general, if a variable has a primitive type, its *value* is used. If the varible is an object, its *reference* is used.

That means that, when a primitive type is called, a copy of it is passed down or used, not the variable itself.

```javascript
"use strict";

var a = true;

function foo(a) {
  a = false;
}

foo(a); // it actually is foo(true), a's value, or its copy
console.log(a) // returns true
```

Whereas, if the variable is an object, a reference of it is passed down.

However, one can not change/reassign the object itself, or where the object, points to, just its **properties**.

```javascript
"use strict";

var b = {};

function foo(b) {
  b = {'moo': 'too'}  // this does not work
  b.moo = false;
}

foo(b); // the function argument is a reference to b
console.log(b) // returns {moo: false}
```


### Rest operators
Introduced in ECMAScript 6. It passes a variable number of arguments in a function as an array.

```javascript
function foo(word, ...options) {
  console.log(word); // "one", as it is the first argument
  console.log(options); // [1, 2, 3], as they are the rest of the arguments
}

foo("one", 1, 2, 3);
```

### Spread operator
The notation of `...` is the same as the **rest operator**, but its use changes in different contexts. 

Used in arrays, the user gets a copy of its items into another variable.

```javascript
var arr1 = [1, 2, 3];
var arr2 = [...arr1, 4, 5];

console.log(arr2) // [1, 2, 3, 4, 5]

var arr3 = [4, 5, ...arr1, 6];

console.log(arr3) // [4, 5, 1, 2, 3, 6]
```

It is also important to note that it copies the array to another, not referecing it.

```javascript
var arr1 = [1, 2, 3];
var arr2 = arr1; // this is a refence to arr1
var arr3 = [...arr1]; // this is a copy of arr1

arr1[0] = -250;

console.log(arr1); // [-250, 2, 3] 
console.log(arr2); // [-250, 2, 3]
console.log(arr2); // [1, 2, 3]
```

Another use of spread operator is passing an array in a function as arguments.

```javascript
var method = "one";
var arr1 = [1, 2, 3];

function newFunction(word, ...options) {
  console.log(word);
  console.log(options);
}

newFuntion(method, arr1); // word = "one" and "options" = [[1, 2, 3]]
newFuntion(method, ...arr1); // word = "one" and "options" = [1, 2, 3]
```

### Template Strings or template literals
Strings that allow embedded expressions and multil-line strings. The notation to create a template string is to englobe the string between `\``.

```javascript
var name = "Mellina";
var isBold = true;

var msg = `
  Hello world!
  My name is ${isBold ? `<strong>${name}</strong>` : name}.
`;
```

Another form is template tag. It is a function that can perform operations on the arguments that are passed down.

```javascript
function h1(strings, nameExp, ageExp) {
  var hello = strings[0];
  var youare = strings[1];

  return `${hello}${nameExp}${youare}${ageExp > 20 ? 'an adult' : 'a teenager'}`;
}

var name = "Mellina";
var age = 28;
console.log(h1`Hello ${name}, you are ${age}`) // "Hello Mellina, you are an adult"
```

----------------

## Types and Equality
The types in Javascript:
- Boolean: true or false
- Number: 1, 1.0
- String: "a", 'a'
- Null
- Undefined
- Object

To know what type is a variable, one can use `typeof`

```javascript
console.log(typeof 1) // "number"
```

In Javascript, variables types are a dynamic, that is, its type can change along the way after being first declared.

### Undefined, null and NaN
**Undefined** is the type the Javascript engine uses for unitialized variables, missing paramethers in functions, unknown variables and properties in objects.

**Null** is a developer/user defined value, usually to indicate a "no value". For JS, `null` is an object.

**NaN** is the short for "not a number". It has the type of "number" and compared to itself `NaN == NaN` (and to anything) returns false. `isNaN(NaN)` doesn't behave as expected every time (it returns true to string values).

To check if the variable is `NaN`, there is a trick:
```javascript
var a = NaN;

console.log(a !== a) // only if this is NaN will it return true
```

### Equality
`==` is equality and `===` is strict equality.

In Equality, JS [coerce](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/) the values to check if they matches. Its output is very unpredictable.

Strict equality checks the variables as they are presented, its type and value.

-------------

## Scopes and Variables
Global variable it is declared outside functions and is accessible throughout the script. It is added to the object `window`.

In the example below, both the variable `one` and the property `moo` are global.
```javascript
var one = 'one';

window.moo = 'two';
```

Local or function variable only exists within the function it was declared.

```javascript
function moo() {
  var foo = 1;
  console.log(foo) // prints 1
}

console.log(foo) // error
```

Variables initiated with `var` in for loops are not local though. They are declared in the global scope.

### Let and const
To declare new variables, the word `var` can be used, but two new keywords were introduced: `let` and `const`.

These are called block scope variables, so they exists only within the block they were created. That improves performance and prevents data leakage.

`let` lets the variable to be reassigned and `const` does not.

In for loops, if the variable is declared with `let`, its value exists only within that block.

```javascript
for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i) // prints 5

for (let i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i) // error, because i doesn't exist outside the for loop block
```

### Hoisting
When using `"use strict"`, the variable and function needs to be declared before using. Variable or function hoisting is what JS does in the compile phase, moving their declarations to the top of the file.

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

### Scope Chain
Javascript will try to find the variable in the innermost scope first, and then goes outwards until it finds it. That is possible because of lexical scope, meaning that variables declared outside a function can be read in a another function defined after that, but the opposide is not true.

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
Is short for Immediately Invoked Function Expression. It is an expression for an annonymous function that is automatically executed.

It can be used to prevent variable scope leakage into other scripts.

```javascript
(function() {
  var thing = { hello: 'world' }; // this variable exists only within this script and block
})()
```

Closure is a function that exists within an lexical environment (its surrounding state), and can access its references. In simpler terms, it is a function inside another function.


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

To solve this, we can use IIFE to keep the variables within that scope and passing a paramether to it, to make sure its value changes every time.

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

-------------

## Destructuring and looping
Destructuring is the extraction of values into variables from object and arrays.

```javascript
const obj = {first: "Mell", last: "Yon", age: 20};
const { first: firstName, age } = obj; // one can use the name of the property itself (age) for variable creation or use a custom one (firstName)

const arr = ['a', 'b'];
const [ x, y ] = arr;
```

Desctructuring can also be used in function parameters.

```javascript
const obj = {first: "Mell", last: "Yon", age: 20};

function func({ first, age = 18, role = 'developer' }) { // default values can be assigned here too
  console.log(first, age, role); // prints Mell 20 developer
}

func(obj)
```

### For Loops
There four ways to do a for loop. The first is the classic one:

```javascript
var arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
  console.log(i);
}
```

It is important to notice that `break`, `continue` and `return` works within the classic for loop. That does is not true for the `forEach()` method.

```javascript
// this has the same effect as the previous for loop
arr.forEach((value) => {
  console.log(value);
})
```

Other ways to for loop are `for in`, for objects and `for of` for arrays. In those cases, `return` does not work.

```javascript
var obj = {prop1: 'one', prop2: 'two'};

for (let prop in obj) {
  console.log(prop) // prints prop1 and prop2
}

var arr = [1, 2, 3];

for (let value of arr) {
  console.log(value) // prints 1, then 2, then 3
}
```

---------------

## This
This is a keyword which refers to the actual scope. Outside any function or block, in the DOM, `this` refers to `window`. In strict mode, `this` default value is undefined.

Depending on the calling **context**, `this` value changes.

```javascript
this.name = "mellina" // or window.name = "mellina"

var obj = {
  checkThis: function() {
    // var self = this // this variable helps to make this more predictable
    console.log(this);

    function checkThisNow() {
      console.log(this); // when called without context, this refers to the global object
    }

    checkThisNow();
  }
}

obj.checkThis(); // prints Object
```

### Call, bind, apply
Those are methods that can be used in initialized functions. `call()` and `apply()` execute the function, and specify its context and parameters. `call()` is used for functions with fixed number of parameters and `apply()`, for functions with dynamic number of parameters.

```javascript
const firstExample = function (b, c) {
  console.log(this); // Numbber { 1 }
  console.log(b); // 2
  console.log(c); // 3
}

a.call(1, 2, 3); // the first arg (1) is the this context and the others are parameters;

const secondExample = function () {
  var total = 0;
  for (var i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

var numbers = [ 1, 2, 3, 56, 8, 3 ];
secondExample(null, numbers); // pass null because the context does not matter
```

`bind()` is used in a function initialization to bind a context to it, so it will be consistent when called afterwards.

```javascript
const bindExample = function(){
  console.log(this);
}.bind(2);

bindExample(); // prints Number { 2 }

// the following does not work because it is a function declaration, whereas the example before it is a function expression, where the function object is created, then its method bind is called, and then assigned to the bindExample const.

// function bindExample(){
// }.bind();
```

## Arrow functions
It is another way to write functions. It also stabelizes the `this` context, by always using the context where it initializes. It is important to know this, because arrow function may not work as expected because of its context setting.

```javascript
const obj = {
  name: "mell",
  exFunc: () => {
    console.log(this); // prints undefined, because arrow function will use the context where the obj was initialized, that is, the window
    setTimeout(() => {
      console.log(`${this.name}`); // prints "mell". In the case of this arrow function, this refer to the obj, where this function was initialized.
    }, 1000);
  }
};

obj.exFunc();
```

--------------

## Object Orientation
### Prototype Chain
Javascript will look for properties in the object, then its prototype. If the prototype points to another object, it will try to find there, and so on.

```javascript
var animal = {
  kind: 'cat'
}

var mell = Object.create(animal, { food: { value: "banana" } }); // creating an object and linking it to animal object. The second parameter pass properties to mell

console.log(mell.kind); // mell has the property of kind. Prints animal

mell.kind = "human"; 

console.log(mell.kind); // prints human
console.log(animal.kind); // prints animal
```

### Classical and Prototypal inheritance
Classical refers to the way Object Orientation works in older languages such as Java and C++. It is defined a class, which is a blueprint, from which other objects can be created.

Prototypal is the way Javascript handles inheritance, building objects from existing ones. Therefore, in JS, there is the "Pseudo-Classical Pattern" (as there is no way to do a true classical one) and the "Prototypal Pattern".

#### Pseudo-Classical Pattern (Constructor Pattern)
There is no way to create a classical Class in Javascript, but we can mimic it.

```javascript
"use strict";

function Person(firstName, lastName){
  this.firstName = firstName;
  this.lastName = lastName;
  this.fullName = function() {
    return this.firstName + ' ' + this.lastName;
    // or return firstName + ' ' + lastName to make it unmutable after creating the instance
  }
}

// we can add new properties and methods to the Person
Person.prototype.introduction = function() {
  return 'Hello ' + this.fullName();
}

var mell = new Person("mell", "yon"); // a new instance mell is created pointed to the Person prototype
console.log(mell); // Person {firsName: "mell", lastName: "yon"}
console.log(mell.fullName());
console.log(mell.introduction());
```

New pseudo-classes can be created and be inherited from another one.

```javascript
function Professional(honorific, firstName, lastName) {
  Person.call(this, firstName, lastName); // get the properties of the Person, but does not create a inheritance link
  this.honorific = honorific;
}

Professional.prototype = Object.create(Person.prototype); // this creates the inheritance link

var developer = new Professional("dev", "mell", "yon");

console.log(developer.fullName()); // Javascript tries to find the method in the developer object. When it doesn't find, it looks into the pointed Person pseudo-class.
```

#### Prototype Pattern
A more natural Javascript way to do Object Orientation, using the tools it has natively. There is no need to create a constructor function that mimics the classical inheritance.

```javascript
var Person = {
  init: function(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    return this;
  }
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}

var mell = Object.create(Person);
mell.init("mell", "yon");
```

Another way to create an object and pass on its properties is via the `Object.create()` method.

```javascript
var Person = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}

var mell = Object.create(Person, {
  firstName: {
    value: 'mell'
  },
  lastName: {
    value: 'yon'
  }
});
```

Another method is creating a "factory" type function, which handles the creation and passing the properties of the new instance.

```javascript
var Person = {
  fullName: function() {
    return this.firstName + " " + this.lastName;
  }
}

function PersonFactory(firstName, lastName) {
  var person = Object.create(Person);
  person.firstName = firstName;
  person.lastName = lastName;
  return person;
}

var mell = PersonFactory("mell", "yon");
```

To create a new prototype with inheritance, one can choose one of the three methods before mentioned and create that link.

#### ES6 Class and extends
`class` is a feature implemented in ES6 (or ECMAScript 2015). It does not create another kind of pattern, rather it defines another way to do prototype pattern.

Getters and setters methods are available, which creates a 'fake' property.

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get firstName() {
    return this.firstName;
  }

  set firstName(name) {
    if(name === "") {
      console.error("name cannot be blank");
    } else {
      this.firstName = name;
    }
    return;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`
  }

  whoAreYou() {
    return `I'm ${this.fullName}`
  }
}

const mell = new Person("mell", "yon"); // create a new instance
console.log(mell.fullName());
console.log(mell.firstName); // as getter, there is no need to ()
mell.firstName = "mayu";
console.log(mell.firstName);
```

`extends` is a keyword which created a new object, with an inheritance.

```javascript
class Student extends Person {
  constructor(firstName, lastName, course){
    super(firstName, lastName); // call super() before declaring other properties
    this.course = course;
  }

  whoAreYou() {
    return `${super.whoAreYou()} and I'm studying ${this.course}`
  }
}
```

## Asynchronous Programming
Async operations are not executed in order. They are mostly used when perfoming an API request or downloading some media (videos, audio etc).

### Callback
Callback is a function that is passed as an argument to another one. Callbacks are not async.

```javascript
function doAsyncTask(cb) {
  setTimeout(() => {
    cb(null, "The correct data");
  }, 0); // force async
}

doAsyncTask((err, data) => { // first parameter is the error
  if(err) {
    throw err
  } else {
    console.log("Data, ", data)
  }
})
```

When a function is async, it can be hard to know in what order it will execute. For that reason, a **callback hell** can happen. That is, functions being nested one after another, defining the calls' order.

### Promises
Promises are async by default. It handles the problem of the callback hell, by representing a completion or failure of an async operation. `.then()` can be called to perform an operation after the promise is resolved.

```javascript
function doAsyncTask() {
  const promise = new Promise((resolve, reject) => {
    console.log("Async Work Complete");
    if(false) {
      resolve({ x:1 });
    } else {
      reject( "Error" );
    }
  });
  return promise;
}

doAsyncTask().then(
  (val) => console.log(val),
  (err) => console.log(err)
);
```

Promises also can be chained. The returned value will be sent as a parameter to the second promise in the chain.

When an error occurs, it will pass it over the chain until it finds an error handler, so there is no need to add error handlers in all steps of it. `throw` also works, but more usually it is used the `.catch()` method.

An optional method `.finally()` can be called at the end of the chain, and the operation inside will execute either it is resolved or rejected.

A promise fork happens when promises are called separately.

```javascript
let promise = Promise.resolve("done");

// promise chain
promise.then(val => {
  console.log(val);
  return "done2"
}).then(val => console.log(val))
  .catch(err => console.log(err)); // error handler
  .finally(_ => console.log("cleaning up"))

// promise fork
promise.then(val => {
  console.log(val);
  return "done2"
})

promise.then(val => {
  console.log(val); // this keeps the first value, so it prints "done"
})
```

#### Promise.all
When multiple promises are called and there is the need to handle its resolution, `Promise.all` is used.

```javascript
const doAsyncTask = delay => {
  return new Promise(res => setTimeout(() => res(delay), delay))
}

let promises = [doAsyncTask(1000), doAsyncTask(2000), doAsyncTask(500)]

Promise.all(promises).then((values) => console.log(values))
// after 2 seconds, a single log is printed with the three values
```

### Async and await
When calling an async function, to prevent the script from running before its resolution, the `await` keyword is used. It only works on `async` functions.

It is important to know that this causes a _blocking_ operation, that is, nothing runs until that function is resolved or rejected.

```javascript
const doAsyncTask = () => Promise.resolve("done");

(async function() {
  let value = await doAsyncTask(); // await causes this to be blocking
  console.log(value); // will be done, rather than undefined
})();
```

`async`/`await` can also be used in conjunction with `.then()`.

```javascript
let asyncFunc = async function() {
  let value = await doAsyncTask();
  console.log(value);
  console.log(2);
  return 3;
}

asyncFunc().then(v => console.log(v)) // prints 3
```

Error handling with promises and `await` is intuitive, as when the promise is rejected, it throws an error which can be caught.

```javascript
const doAsyncTask = () => Project.reject('error'); // throws an error

let asyncFunc = async function() {
  try {
    const value = await doAsyncTask();
  } catch (e) {
    console.log("moo: ", e);
    return;
  }
}

asyncFunc()
```

---------------


## Networking

### CORS
By default, browsers reject resources comming from origins that are not the current one. For example, if the host website is `foo.com` and it requests a script from `moo.com`, this request is rejected, as they are not from the same origin.

**CORS**, or Cross-Origin Resource Sharing, is a mechanism that allow requests from different origins.

#### Requests
When performing a request, the browser sends the origin to the target resource. It then, returns a header property of `Access-Control-Allow-Origin`. Its value has to match the origin or use the "all" value, `*`. If the value is different than the origin, then the request response is rejected.

For GET requests, only one back-and-forth response is needed. But for others that might change the server, an additional step is added upfront, which is called the _preflight request_.

A preflight send a OPTIONS request, with the origin and the `Access-Control-Request-Method`, whose value can be PUT, DELETE, POST etc. The response's values have to match the origin's ones.

If the response is successful, then the request with the chosen method will be performed.

An important note here is that if the request has failed because of a CORS issue, then the **server** has to change its response headers.

### JSONP
JSONP is a solution for GET request that does not need to handle with cross origin resources. It is a `.js` file that calls a function and returns an array of objects (similiar to an json output).

The developer has to create a function with the same name as the one called in the jsonp file.

in `index.html`
```html
<script>
function callbackFunction(json) {
  console.log(json);
}
</script>

<script src="jsonp.js"></script>
```

in `jsonp.js`:
```javascript
callbackFunction([
  {
    "id": 1,
    "firstName": "Mell",
    "lastName": "Yon"
  }
])
```

In the network Devtools tab, this does not appear under XHR, because is not a GET request.


---------------


## Events
In the DOM, elements are created starting to the `window` object, as in a tree structure. When an event is fired in an element, two phases occur: one triggering from the bottom to the top, and the second from the top to the bottom.

For example, if the structure is `window > document > body > button-one > button-two`, then the first phase, the "Event Capturing Phase", triggers the window, then document, body and so on. The second phase, the "Event Bubbling Phase", triggers from the button-one, to button-two, body, document and window.

That means that and event triggers **twice**. However, when using `.addEventListener(event, callback, eventPhase)`, by default, only the event in "Event Bubbling Phase" is trigger. If `eventPhase` is set to true, then the event in "Event Capturing Phase" is triggered.

### stopPropagation() and preventDefault()
`stopPropagation()` stops the propagation of events in the element of the tree, through the phases seen before.

`preventDefault()` does not perform the default action of an element. That means, for example, that a checkbox will not be checked when clicked, if this method is used. This does not stop the propagation of events.


---------------


## Reference
- [Closure](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36);
- [Scope](https://spin.atomicobject.com/2014/10/20/javascript-scope-closures/);
- [Callback Hell](http://callbackhell.com/);