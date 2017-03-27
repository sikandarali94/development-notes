# Arrow Functions

### Fat Arrow Functions

* Arrow functions are one of the most popular by far in ES6.
* The change with ES6 is instead of using the `function` keyword we use a fat arrow `=>` instead. However if like using the `function` keyword we can continue to use it and not adopt the fat arrow `=>` at all. Here is the example:

```js
/*
    This is instead of writing 'const add = function(a,b){}'.
*/
const add = (a,b) => {
    return a + b;
}
add(1,2);
```

* The reason it is called a fat arrow `=>` is because the skinny arrow is `->`.
* If we write a single line of expression like above we can condense it even further with fat arrow functions like this:

```js
/*
    Notice that we do not need the return keyword here, and neither
    do we need the curly braces.
*/
const add = (a,b) => a + b;
```

* When we removed the curly braces and the `return` keyword we gain what is called an **implicit return** of the function. Implicit `return` is just fancy talk for saying whatever the result of `a + b;` expression is it will automatically get returned from the function.

* If we include the curly braces we do not get the benefit of the automatic return and have to put a `return` keyword. Here is an example:

```js
const add = (a,b) => {
    /*
        This expression will not automatically return from the
        function due to there being curly braces.
    */
    a + b;
}
```

### Advanced Use Of Arrow Functions

* Another feature of fat arrow functions is that if we have a single argument we can also omit the parenthesis around it. So we can write this: `const double = (number) => 2 * number;` like this: `const double = number => 2 * number;`.
* If we have no arguments at all we have to put the parenthesis like this: `const double = () => 2;`.
* We must not put a semicolon in the function body like this: `const double = () => (a + b;)`. Instead we could omit the semicolon like this: `const double = () => (a + b)` or put the semicolon outside the function body like this: `const double = () => (a + b);`
* Here is a more complex example of using the fat arrow function:

```js
const numbers = [1,2,3];
numbers.map(function(number) {
    return 2 * number;
});
```

* We can refactor the above code into this:

```js
/*
    Notice how simple this is and that the semicolon is outside the
    function body. Array helpers used with arrow functions can give
    us amazingly concise code as opposed to using a for loop.
*/
numbers.map(number => 2 * number);
```

### When To Use Arrow Functions

* In this code:

```js
const team = {
    members: ['Jane', 'Bill'],
    teamName: 'Super Squad',
    teamSummary: function() {
        return this.members.map(function(member) {
            /*
                This gives the error: Cannot read property 'teamName'
                of undefined.
            */
            return `${member} is on team ${this.teamName}`;
        });
    }
};
```

* Let us figure out the error of the above code through the help of this diagram:

![](/assets/Iterator Function Map Helper Diagram.png)

* The Iterator Function passed into the `map` helper is essentially being passed somewhere into the code base that we don't know about; it is being passed into the ether. Therefore the value of this is lost; so this is not equal to the constant `team`.
* The whole purpose of the fat arrow function is to solve the above issue elegantly.
* Here is the code that solves the above issue:

```js
const team = {
    members: ['Jane', 'Bill'],
    teamName: 'Super Squad',
    teamSummary: function() {
        return this.members.map((member) => {
            return `${member} is on team ${this.teamName}`;
        });
    }
};
team.teamSummary();
```

* The word `lexical` means the placement of this team depends on how it is interpreted or how it is evaluated. So `lexical` `this` with fat arrow functions means that depending upon where we are putting the keyword this it will change when we are using a fat arrow function. When we use a fat arrow function and make reference to this inside of it this is automatically set equal to this in the surrounding context which in this case is the constant `team`.
* In short fat arrow functions make the value of this behave as we would expect and hope it to. By default Javascript context works exactly opposite the manner in which we really want and expect it to.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

