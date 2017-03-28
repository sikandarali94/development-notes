# Default Function Arguments

### Specifying Default Function Arguments

* Specifying default function arguments is another bit of syntactic sugar in ES6.
* In AJAX requests GET is by the far the most common request. Therefore we can reflect that with default function arguments:

```js
/*
    If the user did not specify the method for the AJAX request we can
    assume the user wanted a 'GET' request because it is so common. This
    will be 'GET' if we do not provide the method argument for this
    function. If we pass 'undefined' into the method argument we will
    get method as the default 'GET'.
*/
function makeAjaxRequest(url, method = 'GET') {}
```



