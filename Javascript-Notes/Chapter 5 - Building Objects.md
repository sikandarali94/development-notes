# Building Objects

### Function Constructors, 'new', and the History of Javascript

* There are more ways to build objects other than: `new Object();` and the object literal syntax \({}\).
* One way to build an object is utilizing a function \(i.e. object\). Here is an example:

```js
function Person() {
    this.firstname = 'Sikandar';
    this.lastname = 'Ali';
}

/* 
    This creates an object named siky that takes the properties
    of firstname and lastname embedded within Person(). The new
    keyword is actually an operator in JS.
*/
 var siky = new Person();
```

* When we write `new` keyword it immediately sets up an empty object like `var a = {}`. So when we write `var john = new Person();` what this actually does is that the `new` keyword sets up an empty object and the `this` assignment expression in `Person` point to this empty object and create the properties of `firstname` and `lastname`. As long as a function does not have a `return` statement the `new` keyword will return an object. The assignment expression of `var john = new Person();` also invokes the `Person()` function.
* The `Person()` is called a function constructor, because it constructs an object.
* We can even set parameters for a function constructor. Here is an example:

```js
function Person(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}
```

* **Function Constructor**: A normal function that is used to construct objects. The this variable points to a new empty object, and that object is returned from the function automatically.

### Function Constructors and '.prototype'

* When we use a function constructor Javascript will automatically set the prototype for us.
* When we type in the console `john.__proto__` \(John is an object created using a function constructor in the last example with the `Person()` constructor\), this will give us an empty called `Person` like this: `Person {}`.
* The prototype of a function is not the function's prototype but it is the prototype of an object that is created using that function as a constructor. Every function has a prototype property that starts off its life as an empty object. Unless we are using the function as a constructor the prototype \(empty obj\) just hangs out and is never used. As soon as we use the new operator to invoke a function then it means something. It sits there and lives only for when we are using a function to construct an object as a function constructor. Here is a visual:

![](/assets/FunctionConstructorPrototypePropertyDiagram.png)

* Regarding 'prototype': It is terribly named because this is not the prototype of the function but is a property used when the function is a constructor to set the prototype of an object created using this function constructor.
* Therefore, if we use the last topic's code and write:

```js
Person.prototype.fullName = function() {
    return this.firstname + ' ' + this.lastname;
}
```

* When we construct an object the object's prototype would point to Person.prototype. This is terribly confusing only because the name is given as 'prototype' when it isn't actually the prototype of the function at all, but the prototype of the objects created using the function as a constructor.

* Therefore if we updated the prototype object even after an object is created it will update the prototype of the objects, because they point to the prototype property of the function constructor that constructed them. Here is an example:

```js
function Person() {
    this.firstname = 'Sikandar';
    this.lastname = 'Ali';
}

var siky = new Person();

/*
    siky will get the getFullName function in its prototype because
    it points to Person.prototype for its own prototype.
*/
Person.prototype.getFullName = function() {
    return this.firstname + ' ' + this.lastname;
}
```

* If we were to put the `getFullName()` into the function constructor it will copy the method into the objects created using the function constructor. If we had 1000 objects constructed from the function it will copy the method into each one of them taking up lots of memory. However, if we put the function method in the prototype property all objects will point to the method rather than copy the method. Therefore we will only have one copy sitting in the prototype with objects pointing to it, thus saving a lot of memory.

### 'new' and Functions \(Dangerous\)

* In `var john = new Person();`, if we forget to type the `new` keyword and `Person()` does not have anything returned then `john` will receive `undefined`. In order to debug this issue when it arises it is a convention to name function constructors starting with a capital letter. From there we can know to put a `new` keyword when debugging an error where `john` is `undefined`. The future of Javascript will have more ways to construct objects so most likely function constructors are going away \(not completely however\), and being replaced with a better method.

### Built-In Function Constructors \(Conceptual\)

