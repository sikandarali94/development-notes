# Objects and Functions

### Objects and the Dot

* In Javascript objects and functions are very mush related and should be learned together and not separately.
* An object can have properties and methods. So an object can have a primitive \(property\), an object can have another object \(property\), and an object can have a function \(method\).

![](/assets/Object Diagram.png)

* Concerning 0x001, 0x002, 0x003, 0x004 an object will have an address in memory and it will have references to different properties and methods.
* In this example:

```js
var person = new Object;

person["first name"] = "Sikandar";
/*
    The square brackets is an operator. Below, it creates a space in memory that name,
    "last name", and the object will get an address of location of the name.
*/
person["first name"] = "Ali";

var firstNameProperty = "first name";
console.log(person);
console.log(person[firstNameProperty]);
```

* In the code `console.log(person.firstname);` The dot is known as the dot operator.The parameter it takes here is `person` and `firstname` since an operator is a function. We don't have to put string quotes around `firstname` because dot operator function already recognizes that `firstname` is meant to be a string that we typed.
* We can store an object within an object and set properties and methods inside it. For example:

```js
var person = new Object();
//This creates an object called address within the object person.
person.address = new Object();
person.address.street = "111 Main St.";
```

* It is recommended to use the dot operator when dealing with objects, unless use the `[]` operator when we are dealing with dynamic strings, meaning strings that can be changed and we use a variable to store that dynamic string.

### Objects and Object Literals

* The recommended way to create an object is using an object literal, like this: `var person = {};` The `{}` is not an operator! What is hapenning is that the Javascript engine when parsing and it comes across a curly brace, and its not part of an if statement or a for loop or something, it assumes we are creating an object.
* We can set properties or methods of an object by just using the object literal:

```js
var person = {firstname:'Sikandar', lastname: 'Ali'};
```

* We can pass an object as a parameter into a function on the fly. For example:

```js
var Siky = {
    firstname:'Sikandar',
    lastname: 'Ali',
    address: {
        street:'111 Main St.',
        city:'New York',
        state:'NY'
    };

function greet(person) {
    console.log('Hi ' + person.firstname);
}

//This will log to the console, 'Hi Sikandar'
greet(Siky);

//We can also create an object on the fly within function invocation
greet({firstname: 'Jane', lastname: 'Austen'});
```

### Faking Namespaces \(Framework\)

* **Namespace**: A container for variables and functions. Typically to keep variables and functions with the same name separate. The problem is Javascript doesn't have namespaces, but there is a way to fake it using objects.
* Here is an example on how to fake it:

```js
var greet = 'Hello!';
var greet = 'Hola!';

//This creates a clash.
console.log(greet);
```



