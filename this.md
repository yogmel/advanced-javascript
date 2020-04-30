## This

[**Go back to Summary**](README.md#summary);

- [Call, bind, apply](#call-bind-apply);
- [Arrow functions](#arrow-functions);

---------

- [this - MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

This is a keyword that refers to the actual scope. Outside any function or block, in the DOM, `this` refers to `window`. In strict mode, `this` default value is undefined.

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
Those are methods that can be used in initialized functions. `call()` and `apply()` execute the function, and specify its context and parameters. `call()` is used for functions with a fixed number of parameters and `apply()`, for functions with a dynamic number of parameters.

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

`bind()` is used in a function initialization to bind a context to it, so it will be consistent when called afterward.

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
It is another way to write functions. It also stabilizes the `this` context, by always using the context where it initializes (the value of the enclosing lexical context). It is important to know this, because arrow function may not work as expected because of its context setting.

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

Also, the before-mentioned methods, `call()`, `apply()` and `bind()` do not change the context when used in an arrow function.
