# Const/Let

### Variable Declarations With Const and Let

* Nearly all features in ES6 can be put in one of two buckets. The first bucket is features that are syntactic sugar: stuff to help us write less code. The second bucket is features which bring new ideas, constructs or functionality to the language.
* Syntactic sugar are not changing Javascript for the sake of changing it.
* Const, Let and Var are features which bring new functionality to Javascript, which are part of the second bucket.
* When we are using ES6 we are no longer going to be using the keyword `var` in order to declare variables. We either use `const` or `let`.
* `const` is a keyword that is used to declare variables when we expect the value that we are assigning to it to never change. Conversely `let` is used for variables where we expect the value of the variable to change over time.
* In ES6, before declaring a variable, we ask ourselves that do we expect the value of the variable to change. Here is an example illustrating the difference between ES5 and ES6:

```js
// var name = 'Jane';
// var title = 'Software Engineer';
// var hourlyWage = 40;

//ES6 Below

//We don't obviously expect the name to change.
//name = 'Janet'; This will throw up the error: TypeError: Assignment to constant variable.
const name = 'Jane';
//We expect the job title to change in the future obviously.
let title = 'Software Engineer';
//We obviously expect the wage to change. 
let hourlyWage = 40;

//some time later...
title = 'Senior Software Engineer';
hourlyWage = 45;
```

### What Const and Let Solve

* In complex and big Javascript applications we have variable declarations flying all over the place.
* `const` and `let` makes the code name legible to read and thus can be understood better. There are many technical opinions but this is the most basic.
* By changing `var` to either `const` or `let` is called refactoring code.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

