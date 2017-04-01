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

### Destructuring Arguments Object

* Destructuring deserves its own book because of the many ways to make use of destructuring. 
* We can use destructuring to pull properties off an object that are passed to functions. Here is an example of using ES5 to pull properties off an object without destructuring:

```js
var savedFiled = {
    extension: 'jpg',
    name: 'repost',
    size: 14040
};

function fileSummary(file) {
    return 'The file ' + file.name + '.' + file.extension + ' is of the size ' + file.size;
}

fileSummary(savedFiled);
```

* Here is how we reduce the code using ES6 destructuring:

```js
const savedFiled = {
    extension: 'jpg',
    name: 'repost',
    size: 14040
};

/*
    Here destructuring let us pull the properties off an object with same
    variable names and assign them to the variables with the same name as
    the property names.
*/
function fileSummary({ name, extension, size }) {
    return `The file ${name}.${extension} is of size ${size}`;
}

fileSummary(savedFiled);
```

* If we wanted to pull from the arguments the data from this function call: `fileSummary(savedFiled, { color: 'red' });` we would have to do this: `function fileSummary({ name, extension, size }, { color }){}`.

### Destructuring Arrays

* With destructuring we are not limited to pulling properties from an object; we can also pull values from an array using destructuring. If destructuring objects is all about pulling properties from objects then destructuring arrays is all about pulling individual elements off of an array. Here is an example of destructuring arrays:

```js
const companies = [
    'Google',
    'Facebook',
    'Uber'
];

/*
    Note around 'name' are square brackets.
*/
const [name] = companies;

/*
    This will log 'Google' because with destructuring array the individual
    elements are assigned to variables in the order they are written in the
    array originally.
*/
console.log(name);
```

* If we were to write for the above: `const [name, name2, name3] = companies;` then `name` would be `'Google'`, `name2` would be `'Facebook'`, and `name3` would be `'Uber'`. There is nothing wrong with adding `name4` to the destructuring array expression; in this case `name4` will be `undefined` but writing `name4` will not give an error.
* Here is an interesting scenario. In this code:

```js
const companies = [
    'Google',
    'Facebook',
    'Uber'
];

/*
    The variable 'length' will be assigned 3 because the curly braces literally
    pull off the 'length' property from the companies which has obviously 3 as
    the 'length' property value because companies array has 3 element inside it.
*/
const { length } = companies;
```

* We can use the spread operator within destructuring. Here is an example:

```js
/*
    'name' would be assigned 'Google' and rest would be assigned the array
    ['Facebook', 'Uber'].
*/
const [ name, ...rest ] = companies;
```

### Destructuring Arrays and Objects \*At the Same Time\*

* One aspect of destructuring that is often overlooked is destructuring arrays and object at the same time. Here is an example:

```js
/*
    'location' stores 'Mountain View'. Here we destructured the array first and
    so the square brackets are first encased. The destructuring of the array
    returns: { name: 'Google', location: 'Mountain View' }. Then we destructured
    at the same time the object  along with the variable name 'location' by
    encasing the variable name 'location' with curly brackets inside the square
    brackets and thus the 'location' variable stores 'Mountain View'.    
*/
const companies = [
    { name: 'Google', location: 'Mountain View' },
    { name: 'Facebook', location: 'Menlo Park' },
    { name: 'Uber', location: 'San Franciscoe' }
];

const [{ location }] = companies;
```

* Here is a more challenging example of destructuring arrays and objects at the same time:

```js
//Here we want to grab 'Mountain View'.
const Google = {
    locations: ['Mountain View', 'New York', 'London']
};

/*
    The constant 'location' will store 'Mountain View'. First we destructured the
    object Google with the curly braces and the 'locations' property key 
    ( 'const{locations} = Google ) which returns the array
    [ 'Mountain View', 'New York', 'London' ]. Then we provide directions for the
    destructuring with the colon (:) and then saying to grab the first element in
    the array and assign it to 'location' on the right hand side of the colon
    using the square brackets and the variable name.
*/
const { locations: [ location ] } = Google;
```

### So...When To Use Destructuring?

* With destructuring we can pass arguments into a function without remembering the order of the arguments. Here is an example of passing arguments into function without destructuring:

```js
function signup(username, password, email, dateOfBirth, city) {}

/*
    The issue here is that we have to remember the order of the arguments and
     if we have long lines of code this can be tedious and confusing.
*/
signup('username', 'password', 'example@email.com', '1,1,1916', 'Berlin');
```

* The issue of remembering the order of the arguments can be solved with destructuring. Here is an example:

```js
/*
    It doesn't matter in the order which we destructure and thus this
    creates ease of mind and flexibility writing code.
*/
function signup({email, password, dateOfBirth, city, username}){}

/*
    For destructuring we should include our argument as properties within
    objects.
*/
const user = {
    username: 'myname',
    password: 'mypassword',
    email: 'myemail@example.com',
    dateOfBirth: '1/1/1900',
    city: 'Berlin'
}

/*
    We simply drop the object into our function and destructuring take
    care of the rest.
*/
signup(user);
```

* Say we want to map out a series of pairs of points into an array of object where each point is mapped into an object like this: `{ x : point1, y : point2 }`. Here is how we do it:

```js
const points = [
    [4,5],
    [10,1],
    [0,40]
];

//We destructure the array directly in the argument.
points.map(([x,y]) => {
    //This is taking advantage of literal objects.
    return { x, y };
});
```

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

