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

```js
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

* We can then simply write into HTML: `<search-result></search-result>`. Angular will look at `search-result`, normalize it into `searchResult` and look for the directive in the Javascript code and then load the structure into HTML.
* Angular by default will wrap the structure we provided as the template into the `<search-result></search-result>` element tags. If we didn't want this to be wrapped with the element tags we can do this:

```js
myApp.directive("searchResult", function() {
    return {
        template: ' /* We put our HTML structure here */',
        replace: true
    }
});
```

* This will completely where we write the `<search-result></search-result>` tags with only the template structure and not wrapped with the &lt;search-result&gt;&lt;/search-result&gt; tags.
* There is another way to tell Angular that we are using a custom directive. Here is an example:

```
<!--
    Angular will look at the search-result and find the custom directive in
    the Model.
-->
<div search-result></div>
```

* We can restrict which implementation of custom directives is allowed in HTML. Here is how we do it:

```js
myApp.directive("searchResult", function() {
    return {
        /*
            We can use 'AE' or 'EA' for attribute and element implementation in
            HTML. We use 'E' to restrict it to element implementation only or 
            'A' to restrict it to attribute implementation only. 'AE' is the
            method, the people that built Angular, recommend.
        */
        restrict: 'E',
        template: ' /* We put our HTML structure here */',
        replace: true
    }
});
```

* Since `'AE'` is the recommended way there are methods to implement custom directives beyond `'AE'` but not by default. `'AE'` or `'EA'` is the default. The class implementation is `<div class="search-result"></div>`. The comment implementation is `<!-- directive: search-result -->`. To allow these implementations we write `'C'` for class implementation in the restrict property \(we have to specify 'C' because it is not allowed by default\), and we write 'M' for comment-implementation in the restrict property \(we have to specify 'M' because it is not allowed by default\). Here is an example:

```js
myApp.directive("searchResult", function() {
    return {
        /*
            This allows attribute, element, class and comment implementation.
            We can mix and match these however we like.
        */
        restrict: 'AECM',
        template: ' /* We put our HTML structure here */',
        replace: true
    }
});
```

### Templates

* The HTML structure we put in our template property for custom directives can get pretty large and unwieldy to manage. Angular allows us to manage this. We can create a folder named 'directives' or whatever we like and create 'searchresult.html' or whatever we like. In the HTML file we write the HTML structure for the template. We can link to that structure for the template like this:

```js
myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        // Instead of template property we use templateUrl property.
        templateUrl: 'directives/searchresult.html',
        replace: true
    }
});
```

### Scope \(@, =, and other obtuse symbols\)

* Anything for the Model that contains the directive the directive also has access to. In other words for example the Model for the `searchResult` directive in searchresult.html is also the Model for the main HTML that has `ng-app = "myApp"`. In the Model if we define:

```js
$scope.person = {
    name: 'Siky',
    address: '55 Main St.'
}
```

* We can then interpolate this within searchresult.html like this:

```
<a href="#" class="list-group-item">
    <h4 class="list-group-item-heading">{{ person.name }}</h4>
    <p class="list-group-item-text">
        {{ person.address }}
    </p>
</a>
```

* However this can have unintended consequences. If the directive is doing things to items and elements in the `$scope` that we are not anticipating under those particular circumstances. See we've connected the `$scope` for the parent page directly to the custom directive therefore the directive can then affect the data on the parent page and maybe we are under a situation where we don't realize it's not desired.
* So AngularJS provides a method to isolate a custom directive, the Model part of the directive, from the Model for whatever page contains the directive. This is called **isolated scope**, or **isolating the scope**.
* Here is how we isolate the scope and also poke holes in the isolated scope to get values from the main controller Model.

**app.js**

```js
myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        /*
            1. By introducing the scope property we have isolated the scope
            meaning the scope property here becomes the Model and
            searchresult.html becomes the View. Therefore the parent page
            cannot affect the data for this custom directive or vice versa.
            This avoids accidents.
        */
        scope: {
            /*
                3. Since the value of the custom attribute is just text
                we'll use a text hole for example. We are poking a hole that
                is just text, in other words, 'Local Text Binding'. The
                property personName is a normalized version of person-name.
                The @ symbolizes that we poked a text hole, so @ just means
                text. The @ symbol is one-way binding.
            */
            personName: "@",
            /*
                If we wanted to change the name of the property instead of
                having the normalized name of the custom attribute we have
                to reference the custom attribute as personName with @ at
                the beginning to tell Angular it is a text hole.
            */
            personNameSpecial: "@personName"
        }
    }
});
```

**main.html**

```
<label>Search</label>
<input type="text" value="Doe"/>

<h3>Search Results</h3>
<div class="list-group">
    <!--
        2. The way we poke holes in AngularJS is through custom attributes
        like person-name here which is normalized to personName for the
        Model. Here we give the custom attribute an interpolated value from
        our scope in main.html. person.name comes from the main controller
        model.
    -->
    <search-result person-name="{{ person.name }}"></search-result>
</div>
```

**search.html**

```
<a href="#" class="list-group-item">
    <!--
        4. We can then grab the value we grabbed through the text hole and
        interpolate it in the directive's View.
    -->
    <h4 class="list-group-item-heading">{{ personName }}</h4>
    <p class="list-group-item-text">
        <!--
            Here it cannot find person.address because the scope of the
            directive we have isolated it from the scope of the main
            controller.
        -->
        {{ person.address }}
    </p>
</a>
```

* Now that we have poked a text hole. So we can take personName variable and now it is available to us as part of our Model, as part of our scope for our directive, for the searchresult.html View.

* Here is how we pass an object from the controller's Model into the Model of a directive that has an isolated scope:

**main.html**

```
<label>Search</label>
<input type="text" value="Doe"/>

<h3>Search Results</h3>
<div class="list-group">
    <!--
        1. We don't write {{ person }} because that means text of the
        object and that really doesn't make sense. We just pass the object
        into the custom-attribute by name.
    -->
    <search-result person-object="person"></search-result>
</div>
```

**app.js**

```js
myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        scope: {
            /*
                2. The equals sign tells us that this is a two-way binding
                and we can pass an object into the directive via the 
                personObject property on the scope. Two way binding means
                whatever happens to the object inside this directive will
                update the object passed in from outside. So we have to be
                very careful with this. @ is one-way binding.
            */
            personObject: "="
        }
});
```

**searchresult.html**

```
<a href="#" class="list-group-item">
    <!--
        3. Now we have access to the object with all its properties and
        methods.
    -->
    <h4 class="list-group-item-heading">{{ personObject.name }}</h4>
    <p class="list-group-item-text">
        {{ personObject.address }}
    </p>
</a>
```

* Be wary in trying to do too much as far as having the directive affect the scope that's being passed in. If we affect the object too much because it is two way binding it can cause pages to reload in unexpected ways and cause interface issues.
* Angular is not perfect with making software applications; nothing is.



