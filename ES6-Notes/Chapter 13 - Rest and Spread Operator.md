# Rest and Spread Operator

### Capturing Arguments With Rest and Spread

* Say we have lots of arguments coming into a function and we want to package these arguments into an array. To do so we can use the rest operator like this:

```js
/*
    The triple dots operator accepts that the function is going to receive an
    unknown number of arguments and what it does is that it captures all the
    arguments and puts it in the variable 'numbers' as an array. The '...' is 
    known as the rest operator.
*/
function addNumbers(...numbers) {
    return numbers.reduce((sum, number) => {
        return sum + number;
    }, 0);
}
addNumbers(1,2,3,4,5,6,7);
```

### The Rest on Rest and Spread

* Say we have two arrays that we want to concatenate together. We can achieve this using the spread operator like this:

```js
const defaultColors = ['red', 'green'];
const useFavoriteColors = ['orange', 'yellow'];

/*
    This will return the array: ['red', 'green', 'orange', 'yellow']. The
    triple dots operator here is known as the spread operator. If we were
    to just write [ ...defaultColors, userFavoriteColors ] then this will
    return ['red', 'green', ['orange', 'yellow']].
*/
[ ...defaultColors, ...userFavoriteColors ];
```

* As opposed to using `concat` method to join arrays the spread operator better visualizes what we are trying to achieve because we can see the square brackets indicating we want to return an array and we can see how we are mapping out the joining of the array.
* Another advantage of the spread operator over the `concat` method is that if we had more than two arrays we wanted to concatenate than the spread operator helps us achieve that more easily than the `concat` method. Another advantage is that we can add single elements easily with the spread method like this:

```js
const defaultColors = ['red', 'green'];
const userFavoriteColors = ['orange', 'yellow'];
const fallColors = ['fire red', 'fall orange'];

const finalArray = ['blue', ...fallColors, ...defaultColors, ...userFavoriteColors];
```

* Here is an example of mixing and matching the rest and spread operator:

```js
function validateShoppingList(...items) {
    if (item.indexOf('milk') < 0) {
        return ['milk', ...items];
    }
    return items;
}
validateShoppingList('oranges', 'bread', 'eggs');
```

### Look To Use Rest and Spread In This Case

* A great use case of the rest operator and spread operator are handling pass throughs of arguments to different functions. Here is an example:

```js
const MathLibrary = {
    calculateProduct(...rest) {
        return this.multiply(...rest);
    },
    multiply(a,b) {
        return a * b;
    }
};
```

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

