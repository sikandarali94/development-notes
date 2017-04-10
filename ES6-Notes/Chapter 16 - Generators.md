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
    This line returns: {value: "cash", done: false}. 
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
    {value: "groceries", done:true}.
*/
gen.next('groceries'); //leaving the store with groceries.
```

* The `yield` statement can be used multiple times in a generator to make multiple stops within the generator. Here is an example:

```js
function* shopping() {
    const stuffFromStore = yield 'cash';
    const cleanClothes = yield 'laundry';
    return [stuffFromStore, cleanClothes];
}

const gen = shopping();
//This returns: {value:"cash", done:false}.
gen.next();
//This returns: {value:"laundry", done:false}.
gen.next('groceries');
//This returns: {value:["groceries","clean clothes"], done:true}.
gen.next('clean clothes');

```

### The Big Reveal On ES6 Generators

* The **big reason** for using generators is that they are made to work perfectly with for...of loops. Here is an example:

```js
function* colors() {
    yield 'red';
    yield 'blue';
    yield 'green';
}

const myColors = [];
for (let color of colors()) {
    myColors.push(color);
}

//This will log: ["red","blue","green"]
console.log(myColors);
```

* Therefore we can see that the for...of loop works perfectly with generators as we stated earlier. This is where really generators and for...of loops start to get much more interesting. When we use them in tandem we can build up objects that will iterate through any type of data structure we can possibly imagine. That is therefore the purpose of generators.

### A Practical Use of ES6 Generators

* Here is our practical example:

```js
/*
    Say we only want to iterate through the names of the engineering team
    and not the 'size' and 'department' and store it in the array. The
    generator and for...of loop below does this for us.
*/
const engineeringTeam = {
    size: 3,
    department: 'Engineering',
    lead: 'Jill',
    manager: 'Alex',
    engineer: 'Dave'
};

function* TeamIterator(team) {
    yield team.lead;
    yield team.manager;
    yield team.engineer;
}

const names = [];
for (let name of TeamIterator(engineeringTeam)) {
    /*
        By the end of the for...of loop names would store the array
        ['Jill','Alex','Dave'].
    */
    names.push(name);
}
```

### Delegation of Generators

* Here is the diagram that we will model code after to learn about generator delegation:

![](/assets/Generator Delegation Story Diagram.png)

* From the diagram we want to iterate through both the engineering team names and the testing team names.
* To iterate through the names of both teams it is better suggested to create separate generators for each team and iterate through both the teams using a single for...of loop.
* In this case generator delegations means iterating through the `engineeringTeam` down to the `testingTeam` as seen in the diagram. Here is how we achieve generator delegation through code:

```js
const testingTeam = {
    lead: 'Amanda',
    tester: 'Bill'
}

const engineeringTeam = {
    /*
        Using literal object syntax we do not need to type 
        testingTeam:testingTeam.
    */
    testingTeam,
    size: 3,
    department: 'engineering',
    lead: 'Alex',
    manager: 'Steve',
    engineer: 'Sophie'
}

function* TeamIterator(team) {
    yield team.lead;
    yield team.manager;
    yield team.engineer;
    const testingTeamGenerator = TestingTeamIterator(team.testingTeam);
    /*
        To tell the for...of loop to fall into the second generator we put
        an asterisk next to the yield keyword and pass in the variable
        testingTeamGenerator that refers to the second generator. yield*
        tells Javascript that it has another generator with yield statements
        that it should care about.
    */
    yield* testingTeamGenerator;
}

function* TestingTeamIterator(team) {
    yield team.lead;
    yield team.tester;
}

const names = [];
for (let name of TeamIterator(engineeringTeam)) {
    /*
        names stores in the end the array: 
        ['Alex', 'Steve', 'Sophie', 'Amanda', 'Bill'].
    */
    names.push(name);
}
```

* `yield*` is what makes Generator Delegation possible.



