# The 'forEach' Helper

### Array Helper Methods

* These are the popular helpers in Javascript that were popular in libraries such as lodash and underscore and are now codified into ES6: forEach; map; filter; find; every; some; reduce.
* The goal here is to use these helper methods instead of the for loop.

### The forEach Helper

* To go through an array like this: `var colors = ['red', 'blue', 'green'];`, we would usually write a for loop like this:

```js
for (var i = 0; i < colors.length; i++) {}
```

* The problem with for loops is that there is a lot of places where we can make errors like for example the order of the arguments or putting in semi-colons.
* Instead we can use the simpler forEach helper like this:

```js
/*
    We pass an anonymous function into the forEach method. We also call this
    function the iterator function. The color variable is what holds a single
    element that has been currently passed by the forEach helper from the array.
*/
colors.forEach(function(color) {
    console.log(color);
});
```

![](/assets/Screen Shot 2017-03-19 at 7.08.58 am.png)

* In the diagram above the `forEach` helper method takes the first element in the array and passes it to the Iterator Function. `'red'` is passed to the Iterator Function and runs and goes off and does something and it returns. After the Iterator Function returns we then take the next element in the array and pass it to the Iterator Function. This goes on and on until all the elements in the array have been passed to the Iterator Function.
* The reason to use `forEach` rather than a for loop is that `forEach` dramatically reduces the code we need to write and thus there is less room for errors.
* Here is an example of using the `forEach` helper to sum the values in an array:

```js
//Create an array of numbers
var numbers = [1,2,3,4,5];

//Create a variable to hold the sum
var sum = 0;

//Loop over the array, incrementing the sum variable
numbers.forEach(function(number) {
    sum += number;
});

//Print the sum variable
console.log(sum);
```

* We can build a separate function and pass it into the `forEach` helper like this to use as an Iterator Function:

```js
function adder(number) {
    sum += number;
}

numbers.forEach(adder);
```

### Why use forEach?

* forEach is better than for loops because it dramatically reduces the code we have to write and thus there is less room for errors.



