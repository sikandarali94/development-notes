# Types and Operators

### Types and Javascript \(Conceptual\)

* **Dynamic Typing**: We don't tell the engine what type of data a variable holds, it figures it out while our code is running. In Javascript, variables can hold different types of values because it's all figured out during execution.
* In many other languages we have to specify the variable type explicitly. This is called **Static Typing**.
* If we don't know how Javascript makes a decision when figuring out the type of variable, this can cause us many issues.

### Primitive Types

* There are **six** primitive types in Javascript.
* **Primitive Type**: A type of data that represents a single value. Therefore, an object is not a primitive type because it represents multiple values.
* The primitive types are: **undefined**: represents a lack of existence \[shouldn''t set variable to undefined\]; **null**: represents lack of existence \[we can set a variable to null\]; **boolean**: either _true_ or _false_; **number**: floating point number \[there's always some decimals\]; **string**: a sequence of characters \(both ' ' and " " can be used\); **symbol**: used in ES6 \(the next version of Javascript\).
* While other languages have various number types like int, float, big, etc., Javascript only has one which is the floating point number \(although we can tweak this\).

### Operators \(Conceptual\)

* **Operator**: A special function that is syntactically written differently. Generally, operators take two parameters and return one result.
* When we write: `var a = 3 + 4;` The `+` operator actually is a function that takes the parameters 3 and 4 and returns the sum of the numbers. The ability is called **infix notation**, meaning the function call \(which is `+`\) sits between the two parameters.



