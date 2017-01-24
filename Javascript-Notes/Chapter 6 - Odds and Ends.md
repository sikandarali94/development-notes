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



