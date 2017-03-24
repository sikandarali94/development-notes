# The 'reduce' Helper

### Condensing Lists With Reduce

* This is a tough helper to get our heads around but it is one of the most flexible. It is so flexible in fact that we can probably reimplement every other helper we've covered by just using the `reduce` helper.
* Here is a classical example of using a for loop to mimic what a `reduce` helper can do:

```js
var numbers = [10, 20, 30];
var sum = [];

for (var i = 0; i < numbers.length; i++) {
    sum += numbers[i];
}
```

* Here is an example of using a `reduce` helper:

```js
/*
    The sum stores the initial values until the Iterator Function goes through
    all the elements in the array and the final value it returns will be the
    final value of sum. This sum is not the sum variable initialized above.
*/
numbers.reduce(function(sum, number) {
    return sum + number;
/*
    The 0 is called the initial value. We can have any arbitrary value here
    like 1000 and thus will return 1060.
*/
}, 0);
```

![](/assets/Reduce Helper Diagram.png)

* The Iterator Function is getting two arguments: the initial value and an element from the array. The value that the Iterator Function returns then becomes the initial value in the next passthrough and the second argument is the next argument in the array. After all elements have been passed into the Iterator Function the value that it returns becomes the final return value.
* Here is a more interesting use of the `reduce` helper:

```js
var primaryColors = [
    { color: 'red' },
    { color: 'yellow' },
    { color: 'blue' }
];

primaryColors.reduce(function(previous, primaryColor) {
    previous.push(primaryColor.color);
    return previous;
}, []);
```

### Ace Your Next Interview With Reduce

* Many people say reduce is for summing up numbers but it can be used for a whole lot more.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