* If we use a built-in function as a constructor, for example: `var a = new Number(3)`, then `a` will be an object because anything returned from a function constructor is an object. Prototype of `a` will point to the prototype property of `Number()`. This is similar to all other built-in functions of Javascript like `String()`, `Boolean()` and so forth.
* Sometimes Javascript will automatically put primitives in object. For example: `"Sikandar".length;`, this will return `8` because Javascript understands that we want to know the length of the string `"Sikandar"` and will box it into an object string that has the prototype of `length`.
* We can update the in built-in method, so in Javascript by updating the prototype. Here is an example:

```js
//We created a new method of the built-in function of String().
String.prototype.isLengthGreaterThan = function(limit) {
    return this.length > limit;
}

console.log("Sikandar".isLengthGreaterThan(3));
```

* While Javascript is nice to convert a string into an object it does not convert a number into an object. Therefore this:

```js
//Provided we created this method within the Number function's prototype property...
3.isPositive();
```

* Therefore the code above will give us an error.
* Therefore we should do something like this:

```js
var a = new Number(3);
a.isPositive();
```

* In general we shouldn't use built-in function constructors for these primitive types, unless we desperately have to.

### Built-In Function Constructors \(Dangerous\)

* If we write something like this:

```js
var a = 3;
var b = new Number(3);
//Returns false, whereas == will have returned true.
a === b;
```

* The strict equal comparison operator will return `false`because `a` stores a primitive value while `b` stores an object created using the built-in Number function as a constructor. If we compared it with a normal equal it will return `true`. This is dangerous because by using built-in function constructors for creating primitives we aren't really creating primtives and strange things can happen when comparison operators are used.
* That is why in general we shouldn't use built-in functions as constructors and instead use literals to store primitive values.
* If we use a lot of date work it is recommended to use the library framework named **Moment.js**.
* These built-in function constructors should be meant for conversion. Here is an example:

```js
var c = Number("3");
```

### Arrays and for..in \(Dangerous\)

* Arrays are objects in Javascript. The index values are actually property names and makes them different to arrays in other languages. So this: `a = ['Sikandar', 'Ali'];` is actually:

```js
a {
    0:'Sikandar'
    1:'Ali'
}
```

* In this example:

```js
Array.prototype.myCustomFeature = 'cool';
var arr = ['John', 'June', 'Jim'];
for (var prop in arr) {
    /*
        This will log: 0: John
                       1: Jane
                       2: Jim
                       myCustomFeature: cool

        That is why we shouldn't use for..in to loop through an array because it loops
        through the properties within the prototype object.
    */
    console.log(prop + ': ' + arr[prop]);
}
```

### Object.create and Pure Prototypal Inheritance

* Object.create creates an empty object with its prototype pointing to whatever object we passed. Here is an example:

```js
var person = {
    firstname: 'Sikandar'
}
/*
    The sikandar object will be empty but its prototype will point to person object, so:
    sikandar.firstname will say 'Sikandar'.
*/
var sikandar = Object.create(person);
```

* This is known as Pure Prototypal Inheritance. If there was a method in `person` object and there was `this.firstname` and we put in a property of `firstname` in `sikandar` object like this: `sikandar.firstname = 'siky';` then `this.firstname` will point to `'siky'` rather than `'Sikandar'`. Even then the prototype is pointing to person because if we mutated a method in person it would affect the prototype of every object created using person.
* However `Object.create` is available only in newer browsers. To support older browsers we can use something called a polyfill.
* **Polyfill**: Code that adds a feature which the engine may lack.
* There is an example code of a polyfill within the course that should be in the notes.
* The most powerful aspect of Pure Prototypal Inheritance is that we can mutate the object \(that is being pointed to by other objects that were created by it\) on the fly.

### ES6 and Classes

* ES6 is called ECMAScript 6 or ECMAScript 2015.
* Classes are used to define the structure of an object. ES6 will have classes but 'implemented in a different way.'
* In other programming languages class isn't an object, it is a definition that tells what an object should look like and is only an object when the `new` keyword is used. In Javascript a class is an **object**. The **extends** keyword sets the prototype of the object.
* **Syntactic Sugar**: A different way to type something that doesn't change how it works under the hood.
* The new class syntax does not change how objects are created and treated in Javascript. It still uses Pure Prototypal Inheritance.

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

