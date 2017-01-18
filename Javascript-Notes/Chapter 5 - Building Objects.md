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
* The Person\(\) is called a function constructor, because it constructs an object.
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

![](/assets/Function Constructor Prototype Property Diagram.png)

* Regarding 'prototype': It is terribly named because this is not the prototype of the function but is a property used when the function is a constructor to set the prototype of an object created using this function constructor.
* Therefore, if we use the last topic's code and write:

```js
Person.prototype.fullName = function() {
    return this.firstname + ' ' + this.lastname;
}
```



