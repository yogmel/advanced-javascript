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
