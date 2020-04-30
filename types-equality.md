## Types and Equality
[**Go back to Summary**](README.MD#summary);

- [Undefined, null and NaN](#undefined-null-and-nan);
- [Equality](#equality).

----------

- [Data Structures - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)

The types in Javascript:
- Boolean: true or false
- Number: 1, 1.0
- String: "a", 'a'
- Null
- Undefined
- Object

To check a data type, use `typeof`.

Primitive values are all types but objects. Primitive types can not be changed.

```javascript
console.log(typeof 1) // "number"
```

Javascript is a loosely typed or a dynamic language, that is, its type can change along the way after being first declared.

### Undefined, null and NaN
**Undefined** is the type the Javascript engine uses for uninitialized variables, missing parameters in functions, unknown variables and properties in objects.

**Null** is a developer/user-defined value, usually to indicate a "no value". For JS, `null` is an object. It is also shown when at the end of a _Prototype Chain_.

**NaN** is short for "not a number". It has the type of "number" and compared to itself `NaN == NaN` (and to anything) returns false. `isNaN(NaN)` doesn't behave as expected every time (it returns true to string values).

To check if the variable is `NaN`, compare it to itself. It should return true:
```javascript
var a = NaN;

console.log(a !== a) // only if this is NaN will it return true
```

### Equality
`==` is abstract equality and `===` is strict equality comparison.

In Abstract Equality (or loose equality), JS [coerce](https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839/) the values to check if they match. Its output is very unpredictable.

- [Equality table](https://dorey.github.io/JavaScript-Equality-Table/)

Strict equality checks the data without the type conversion.

- [Equality comparisons and sameness](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness);
