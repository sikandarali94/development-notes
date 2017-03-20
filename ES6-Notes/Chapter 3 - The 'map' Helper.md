# The 'map' Helper

### The Map Helper

* Here is a classic example of using a for loop to double the numbers in array and store those numbers in a new array:

```js
var number = [1, 2, 3];
/*
    The reason we use a new array to store the numbers is because in large
    Javascript applications we want to avoid mutating variables as much as
    possible.
*/
var doubledNumbers = [];

for (var i = 0; i < numbers.length; i++) {
    doubledNumbers.push(numbers[i] * 2);
}
```

* This is how we implement the above code using the map helper:

```js
var numbers = [1,2,3];

var doubled = numbers.map(function(number) {
    return number * 2;
});

//This logs [2,4,6]
console.log(doubled);
```

![](/assets/Screen Shot 2017-03-19 at 7.30.35 am.png)

* 


