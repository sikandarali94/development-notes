# Classes

### Introduction to Classes

* Under the hood when we are using classes we are still using prototypal inheritance.
* Here is the classical example of using a constructor function in ES5 to do prototypal inheritance:

```js
function Car(options) {
    this.title = options.title;
}

/*
    Whenever we add methods onto an object prototype we add it to the
    prototype object of the constructor.
*/
Car.prototype.drive = function() {
    return 'vroom';
}

var car = new Car({ title: 'Focus' });
//This will log 'vroom.'
console.log(car.drive());
```

### Prototypal Inheritance

* Here is a continuation of the previous section code when a new object inherits the methods and properties of the `Car` object:

```js
function Toyota(options) {
    Car.call(this, options);
    this.color = options.color;
}

Toyota.prototype = Object.create(Car.prototype);
/*
    When we established Toyota.prototype = Object.create(Car.prototype) the
    constructor pointer points to Car and so we have to reset the constructor
    in Toyota's prototype back to the Toyota function.
*/
Toyota.prototype.constructor = Toyota;

Toyota.protoype.honk = function() {
    return 'beep';
}

var toyota = new Toyota({ color: 'red', title: 'Daily Drive' });
//This will log 'vroom.'
console.log(toyota.drive());
//This will log 'beep.'
console.log(toyota.honk());
```

### Refactoring With Classes

* Here is an example of using classes in ES6:

```js
class Car {
    /*
        We must have the constructor method in classes because this is the
        method that will take the initial arguments from the creation of
        an instance of this class. Therefore we do the initilization here.
    */
    constructor({ title }) {
        /*
            Here we are able to achieve this using destructuring. If in the
            arguments we just wrote a variable say 'options' instead of {title}
            we would then write this line as: this.title = options.title;
        */
        this.title = title;
    }
    //drive() is using object literal syntax,
    drive() {
        return 'vroom';
    }
}

const Car = new Car({ title: 'Toyota' });
```

### Extending Behavior of Classes

* Continuing from the previous code we will create a class that inherits the properties and methods of the `Car` class. This however will produce an error, which we will explain later in how to fix:

```js
/*
    When we use the extends keyword we are saying that we want Toyota to have
    access to all of the methods and properties inside of Car.
*/
class Toyota extends Car {
    constructor({ color }) {
        super();
        this.color = color;
    }
    honk() {
        return 'beep';
    }
}
```

* From the above code here is how `super()` works.

![](/assets/Screen Shot 2017-04-01 at 5.55.13 pm.png)

* In the diagram both `Toyota` and `Car` have a defined  constructor method. But we want to make sure we call the `Car`'s constructor method so we get any initialization or setup code in there to run as well. Whenever we have a subclass like `Toyota` that wants to call a method on the parent\(`Car`\) when that same method is declared on both sides we refer to the `super` keyword. So `super()` is the exact same as saying `Car.constructor();`. Say if both classes had a method of `honk` within the subclass we would write:

```js
honk() {
    super();
    return 'beep';
}
```

* Whenever we use super\(\) we would not use destructuring within the arguments of the constructor method of the subclass. Taking this all into account we write this:

```js
class Toyota extends Car {
    constructor (options) {
        /*
            We are passing along the arguments to super so it can catch the title
            property argument if someone wrote:
            const toyota = new Toyota ({ color: 'red' : title: 'Toyota' });
        */
        super(options);
        this.color = options.color;
    }
    honk() {
        return 'beep';
    }
}
```

### When To Use Classes

* Javascript's community has made a huge embrace of classes.



