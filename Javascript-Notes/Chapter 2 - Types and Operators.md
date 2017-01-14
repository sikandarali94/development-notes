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

### Operators Precedence and Associativity

* **Operator Precedence**: Which operator function gets called first. Functions are called in order of precedence \(HIGHER precedence wins\).
* **Associativity**: What order operator functions get called in: left-to-right or right-to-left. This is when functions have the **same** precedence.
* When we assign a variable value to another Javascript does that and returns the assigned value. For example:

```js
var a = 2, b = 3, c = 4;

a = b = c;

/* 
   With b = c, here it assigns the value of c to b and returns 4, the assigned value. 
   So it would be a = 4 by the end (right-to-left associativity).
*/
```

![](/assets/FireShot Capture 33 - Operator precedence - JavaScript I MDN_ - https___developer.mozilla.org_en_d.jpg)

### Coercion

* **Coercion**: Converting a value from one type to another. This happens quite often in Javascript because it's dynamically typed.
* When we write, `var a = 1 + '2';` it gives us `'12'`, a string. We say here that `1` was **coerced** into a string due to the `'2'`.

### Comparison Operators

* Coercing a `false` to a number, it becomes `0`. Coercing a `true` to a number, it becomes `1`.
* Sometimes values get coerced into something that we don't expect. For example, coercing `undefined` to a number gives us `NaN` \(Not a Number\). The unexpected thing is that `undefined` did not ultimately coerce to an actual number. However, coercing a `null` to a number becomes a `0`. So, these two behaviours are very unexpected.
* Coercion, although powerful, can be dangerous. For example, `false == 0`, will give us `true` and this is not what we want and can cause bugs. However, when we write `null == 0` it gives us `false` even though we learnt that `null` being coerced into a number will give us `0`, and in this case it didn't do that. This is one of the negative sides to Javascript. When we write "" == 0 it gives us true, and when we write "" == false it asls



