# Destructuring

### Guideline of ES6: Destructuring

* Destructuring is going to be one of the most commonly used features of ES6. Destructuring gives us the most flexibility in writing the style of our code than any other feature.
* Here is an example of ES5 code that has duplication which we will later fix with ES6's destructuring:

```js
var expense = {
    type: 'Business',
    amount: '$45 USD'
};

/*
    In this code we've got 'type' twice, 'expense' twice, 'amount' twice,
    and 'var'twice.
*/
var type = expense.type;
var amount = expense.amount;
```

* Destructuring in ES6 is all about making assignments like: `var type = expense.type;` like assigning properties off of an object a little bit easier to do by reducing the amount of duplicate code. Here is an example of destructuring:

```js
/*
    Important to especially note that with the curly braces here we are
    NOT creating an object. These curly braces say I want to create a new
    variable called 'type' and I want it to reference the 'expense.type'
    property.
*/
const { type } = expense;
const { amount } = expense;
```

* Above we have `const` repeated twice and expense repeated twice. The good news is that with destructuring we can reduce the above code down even more. Here is how we do it:

```js
/*
    The variable name must be identical to the property name of the
    object here.
*/
const { type, amount } = expense;
```



