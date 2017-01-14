# Objects and Functions

### Objects and the Dot

* In Javascript objects and functions are very mush related and should be learned together and not separately.
* An object can have properties and methods. So an object can have a primitive \(property\), an object can have another object \(property\), and an object can have a function \(method\).

![](/assets/Object Diagram.png)

* In this example:

```js
var person = new Object;

person["first name"] = "Sikandar";
/*
    The square brackets is an operator. Below, it creates a space in memory that name,
    "last name" and the object will get an address of location of the name.
*/
person["first name"] = "Ali";

var firstNameProperty = "first name";
console.log(person);
console.log(person[firstNameProperty]);
```



