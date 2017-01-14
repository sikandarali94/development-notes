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

![](/assets/Operator Precedence Table.jpg)

### Coercion \(Conceptual\)

* **Coercion**: Converting a value from one type to another. This happens quite often in Javascript because it's dynamically typed.
* When we write, `var a = 1 + '2';` it gives us `'12'`, a string. We say here that `1` was **coerced** into a string due to the `'2'`.

### Comparison Operators

* Coercing a `false` to a number, it becomes `0`. Coercing a `true` to a number, it becomes `1`.
* Sometimes values get coerced into something that we don't expect. For example, coercing `undefined` to a number gives us `NaN` \(Not a Number\). The unexpected thing is that `undefined` did not ultimately coerce to an actual number. However, coercing a `null` to a number becomes a `0`. So, these two behaviours are very unexpected.
* Coercion, although powerful, can be dangerous. For example, `false == 0`, will give us `true` and this is not what we want and can cause bugs. However, when we write `null == 0` it gives us `false` even though we learnt that `null` being coerced into a number will give us `0`, and in this case it didn't do that. This is one of the negative sides to Javascript. When we write `"" == 0` it gives us true, and when we write `"" == false` it also gives us `true`. Another weird thing is when we write `null < 1` it gives us `true` even though when we write `null == 0` it gives us `false`. This makes our code difficult to anticipate.
* Therefore, it is recommended to use strict equality \( `===` \). This compares two values but doesn't try to coerce them. This is a life saver! Similarly we have strict inequality \( `!==` \). Therefore don't use == unless we consciously want coercion or we definitely know we are comparing same value types and coercion will definitely not happen. Use, however, `===` and `!==` by default!

![](/assets/Sameness Comparison Table.jpg)

### Existence and Booleans

* When we coerce `undefined` to a boolean it becomes `false`. When we coerce a `null` to a boolean it becomes `false`. When we coerce an empty string \( `""` \) to a boolean it also becomes `false`.
* If we put a value within the brackets of an if statement it will try and coerce to a boolean. For example:

```js
var a;

if (a) {
/*
    No matter what value is a it will try and coerce it to a boolean. If a is either
    undefined, null or an empty string the if statement will fail.    
*/
}
```

* Therefore we can use the if statement to our advantage to see if a variable has a value. However, the caveat is that if the variable has a `0` stored it will be coerced by the if statement to `false`. To get away from this caveat we can do something like this:

```js
var a;

a = 0;

/*
    Below, the strict equal has a higher precedence than the ||, therefore the strict
    equal operation will run first, and evaluate to true. Then we'll have "false || 
    true" and this will also evaluate to true.     
*/
if (a || a === 0) {
    console.log('Something is there.');
}
```

### Default Values

* In this code:

```js
function greet(name) {
    /*
        Even though we haven't passed a parameter when we called the greet() function
        the variable name will be set to a default value of undefined. The + operator
        coerced undefined of name to the string 'undefined'.
    */
    console.log('Hello ' + name + '!');
}

greet();
```

* The next version of Javascript called ES6 will introduce syntax to set the default value. In current Javascript code there is a neat trick to do that:

```js
function greet(name) {
    name = name || '<Your name here>';
    console.log('Hello ' + name + '!');
}
```

* To explain the behaviour of the last code example, when we do `undefined || 'some string'`, it returns `'some string'` through coercion, because `'some string'` coerces to `true` and the `||` operator returns whatever is `true`, even if it is coerced. If we do `"First String" || "Second String"` it will return `"First String"` because it was the first thing that was coerced to `true`. When we coerce `undefined` to a boolean it returns `false` and when we coerce `'some string'` it will return `true`. So the `||` operation will return the first value that coerces to `true` when being coerced into a boolean. Similarly, `0 || 1` will return `1` because coercing a `0` to a boolean will return `false`. The reason `||` operation is evaluated first before the `=` operation is because `||` is a higher precedent operation.

### Default Values \(Framework\)

* When we include multiple script files in HTML using &lt;script&gt; Javascript doesn't create separate execution contexts for the script files, rather it stacks the content of the files on top of one another and executes them as they were a single file.
* If we have the same variable name across two js files that we included in our HTML, the variable will behave on how the code is run if the two js files were stacked on top of one another, and run as a single js file. For example:

![](/assets/JS Files Stacked Diagram.jpg)

* This can be fixed with the coercion trick: `window.libraryName = window.libraryName || "Lib 2";` in lib2.js.
* That coersion trick is found in a lot of frameworks. This in itself could cause a lot of errors, however, it is likely the developer's fault for colliding with the name, that he or she is using.







