# Object Oriented Javascript and Prototypal Inheritance

### Classical vs Prototypal Inheritance \(Conceptual\)

* **Inheritance**: One object get access to the properties and methods of another object.
* **Classical inheritance** is how inheritance has been done for a long time and has been popular with sharing properties and methods between objects. Thinking that this is the only approach and it doesn't have its downsides is wrong. It is very verbose.
* **Prototypal inheritance** is much simpler. It is very flexible, very extensible and easy to understand. However, it is not perfect either.

### Understanding the Prototype

* All objects in Javascript, and that includes functions, have a **prototype** property. This property is just a reference to another object. It's an object that stands on its own. Here is a visual:

![](/assets/Prototype Chain Diagram.png)

* Regarding Prop2, when we call: obj.Prop2 it looks for that property in obj and it doesn't find it. It then looks for Prop2 in the prototype object \(proto {}\) It finds it in this this case. Regarding Prop3, when we call: obj.Prop3 it does the same thing in finding it as it did with Prop2, because a prototype can have another prototype and so forth and Javascript looks from properties down these prototypes that are linked via the Prototype Chain.
* It looks like Prop2 is on our obj but its actually on our object's prototype. The same is for Prop3. This way down in searching for properties in the prototype is called the Prototype Chain.
* As shown in the diagram above, if I have obj2 it can point to the prototype of **obj**.Therefore objects can share the same prototype if they have to. If we call: **obj2.Prop2**, it is going to point to same address in memory that **obj2.Prop2** does. So obj and obj2 are sharing Prop2 but not directly, instead via this concept in the Javascript Engine called the Prototype Chain. Prop2 isn't on obj and obj2 its just that when the Javascript Engine goes down the chain to search they happen to be at the same place.
* There is a way a browser allows us to access the prototype of an object but we should **NEVER EVER** use it in real life. Here is that way for demo purposes:

```js
var person = {
    firstname: 'Default',
    lastname: 'Default',
    getFullName: function() {
                    return this.firstname + ' ' + this.lastname;
                 }

var john = {
    firstname: 'John'
}
/*
    Javascript has made sure we never accidentally type this due to
    double underscore, because that is how important is it never to
    use it!
*/
john.__proto__ = person;

/*
    This will log 'John' rather than 'Default' because the JS Engine
    looks first for firstname in the object and because it has found
    it does not go looking for it down the Prototype Chain.
*/
console.log(john.firstname);
var jane = {
    firstname = 'Jane'
}

jane.__proto__ = person;
/*
    This will log 'Jane Default' because 'this' variable will point
    to the Jane object and since it found firstname as Jane it will
    log that. For lastname it will first look for it in Jane and
    because it isn't there it will search it in the prototype and
    find it as 'Default'.
*/
console.log(jane.getFullName());
```

### Everything is an Object \(or a Primitive\)

* Everything in Javascript that isn't a primitive is an object. Meaning they all have a prototype, except for one thing: a base object.
* Within say an object the prototype object has methods like toString or valueOf and so forth.
* The prototype of all functions is: `function Empty(){}`. So what is this? This prototype object has like `apply()`, `bind()` and `call()` that define the `this` variable as we discovered already. So this is where functions get these methods, it is within the prototype object, that Javascript builds into functions.
* An array also has special methods within the prototype that Javascript builds into it, like `indexOf`, `hasOwnProperty` and so forth.
* As we discussed before that prototypes can have prototypes. This chain is not infinite. At some point the prototype in the chain will be a base object, and this does not have a prototype and so the prototype chain ends here. For example an array prototype's prototype is a base object.

### Reflection and Extend

* **Reflection**: An object can look at itself, listing and changing its properties and methods.
* Here is an example of reflection and extend:

```js
var person = {
    getFullName: function() {
                    return this.firstname + ' ' + this.lastname;
                 }
}

var john = {
    firstname: 'John',
    lastname: 'Doe'
}

//don't do this EVER!
john.__proto__ = person;

//The ability to list its members here is part of reflection.
for (var prop in john) {
    if (john.hasOwnProperty(prop)) {
        console.log(prop + ': ' + john[prop]);
    }
}

var jane = {
    address: '111 Main St.',
    getFormalFullName: function() {
                          return this.lastname + ', ' + this.firstname;
                       }
}

var jim = {
    getFirstName: function() {
                     return firstname;
                  }
}

/*
    This is not built into Javascript, instead we are using the underscore.js framework.
    What this essentially does is compiles these object, meaning it takes all the
    properties and methods of these objects, and adds it directly to john. In the
    future Javascript will have the in-built function of extends which sets the
    prototype of objects to extend objects from one another.
*/
_.extend(john, jane, jim);
```



