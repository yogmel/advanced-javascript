## Basics

[**Go back to Summary**](README.md#summary);

- [Use Strict](#use-strict);
- [Reference vs. Value](#reference-vs-value);
- [Rest operators (parameters)](#rest-operators-parameters);
- [Spread operator (syntax)](#spread-operator-syntax);
- [Template literals (template strings)](#template-literals-template-strings).

### "Use Strict"
[Strict Mode - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

It is a rule that turns Javascript into _strict_ mode, a feature introduced in ECMAScript 5. Strict mode is an opt-in mode, contrary to "sloppy mode" (unofficial term to non-strict mode). Both modes can coexist.

It is a _string_, so it doesn't invoke an error in older browsers running other versions of JS.

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
Strict mode helps to debug code, invoking errors which were silent before.

- Prevents variables to be called if they were not declared before.

This causes an error:
```javascript
"use strict";

newVar = 0; // this causes an error: "newVar is not defined"
```

This does not cause an error:
```javascript
"use strict";

var newVar = 1; // var declaration and initialization

newVar = 0; // var assignment
```

- Prevents the use of [reserved keywords](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords)

- Prevents deletion of functions, variables and function arguments

```javascript
"use strict";

var foo = 1;
delete foo; // error: Delete of an unqualified identifier in strict mode.
```

- Makes use of `eval` safer, by not leaking its instructions out of it and prevents of it being used as a variable name


### Reference vs. Value
In general, if a variable has a **primitive** type, its *value* is passed. If the variable is an **object** or an **array**, its *reference* is passed.

That means that, when a primitive type is called, a _copy_ of it is passed down or used, not the type itself. If the original variable is changed, that has no effect on the passed value and vice-versa.

```javascript
"use strict";

var a = true;

function foo(a) {
  a = false;
}

foo(a); // it actually is foo(true), a's value, or its copy
console.log(a) // returns true
```

Whereas, if the variable is an object or an array, a reference of it is passed down.

However, if the object/array is reassigned, then it loses its reference. Only changing its **properties** will keep its reference.

```javascript
"use strict";

var b = {};

function foo(b) {
  b.moo = false;
}

foo(b); // the function argument is a reference to b
console.log(b) // returns {moo: false}
```


### Rest operators (parameters)
- [Rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

Introduced in ECMAScript 6. It passes an indefinite number of arguments in a function as an array.

The last or all of the parameters can be _rest parameters_, but not the first of them.

```javascript
function foo(word, ...options) {
  console.log(word); // "one", as it is the first argument
  console.log(options); // [1, 2, 3], as they are the rest of the arguments
}

foo("one", 1, 2, 3);
```

### Spread operator (syntax)
- [Spread syntax - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

The notation of `...` is the same as the **rest operator**, but its uses changes in contexts. 

Used in arrays, the user gets a copy of its items into another variable.

```javascript
var arr1 = [1, 2, 3];
var arr2 = [...arr1, 4, 5];

console.log(arr2) // [1, 2, 3, 4, 5]

var arr3 = [4, 5, ...arr1, 6];

console.log(arr3) // [4, 5, 1, 2, 3, 6]
```

It is also important to note that it copies the array to another, not referencing it.

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

### Template literals (template strings)
- [Template literals - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

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
