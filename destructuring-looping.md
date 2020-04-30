## Destructuring and looping

[**Go back to Summary**](README.md#summary);

- [Destructuring](#destructuring);
- [For Loops](#for-loops);

----------

## Destructuring

- [Destructuring assignment - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

Destructuring is the extraction of values into variables from objects and arrays.

```javascript
const obj = { first: "Mell", last: "Yon", age: 20 };
const { first: firstName, age } = obj; // one can use the name of the property itself (age) for variable creation or use a custom one (firstName)

const arr = ['a', 'b'];
const [ x, y ] = arr;
```

Destructuring can also be used in function parameters.

```javascript
const obj = {first: "Mell", last: "Yon", age: 20};

function func({ first, age = 18, role = 'developer' }) { // default values can be assigned here
  console.log(first, age, role); // prints Mell 20 developer
}

func(obj);
```

If there is a value in the middle that will not be used, it can be ignored:

```javascript
function f() {
  return [1, 2, 3];
}

const [ a, , b ] = f();
console.log(a); // prints 1
console.log(b); // prints 2
```

### For Loops
There four ways to do a for loop. The first is the classic one:

- [For - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)

```javascript
var arr = [1, 2, 3];

for (let i = 0; i < arr.length; i++) {
  console.log(i);
}
```

It is important to notice that `break`, `continue` and `return` works within the classic for loop. That does is not true for the `forEach()` method.

- [forEach() - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

```javascript
// this has the same effect as the previous for loop
arr.forEach((value) => {
  console.log(value);
})
```

Other ways to for loop are `for in`, for objects and `for of` for arrays (or other iterable objects, such as strings and NodeList). In those cases, `return` does not work.

- [for...in - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)
- [for...of - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

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
