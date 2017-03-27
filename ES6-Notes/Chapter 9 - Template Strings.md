# Template Strings

### Template Strings

* Template strings are in the bucket of syntactic sugar that makes our code a little bit easier to write.
* Template strings are also known as **Template literals**; either way is correct.
* In "The year is " + year; we are joining a string with the variable year. A template string is just a nicer way of Javascript variables with a string.
* With template strings we don't use double quotes or even single quotes, instead we use back ticks \(\`\) that is to the left of the 1 keyboard. To write a variable within these back ticks we write a `$` with opening and closing braces like this: `${}`. Inside of it we write the variable name like this: `${year}`. We can even do calculations like this: `${year - 1}`, or put a whole expression like this: `${new Date().getFullYear()}`. Therefore we can put any valid Javascript expression within a template string.
* Here is an example of using a template string:

```js
function getMessage() {
    return `The year is ${new Date().getFullYear()}`;
}
getMessage();
```

### When To Reach For Template Strings

* The main benefit of template strings is to make string concatenation or string interpolation far more legible.
* A common silly mistake is this:

```js
const year = 2016;
/*
    This is hilariously silly because we can just write year on the right
    hand side instead of `${year}`.
*/
const yearMessage = `${year}`;
```

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

