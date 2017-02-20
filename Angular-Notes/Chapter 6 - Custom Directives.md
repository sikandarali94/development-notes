# Custom Directives

### Reusable Components \(HTML\)

* In HTML there is no way to make our own tags. There is an idea on the way called web components that would soon make this easier.
* Say we wanted to create our own HTML structure for search result like this:

```
<h1>Search Results</h1>
<div>
    <h2>Title</h2>
    <p>
        Detail
    </p>
    <a href='#'>...more detail</a>
</div>
```

* For this it would be convenient to package this structure into a tag like this:

```
<searchResult more-detail="#"></searchResult>
```

OR

```
<div searchResult></div>
```

OR

```
<div class="searchResult"></div>
```

* AngularJS brings this concept to us right now. They are called **custom directives**.

### Variable Names and Normalization \(Javascript and Angular\)

* **Normalize**: To make consistent to a standard. Specifically we are dealing with 'text normalization', or making strings of text consistent to a standard.
* One popular standard in HTML and CSS is to make names all lowercase with dashes in between. Here is an example:

```rust
<!--
    Therefore we should follow this standard when we are creating
    reusable components.
-->
<search-result result-link-href="#"></search-result>
```

* However there is an issue in Javascript in regards to this standard. If we write: `var result-link-href='#';` it will give us the error: `Uncaught SyntaxError: Unexpected token -`. It mistakes `-` for minus operator.
* Therefore in Javascript we cannot have dashes in our variable name. Angular however does normalization meaning it converts strings in the form of attributes, class names, elements into a consistent different standard.
* In Javascript the standard of naming is **camel case**, like this `var resultLinkHref = '#';`. Angular can see that with `resultLinkHref = '#'` in JS can be standardized into `result-link-href='#'` for HTML or CSS, and vice versa. We have to remember this to avoid confusion that `resultLinkHref` in JS is `result-link-href` in HTML, because it has been normalized by Angular.

### Creating a Directive

* Say we want to convert this structure into our own search results directive:

```
<a href="#" class="list-group-item">
    <h4 class="list-group-item-heading">Ali, Sikandar</h4>
    <p class="list-group-item-text">
        365 Main St., Alice Springs, SA 1111
    </p>
</a>
```

* Here is an example of creating a directive in Angular:

```
/*
    Angular will normalize searchResult into the HTML standard of search-result.
    This is the name of the directive.
*/
myApp.directive("searchResult", function() {
    /*
        The function returns the actual directive. It will contain properties one
        of which is template where we input the HTML structure as a string that
        we want converted into a directive.
    */
    return { template: ' /*Here goes the HTML structure we written above*/ ' }
});
```

* We can then simply write into HTML: &lt;search-result&gt;&lt;/search-result&gt;. Angular will look at search-result, normalize it into searchResult and look for the directive in the Javascript code and then load the structure into HTML.



