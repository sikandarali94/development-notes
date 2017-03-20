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

* In the above diagram we take 1 and pass it to the Iterator Function, the Iterator Function multiplies it by two and it returns the number 2. On the second passthrough we pass 2 to the Iterator Function and it does the same thing. We do this for every element in the array. The values the Iterator Function returns is sequentially mapped into the new array.

* If we don't put a return statement in the Iterator Function then the map will return undefined, for example:

```js
var doubled = numbers.map(function(number) {
    number * 2;
};
//This will log [undefined,undefined,undefined]
console.log(doubled);
```

* One of the most common uses of map that we are going to be looking at is kind of collecting properties off of an array of objects. Here is an example:

```js
/*
    From here we only want to know the price of each car store it
    in an array.
*/
var cars = [
    {model: 'Buick', price: 'CHEAP'},
    {model: 'Camaro', price: 'expensive'}
];

var prices = cars.map(function(car) {
    return car.price;
});

//This will log ["CHEAP", "expensive"]
console.log(prices);
```

### Where Map Is Used

* map is one of the most used helpers in front-end development. In many website development projects one of the main tasks is rendering lists of data and therefore map is most helpful for it.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

