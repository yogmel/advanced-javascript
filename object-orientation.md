## Object Orientation

[**Go back to Summary**](README.md#summary);

- [Prototype Chain](#prototype-chain);
- [Classical and Prototypal inheritance](#classical-and-prototypal-inheritance);
  - [Pseudo-Classical Pattern (Constructor Pattern)](#pseudo-classical-pattern-constructor-pattern);
  - [Prototype Pattern](#prototype-pattern);
- [ES6 Class and extends](#es6-class-and-extends);

---------

### Prototype Chain

- [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

Each object has a private property, which holds a link to another object (its **prototype**). This prototype has a prototype of its own, and so on, until an object is reached with null as its prototype. 

When trying to access a property of an object, the property will not only be sought on the object but on the **prototype of the object**. If the prototype points to another one, it will try to find there, and so on.

```javascript
var animal = {
  kind: 'cat'
}

var mell = Object.create(animal, { food: { value: "banana" } }); // creating an object and linking it to animal object. The second parameter pass properties to mell

console.log(mell.kind); // mell has the property of kind. Prints animal

mell.kind = "human"; 

console.log(mell.kind); // prints human
console.log(animal.kind); // prints cat
```

### Classical and Prototypal inheritance
Classical refers to the way Object Orientation works in older languages such as Java and C++. It is defined a class, which is a blueprint, from which other objects can be created.

Prototypal is the way Javascript handles inheritance, building objects from existing ones.

Therefore, in JS, there is the "Pseudo-Classical Pattern" (as there is no way to do a true classical one) and the "Prototypal Pattern".

#### Pseudo-Classical Pattern (Constructor Pattern)
There is no way to create a classical Class in Javascript, but we can mimic it, with a constructor. A constructor, in JS, a function that is called with the `new` operator.

Calling a function with the `new` operator returns an object that is an instance of the function. 

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
A more natural Javascript way to do Object Orientation, using the tools it has natively. There is no need to create a constructor function.

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
// Person ---> Object.prototype ---> null

var mell = Object.create(Person);
// mell ---> Person ---> Object.prototype ---> null

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

Another method is creating a "factory" type function, which handles the creation and passing of the properties of the new instance.

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

To create a new prototype with inheritance, choose one of the three methods before mentioned and create that link.

#### ES6 Class and extends
`class` is a feature implemented in ES6 (or ECMAScript 2015). It does not create another kind of pattern, rather it defines another way to do the prototype pattern.

Getters and setters methods are available, which creates a 'fake' property.

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get getFirstName() {
    return this.firstName;
  }

  set setFirstName(name) {
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
console.log(mell.getFirstName); // as getter, there is no need to ()
mell.setFirstName = "mayu";
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
