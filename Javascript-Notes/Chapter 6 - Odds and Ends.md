### Initialization

* For big initialization it is helpful either to store a large object or array in a JSON type without literally typing it in the code, or use a good IDE to color highlight the data to make it easier to read and figure out the structure.

### 'typeof', 'instanceof', and Figuring Out What Something Is

* Javascript has utilities to help us figure out what type of data something is. However it is not perfect!
* Here is an example of using `typeof`. The syntax is important:

```js
var a = 3;
console.log(typeof a);
```

* If we use `typeof` on array it returns the string `'object'`. It is not so useful. Instead we should use: `Object.prototype.toString.call(d);` This returns `[object Array]` and is more useful.
* The keyword `'instanceof'` tells us if any objects down the prototype chain are a certain object that we specify. For example: `e instanceof Person;` tells us if `Person` object is somewhere down the prototype chain of `e`. It either returns `true` or `false`. 
* One thing that has been a bug in Javascript since forever is when we do: `typeof null;` it says it is an `'object'`. It has been around for too long to fix.
* If we do a `typeof` of a function it returns the string `'function'` which is specific and useful.

### Strict Mode

* There is a way to tell Javascript to parse our code in a stricter way than its usually liberal way. This can help us prevent errors in certain circumstances. To use this write `"use strict";` at the very top of the Javascript code. For example: `persom = {};` Javascript will create a variable called `persom` that points to an empty object even though we didn't write: `var persom = {};`. This can be avoided in strict mode, and it will give us an Uncaught Reference Error.

* We can also write `"use strict";` at the top of a function and Javascript will parse the code inside the function specifically in strict mode.

* Not every Javascript Engine uses strict mode  the same way. They don't all agree on the same rules. In MDN there is an article which talks about how strict mode is implemented.

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

