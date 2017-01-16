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

/*
    This will log d as: {greeting : 'Hola'} because obj parameter
    variable will point to the same location in memory where the 
    d object is stored.
*/
console.log(d);
```

* The equal operator sets up memory space \(new address\), so in the above example if we later write: `d = {greetings : 'Bonjour!'}` this will create a new memory space and put the object in there and `d` will point to the memory space where the new object is stored. So doing `d.greeting = 'Hello';` mutates the object whereas doing `d = {greetings : 'Bonjour!'}` creates a new object and stores it at a different memory address and `d` points to that address.

### Objects, Functions and 'this'

![](/assets/Execution Context ('this' variable) Diagram.jpg\)

* When you simply create a function like this in the Global lexical environment: `function a() { console.log(this); }`, and we invoke the function the `this` variable \(or keyword\) will point to the Global object which is called `window`. This is even true for an anonymous function like this, where lexical environment is Global:

```js
var b = function() {
    // the 'this' variable points to Global object.
    console.log(this);
}
```

* Here is an example of creating a method in Javascript:

```js
var c = {
    name : 'The c object',
    log: function() {
            /*
                    The this keyword placed within the method of c object will point to
                    the c object itself. We can even say: this.name = 'Updated';
            */
            console.log(this);
    }
}

// We can invoke the method like this.
c.log();
```

* Here is a weird way Javascript works with the `this` keyword: 

```js
var c = {
    name : 'The c object',
    log : function() {
            var setname = function (newname) {
                    /*
                            Rather than this modifying the name property of the c object,
                            because we expect the 'this' keyword here to point to the c
                            object, instead creates a variable called name in Global
                            which holds the value 'Updated again!'. Many people believe
                            that shouldn't be the behaviour.
                    */
                    this.name = newname;
            }
```

* The way we can fix this is by writing: `var self = this;` within the log method. Because this is an object `self` will point to the same address where the `c` object is stored. Instead of then using `this` keyword we can use the `self` variable, and this wouldn't cause `self` to accidentally point to the Global object within a function within the log method of the `c` object as we saw in the dot point before.

### Arrays - Collections of Anything \(Conceptual\)

* We can create an array using the array literal syntax like this:

```js
//The square brackets is the array literal syntax
var arr = [];

//This is the non-preferred way.
var arr = new Array();
```

* Arrays in Javascript are special compared to many other languages because Javascript is dynamically typed, whereas in many others we can only have an array of a certain type like integer or string. We can mix and match in Javascript; say, we can have a number, then a boolean, then an object, then even a function.
* Say we have an array like this:

```js
var arr = [
    1,
    false,
    {
        name: 'Sikandar',
        address: '111 Main St.'
    },
    /*
        We can invoke this function and pass a parameter the value of the name
        property of the object stored in the array through this expression:
        arr[3](arr[2].name);
    */
    function(name) {
        var greeting = 'Hello';
        console.log(greeting + name);
    },
    "hello"
];
```

### 'arguments' and spread

* In frameworks we'll come across arguments quite often in the source code.
* When a function is run it creates the execution context as shown below:

![](/assets/Execution Context Functions Diagram.jpg)

* **Arguments**: The parameters we pass to a function. Javascript gives us a keyword of the same name which contains them all, like this:

```js
function greeting (firstname, lastname, language) {
    console.log(arguments);
}

greeting("Sikandar", "Ali", "en");
```

* Concerning, _console.log\(arguments\);_, this will give us an array-like \(array-like is set up by Javascript that behaves and acts like an array but isn't an array\) set that holds the arguments like this: _\[ _'John', 'Doe', 'en' _\]_. The slanted square brackets indicate that this is an array-like.
* Even though arguments isn't exactly an array it behaves like one:

```js
//Like an array we can check the length of the array-like.
if (arguments.length === 0) {
    console.log('Missing Parameters!');
}

//Like an array we can grab specific values using an index.
console.log(arguments[0]);
```

* As years go on arguments will become deprecated, meaning they will still be around it just won't be the best way to do something. The thing to replace arguments in the future is called a spread parameter.
* A spread allows us to do this:

```js
/*
    If we pass parameters to this function beyond the main parameters like 
    this: greet('Sikandar', 'Ali', 'es', '111 main street', 'sydney');
    '111 main street' and 'sydney' will be put into an array called other
    (because we wrote ...other in the function parameter definition).
*/
function greet(firstname, lastname, language, ...other) {}
```

### Function Overloading \(Framework\)

* Function Overloading; the ability is not available in Javascript as functions are objects. Function overloading is the idea that we can have a function of the same name that has different numbers of parameters.
* A pattern in Javascript allows us to call a function but necessarily does not have to pass a certain parameter. It is this:

```js
function greet(firstname, lastname, language) {}

//We don't have to pass the language parameter due to this pattern.
function greetEnglish(firstname, lastname) {
    greet(firstname, lastname, 'en');
}

function greetSpanish(firstname, lastname) {
    greet(firstname, lastname, 'es');
}

//Much simpler function call rather than remembering English is 'en'.
greetEnglish('Sikandar', 'Ali');
greetSpanish('Sikandar', 'Ali');
```

### Syntax Parsers \(Conceptual\)

* The Javascript Engine in the browser is a syntax parser. It goes through the code **character** by **character** making assumption, stating various rules, and can even make changes to our code before its executed.

### Automatic Semicolon Insertion \(Dangerous\)

* Semicolons are optional in core Javascript. This is because the Javascript engine does something for us. If it sees one character at a time it knows what to expect. If it sees that we are finishing a line and we press carriage return, the syntax parser says, 'Hey, you can't continue the next line', then automatically inserts a semicolon at the end of the line. So if it expects that there should be a semicolon at a particular place it automatically puts it there.
* However, we don't want Javascript making that decision for us. That is why we should put our own semicolons where appropriate.
* Here is an example of how a flaw can creep in our code when we don't put a semicolon ourselves and the Javascript engine makes the decision for us:

```js
function getPerson() {
    /*
        Javascript automatically puts a semicolon after return even though we want
        to return an object. This is caused by the carriage return. 
    */
    return
    {
        firstname: 'Sikandar'
    }
}

console.log(getPerson()); //This will log undefined.
```

* To fix the above problem we must put the opening curly brace on the same line of return, like this:

```js
return {
    firstname: 'Sikandar'
}
```

### Whitespace \(Framework\)

* **Whitespace**: Invisible characters that create literal 'space' in our written code. These are carriage returns, tabs, and spaces.
* Javascript is very liberal with whitespace. Here is an example of how liberal it can be:

### Immediately Invoked Function Expressions \(IIFEs\)

* This is an example of IIFEs:

```js
var greeting = function(name) {
    console.log('Hello ' + name);
/*
    The round brackets here means that the function is immediately invoked after it
    is created. When it is immediately invoked the greeting variable will store the
    value returned by the function and not the function itself. 
*/
}();
```

* If we write:

```js
/*
    This will return an error [ unexpected token ( ] because the Javascript engine
    expects this to have a name given to the function after 'function'. This means
    the Javascript engine expects this to be a statement because of its syntax
    rules. 
*/
function(name) {
    return 'Hello ' + name;
}
```

* When we wrap this intended expression in round brackets like this:

```js
/*
    Javascript understands that we are intending to write a function expression 
    rather than a function statement. Because Javascript Engine has a rule that if 
    the first thing we write on a new line is 'function' then it must be a 
    statement. By wrapping it in round brackets we have explained it to circumvent 
    this rule and accept it as an expression. This makes sense because we only use 
    round brackets to enclose an expression and not a statement.
*/
(function(name) {
    return 'Hello ' + name;
});
```

* We can use the point above to write an IIFE like this:

```js
(function(name) {
    var greeting = 'Hello';
    console.log(greeting + ' ' + name);

//We can invoke it outside the brackets like this: " )(); ".
}());
```

* An advantage of this is that we can invoke functions without having to store them or their returned value. We will see this repeatedly in every framework of Javascript out there.

### IIFEs and Safe Code \(Framework\)

* What goes behind the scenes of an IIFE is that during the creation phase a Global Execution Context is created, but because the IIFE is an expression the function expression is not placed in the Global Execution Context. It is only during the execution phase when it runs the IIFE that the function is placed in the Global Execution Context. We know when a function is run a new Execution Context is created and is placed at the top of the Execution Stack. Any variables declared in the anonymous function is not placed into the Global Execution Context but the Execution Context of the anonymous function. That is what makes this very handy.
* Here is a visual representation of what we discussed above:

![](/assets/IIFE Execution Context Diagram.png)

* The reason we mentioned above -- about what makes an IIFE handy when the variables inside it are not placed in the Global Execution Context -- is because if we have two script files stacked in HTML atop one another we won't have a clash of variables are not placed inside the Global Execution Context. This makes code safe.

* In a great deal of libraries and functions if we open their source code the very first thing we will see at the top os the opening of IIFE and at the bottom the ending of the IIFE.

* To pass a reference to the Global object we can do something like this:

```js
(function (global, name) {
    //To make this a global variable we can write global.greeting = 'Hello';.
    var greeting = 'Hello';
    //window is the global variable.
}(window, 'John'));
```

### Understanding Closures

* Closures are essential to gain a deep understanding of Javascript. However, it is notoriously difficult to understand.
* Here is an example of invoking a function that is returned by a function:

```js
function greet(whattosay) {
    return function(name) {
              console.log(whattosay + ' ' + name);
           }
}

/*
    This makes sense because greet('Hi') returns a function and that
    function is invoked due to ('Tony') parameter. This will log 'Hi Tony'.
*/
greet('Hi')('Tony');

/*
    How is it possible that when we state sayHi('Tony'); it remembers that
    whattosay stored 'Hi' even though the greet function has popped off
    the execution stack. This is possible because of closures.
*/
var sayHi = greet('Hi');
sayHi('Tony');
```

* When an execution context has popped off the execution stack the memory space of the variable environment for that execution context still exists. Eventually Javascript will clear that memory through a process known as garbage collection, but for our purposes in the running code it still exists. In the code above when greet execution context has popped off the execution stack the whattosay variable still exists in memory. The interesting thing is that when we invoke the function expression that sayHi points to, within its outer environment the reference to the whattosay variable still exists. So when we mention the whattosay variable within the anonymous function, that sayHi points to, and it doesn't have the variable within its variable environment then it goes up the scope chain and finds whattosay within the references inside its outer environment. In this way we say the Execution Context has **closed in** its outer variable, the variables it would have a reference to anyway even though the execution context has gone. This phenomenon is known as a closure. Here is a visual representation of this:

![](/assets/Closure Diagram.png)

* The above representation is after the greet\(\) execution context has popped off and the anonymous function that sayHi points to is invoked. This closure is created. This was the representation in the beginning:

![](/assets/Closure Beginning Diagram.png)

### Understanding Closures \(Part 2\)

* Here is a classical example code when the topic of closures is brought up:

```js
function buildFunctions() {
    var arr = [];
    /*
        i is known as a free variable. A free variable is a variable that is
        outside a function but that we have access to.
    */
    for (var i = 0; i < 3; i++) {
        arr.push(
            /*
                When it is storing the function it stores i as an address
                to memory space. It doesn't invoke the function and reveal
                the value that i points to in the memory space at that
                point.
            */
            function() {
                console.log(i);
            }
        )
    }

    return arr;
}

var fs = buildFunctions();

/*
    That is why here when the stored functions are invoked they then
    look for i up the scope chain and find the reference. The value
    that i references in memory is the value 3, the final value when
    buildFunctions()'s execution context is popped off the execution
    stack.
*/
fs[0]();
fs[1]();
fs[2]();
```

* To get the result of 1,2,3 and 3,3,3, we can do this:

```js
function buildFunctions() {
    var arr = [];
    for (var i = 0; i < 3; i++) {
        arr.push(
            /*
                We are using an IIFE with the parameter i to invoke
                the function that returns a function, but because j
                is being created several times the function that it
                returns is the variable j in each separate function
                pointing to addresses separately that has 0, 1 and 
                2 stored. So when it is invoked it will print out
                0, 1, and 2 because the j's in the functions each
                point to a different address in memory.
            */
            (function(j) {
                return function() {
                    console.log(j);
                }
            }(i))
        )
    }
    return arr;
}

var fs2 = buildFunctions2();
fs2[0]();
fs2[1]();
fs2[2]();
```

### Function Factories \(Framework\)

* When we execute functions that are the same but are invoked at separate occasions their execution contexts are different, meaning the variables they point to sit in separate area of memory. For example:

```js
function makeGreeting(language) {
    return function(firstname, lastname) {}
}

/*
    The closure of this function points to the space in memory
    where the value of the language variable is 'en'. The one
    below it its closure points to the space in memory where
    the value of the language variable is 'es'. Therefore when
    these functions are invoked they will have different
    execution contexts.
*/
var greetingEnglish = makeGreeting('en');
var greetSpanish = makeGreeting('es');
```

* Creating functions from other functions are called function factories.

### Closures and Callbacks

* When we use the inbuilt function of setTimeout that we are actually using closures. Here is an example:

```js
function sayHiLater() {
    var greeting = 'Hi!';
    setTimeout(function() {
        //This will log 'Hi!'
        console.log(greeting);
    }, 3000);
}
sayHiLater();
```

* **Callback Function**: A function we give to another function, to be run when the other function is finished. So the function we call \(i.e. invoke\), 'calls back' by calling the function you gave it when it finishes. Here is an example:

```js
function tellMeWhenDone(callback) {
    var a = 1000;
    callback();
}
tellMeWhenDone(function() {
    console.log('I am done!');
});
```

### call\(\), apply\(\), and bind\(\)

* Functions have access to special methods which are built in. They are `call()`, `apply()` and `bind()`. The three methods have all to do with the `'this'` variable.
* Here is an example of using the `bind()` method:

```js
var person = {
    getFullName: function() {
                    var fullname = this.firstname + ' ' + this.lastname;
                    return fullname;
                 }
}

var logName = function(lang1, lang2) {
    console.log('Logged: ' + this.getFullName());
}

/*
    This method returns a new function. So it actually makes a copy of
    logName(); and sets up this new function object -- this new copy --
    so that whenever its run, its execution context is created, the
    Javascript engine sees that person was created with this bind and
    sets up hidden things in the background. When the Javascript engine
    sets up the 'this' variable it decides it must be the person object.
*/
var logPersonName = logName.bind(person);
logName();
```

* We could even do this:

```js
var logName = function(lang1, lang2) {/*some code*/}.bind(person);
```

* `logName.call(person);` This calls the function logName and sets the 'this' variable to the person object. It works the same as `logName();` but lets us control what the 'this' variable would be. We could also pass it parameters like this:

```js
//First parameter is for 'this' variable, the second is first parameter...
logName.call(person,'en','es');
```

* `logName.apply(person);` does exactly the same thing as the call method except that it takes parameters as an array, like this: `logName.apply(person, [ 'en', 'es']);` and not like: `logName.apply(person, 'en', 'es');` as this would give us a 'Type Error: Arguments list has wrong type'.
* We could use these three methods to an immediately invoked function. Here is an example:

```js
(function(lang1, lang2) {
//Notice we don't have () here.
}).apply();
```

* These methods are useful in **function borrowing**. Here is an example:

```js
var person = {
    firstname: 'Sikandar',
    lastname: 'Ali',
    getFullName: function() {
                    var fullname = this.firstname + ' ' + this.lastname;
    }
}

var person2 = {
    firstname: 'Zane'
    lastname: 'Ali'
}

/*
    Here we invoke the function of the person object but use the
    variable of firstname and lastname of person2. Secondly, we
    don't need to type out the function again in person2 we can
    just function borrow getFullName() from person object.
*/
person.getFullName.apply(person2);
```

* The `bind()` method especially is useful in 'function currying'. Here is an example:

```js
function multiply(a,b) {
    return a * b;
}

/*
    The first parameter sets the this variable. In this case
    we are personally setting the parameter 'a' to the value
    of 2.
*/
var multipleByTwo = multiply.bind(this, 2);
```

* **Function Currying**: Creating a copy of a function but with some preset parameters. This is very useful in mathematical situations.

### Functional Programming

* Javascript is more inline with **Functional Programming**. These are languages that have first-class functions. Javascript is most powerful when we code it in terms of functions.
* We use functions to limit repetition. First-class functions let you avoid repetition to a larger extent than programming languages that do not have first-class functions.

### Functional Programming \(Part 2\)

* These advanced concepts help us in **'open source education'**, which means opening the source code of frameworks and understanding how they work. Underscore.js is one library which is great for 'open source education'.





