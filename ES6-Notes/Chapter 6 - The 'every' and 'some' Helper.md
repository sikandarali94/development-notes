# The 'every' and 'some' Helper

### A Little Every and a Lot of Some

* `every` and `some` are very similar in how they work.

![](/assets/Screen Shot 2017-03-21 at 8.30.44 am.png)

* For every element in the array we are going to take an element, we are going to pass it to our Iterator Function and that Iterator Function will return a boolean value. Once all the boolean values are returned we are going to look at all the booleans that have been returned and kind of do an `&&` operation between each of them and then produce a final result of either `true` or `false`.
* This is how we can demonstrate the helper using a typical for loop:

```js
var computers = [
    { name: 'Apple', ram: 24 },
    { name: 'Compaq', ram: 4 },
    { name: 'Acer', ram: 32 }
];

/*
    This boolean determines whether all computers can run the program that
    requires at least 16gb.
*/
var allComputersCanRunProgram = true;
/*
    This boolean determines whether any of the computers can run the program
    that requires 16gb ram.
*/
var onlySomeComputersCanRunProgram = false;

for (var i = 0; i < computers.length; i++) {
    var computer = computers[i];
    if (computer.ram < 16) {
        allComputersCanRunProgram = false;
    }
}
```

* Here is an example of using the `every` helper:

```js
computers.every(function(computer) {
    /*
        This will return false because not all the elements in the array
        satisfy this query.
    */
    return computer.ram > 16;
});
```

![](/assets/some helper diagram.png)

* This is similar to `every` helper except we kind of do an `||` operation between each of them and then produce a final result of either `true` or `false`.
* Here is an example of using the `some` helper:

```js
/*
    This will return because there is at least one element in the array that
    satisfies this query. Notice how similar it is to the every helper.
*/
computers.some(function(computer) {
    return computer.ram > 16;
});
```

### Every and Some Syntax

* Here is a more interesting use of the `some` and `every` helper:

```js
/*
    We want to know if either all of the names have a character length of
    greater than 4 or any of the names have a character length of greater
    than 4.
*/
var names = [
    "Alexandria",
    "Matthew",
    "Joe"
];

names.every(function(name) {
    //This will return false because of "Joe."
    return name.length > 4;
});

names.some(function(name) {
    //This will return true because of "Alexandria" and "Matthew."
    return name.length > 4;
});
```

### Every and Some In Practice

* `every` and `some` are great for data validation and form validation.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

