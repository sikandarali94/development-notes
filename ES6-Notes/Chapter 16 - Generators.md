# Generators

### One Quick Thing: For...Of Loops

* There are even more ways in ES6 to iterate through collections and arrays than we have covered in the previous chapters.
* Even though with the new array helpers it is now discouraged to use for loops there is an aspect in for...of loops that is particularly important to note for our understanding of generators. for...of loops is a **feature** of ES6.
* Here is an example of for...of loops:

```js
const colors = ['red', 'green', 'blue'];

/*
    Here we are iterating through the colors array and each element we iterate
    through is stored in the variable 'color.'
*/
for (let color of colors) {
    console.log(color);
}
```

### Introduction to Generators

* Generators are the motherload of ES6. They are by far the most brain-bending feature. Therefore understanding generators is notoriously hard.
* A generator is a function that can be entered and exited multiple times. Unlike normal functions, with generators we can run some code, return a value and then go right back into the function at the same place that we left it.
* To make a function a generator or to create a generator we put an asterisk next to the `function` keyword. Here is an example:

```js
//Notice the asterisk right next to the function keyword.
function* numbers() {

}
```

* Another way to make a function a generator or to create a generator is to put an asterisk before the function name. Here is an example:

```js
function *numbers() {

}
```

* Let us look at this code which we explain in detail later in the next topic:

```js
function* numbers() {
    //yield is a keyword we put inside a generator.
    yield;
}

const gen = numbers();
//This first expression when logged returns the object: {"done": false}.
gen.next();
//This second expression when logged returns the object: {"done": true}.
gen.next();
```

### Generators With A Short Story

* In the previous topic code if we were to remove the `yield` keyword both the `gen.next()` expressions would have returned the objects `{"done": true}`.
* To explain generators let us use the diagram below to model code:

![](/assets/Generator Story Diagram.png)

* Please note that the line in the diagram marks a distinct transition from going from the sidewalk to the store and exiting the store back to the sidewalk.
* Here is our modelled code from the story in the diagram:

```js
function* shopping() {
    //stuff on the sidewalk.
    
    //walking down the sidewalk.
    
    //go into the store with cash.
    const stuffFromStore = yield 'cash';
    
    //walking back home
    return stuffFromStore;
}

//stuff in the store.
/*
    1. When we first call our generator of shopping and create the 'gen' object
    nothing happens inside of our generator. So just calling shopping() doesn't
    invoke any code.
*/
const gen = shopping();
/*
    2. When we first evaluate gen.next();" is when some code is executed in the
    generator 'shopping'. It executes the part in the diagram when we take
    money inside the store. At this point we are paused after: yield 'cash';.
    This line returns: {"value": "cash", "done": false}. 
*/
gen.next(); //leaving our house.
/*
    3. When we evaluate: gen.next('groceries'); we are modelling the part of the
    diagram where we transition with groceries back onto the sidewalk. 
    Essentially we exchange 'cash' for 'groceries' and it is the same as writing
    from this: const stuffFromStore = yield 'cash'; to this: 
    const stuffFromStore = 'groceries';. Therefore 'stuffFromStore' is assigned
    'groceries', and then we go to the end of the generator and return 
    'stuffFromStore' which is 'groceries.' The gen.next('groceries') line returns:
    {"value": "groceries", "done":true}.
*/
gen.next('groceries'); //leaving the store with groceries.
```



