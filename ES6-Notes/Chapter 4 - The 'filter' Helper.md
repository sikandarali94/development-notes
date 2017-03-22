# The 'filter' Helper

### Selecting Needed Data With Filter

* filter is a tough helper to understand. Here is an example of how to use a for loop to filter through an array of objects:

```js
/*
    Say we only want to get the objects from the array that have the
    type 'fruit'.
*/
var products = [
    {name: 'cucumber', type: 'vegetable'},
    {name: 'banana', type: 'fruit'},
    {name: 'celery', type: 'vegetable'},
    {name: 'orange', type: 'fruit'}
];

var filteredProducts = [];

for (var i = 0; i < products.length; i++) {
    if(products[i].type === 'fruit') {
        filteredProducts.push(products[i]);
    }
}
```

![](/assets/Screen Shot 2017-03-20 at 8.02.26 am.png)

* `filter` works as : In the diagram above we take an element from our source array and we pass it to our Iterator Function. That Iterator Function either has to return a truthy or falsy value. If it returns a truthy value it will be included in the results array, otherwise if it returns a falsy value the element from our source array will not be included in the new array.
* This is how we achieve the result of the previous for loop code using the 'filter' helper:

```js
filteredProducts = products.filter(function(product) {
                       /*
                           This statement either returns a truthy or falsy
                           value and there is no need to use an if statement.
                       */
                       return product.type === 'fruit';
                   });
```

* Here is an example of writing a complex filter:

```js
/*
    We want to write a filter where the type is vegetable, the quantity is greater
    than 0 and the price is less than 10.
*/
var products = [
    { name: 'cucumber', type: 'vegetable', quantity: 0, price: 1 },
    { name: 'banana', type: 'fruit', quantity: 10, price: 15 },
    { name: 'orange', type: 'fruit', quantitiy: 3, price: 5 }
];

products.filter(function(product) {
    //This will be true for only the celery.
    return product.type === 'vegetable'
           && product.quantity > 0
           && product.price < 10
});
```

### Choosing When To Filter

* filter is great for when we are working with relational data.

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

