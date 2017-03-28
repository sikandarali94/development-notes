# Enhanced Object Literals

### Enhanced Object Literals

* Enhanced object literals are all about writing out object literals in a more compact fashion. This is **syntactic sugar**.
* First feature of an enhanced object literal is if the key and value have the exact same name like this: `inventory: inventory,`. We can condense this down to just `inventory,`.
* Second feature of an enhanced object literal is if we have a key-value pair and value is a function like this: `inventoryValue: function() {}`. In ES6 we can omit the function keyword and the colon as well, which will give us: `inventoryValue() {}`.

### Wondering When To Use Enhanced Literals?

* A good convention is to place the condensed key-value pairs as a single value before the non-condensed key-value pairs like this:

```js
$.ajax({
    url,
    data,
    method: 'POST'
});
```

---

The main credit for these notes goes to Stephen Grider. He has a great set of courses which can be found at, [https://www.udemy.com/user/sgslo/](https://www.udemy.com/user/sgslo/)

