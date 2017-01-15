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

//This logs 'Hola!' and overrides 'Hello!' because it clashed.
console.log(greet);

var english = {};
var spanish = {};

/*
    This avoids collision. Essentially the english and spanish objects becomes
    namespaces so the greet won't clash with one another.
*/
english.greet = 'Hello!';
spanish.greet = 'Hola!';
```

* When we write: `english.greetings.greet = 'Hello!;` -- when we don't already have greetings as an object in the `english` object -- it gives us the error: _Cannot set property 'greet' of undefined_. This is because the associativity of the dot operator is left to right. So, when it says `english.greetings` it evaluates it to `undefined`. Then what it is essentially saying is `undefined.greet`, which would obviously give an error since `undefined` is not an object. To solve this error we have to write: `english.greetings = {};` beforehand and then write `english.greetings.greet = 'Hello!;`

### JSON and Object Literals

* A common mistake is to think JSON \(Javascript Object Notation\) and object literals are the exact same thing. JSON is just inspired by Object Literal Notation.
* Data had to be sent over the internet in files. What they landed on for a while was the XML file. Here is an example of an XML file:

```
<object>
    <firstname>Mary</firstname>
    <isAProgrammer>true</isAProgrammer>
</object>
```

* However, the issue with this was that data would be unnecessarily big because of the letters in the tag. So they looked at the Object Literal Notation and realized that the syntax is a more efficient way to send data over the internet. For example look how smaller this is:

```
{
    "firstname": 'Mary'
    "isAProgrammer":true
}
```

* That is why JSON is the most popular way to send data over the internet. One difference is that properties have to be wrapped in quotes in JSON.
* So JSON is technically a subset of the object literal syntax. Meaning that is JSON valid is Object Literal Notation valid. However it is not necessarily true vice versa.
* Javascript has some built-in functionality to convert JSON into an object, and vice versa. Here is an example of both:

```js
var objectLiteral = {
    firstname: 'Mary'
    isAProgrammer: true
}

//Convert object literal to JSON syntax
console.log(JSON.stringify(objectLiteral));
//Convert JSON syntax to object literal.
JSON.parse('{"firstname": "Mary", "isAProgrammer":true}');
```

* JSON also doesn't allow functions as values.

### Functions Are Objects

* In Javascript functions are **objects**. These are called first-called functions.
* **First Class Functions**: Everything you can do with other types you can do with functions. That means you can assign them to variables, pass them around, or create them on the fly.
* A function is a special type of object, which means you can attach a primitive, an object, or even another function:

![](/assets/Function As Object Diagram.png)

* Regarding '"Invocable" \(\)', it is special for functions because a function can be called. Regarding 'CODE', it is a special property. Therefore the code that we write is just another property. Regarding 'NAME', it is a special property of a function.
* To prove functions are objects, here is an example:

```js
function greet() {
    console.log('hi');
}

/*
    In many other languages you just can't do this! However if we were to write:
    console.log(greet); it will just print out the function. If we were to however
    write: console.log(greet.language);, the console will log 'english'.
*/
greet.language = 'english';
```

* Here is a visual representation of what we did above:

![](/assets/Function Invocation Diagram.png)

### Function Statements and Function Expressions

* **Expression**: A unit of code that results in a value. It doesn't have to save a variable. For example: `a = 3;` and `1 + 2;` are both expressions because they both return `3`.
* An if statement is a statement and doesn't return a value even though we put a expression in between the round brackets. So a statement just does work and an expression returns a value.
* In Javascript because a function is an object we can have both function statements and function expressions.
* An example of a function statement is:

```js
function greet() {
    console.log('hi');
}
```

* An example of a function expression is:

```js
/*
    This all evaluates to an object being created and that is why it is an expression.
    Here it is putting this object into memory and pointing that anonymous variable at
    the address.
*/
var anonymousGreet = function() {
    console.log('hi');
}
```

![](/assets/Anonymous Function Diagram.png)

* Regarding 'CODE' to invoke this property of the anonymous function we write: anonymousGreet\(\); Regarding 'NAME' since we already have a variable pointing to the address in memory of where the object is located we don't need a name for the function.
* Because functions are objects we can pass them as parameters into other functions. Here is an example:

```js
function log(a) {
    /*
        This will then log to the console:
            function() {
                console.log('hi');
            }
    */
    console.log(a);
    /*
        We can even invoke the anonymous function because it is stored in the
        parameter variable as a.
    */
    a();
}

//Here we are invoking the log() function.
log(function () {
    console.log('hi');    
});
```

* First class functions allow us to do a new kind of programming called functional programming.

### By Value vs By Reference \(Conceptual\)

* When we talk about by value and by reference we are both talking about variables.

![](/assets/By Value Diagram.png)



![](/assets/By Reference Diagram.png)

* Here is a code example of **by-value**:

```js
var a = 3;
var b;

b = a;
/*
    Changing the value of a to 2 will not change the value of b to 2 because b
    is pointing to a different address that contains a copy of the original 
    value of a (=3) before it was changed.
*/
a = 2;
```

* Here is a code example of **by-reference**:

```js
var c = {greetings:'hi'};
var d;

/*
    Equals operator sees that c is an object so it points d to the same 
    location in memory where c object is stored. Therefore if you change the
    property of greetings to another value, say: c.greetings = 'hello'; this
    would be the same as writing d.greetings = 'hello'; because both point
    to the same object stored in memory.
*/
d = c;
```

* **Mutate**: To change something. "Immutable" means it **can't be changed**.
* **By reference **works even when they are passed as parameters. Here is an example:

```js
function changeGreeting(obj) {
    obj.greeting = 'Hola'; //mutate
}
d = { greeting : 'hello' };
changeGreeting(d);
```



