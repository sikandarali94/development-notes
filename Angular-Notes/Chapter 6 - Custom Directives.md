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

* Now that we have poked a text hole. So we can take `personName` variable and now it is available to us as part of our Model, as part of our scope for our directive, for the searchresult.html View.

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
* & in AngularJS means this is a function and we can use it to pass a function from a controller's scope to the custom directive where scope is isolated through a custom attribute. Here is an example:

**main.html**

```
<label>Search</label>
<input type="text" value="Doe"/>
<h3>Search Results</h3>
<div class="list-group">
    <!--
        1. person can actually be different than the parameter listed in the
        function definition in app.js. Angular is just going to look at the
        function and pass the parameters in the appropriate positions. We
        can name person aperson and aperson is essentially just a 
        placeholder.
    -->
    <search-result person-object="person" formatted-address-function="formattedAddress(person)"></search-result>
</div>
```

**app.js**

```js
myApp.controller('mainController', ['$scope', function($scope) {
    $scope.formattedAddress = function(person) {
        //some function code.
    };
}]);

myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        scope: {
            personObject: "=",
            /*
                2. formattedAddressFunction is the normalized name for the
                custom attribute formatted-address-function. The & indicates
                a function has been passed to the custom attribute and is
                being now passed to the isolated scope of the custom
                directive.
            */
            formattedAddressFunction: "&"
        } 
    }
});
```

**searchresult.html**

```
<a href="#" class="list-group-item">
    <h4 class="list-group-item-heading">{{ personObject.name }}</h4>
    <p class="list-group-item-text">
        <!--
            3. An important thing to note is that we can't just pass
            properties like only writing personObject here. We have to
            pass what is called an "object map". The "object map" takes
            the name of the parameter we defined in the custom attribute
            as a property/key and the name of the object in the 
            directive's scope as the value. Essentially an "object map"
            maps the value to the parameter name. If we had multiple
            parameters we can separate key/value with a comma. The need
            to use "object maps" is because the way AngularJS has done
            references in poking this hole into the isolated scope.    
        -->
        {{ formattedAddressFunction({ aperson: personObject }) }}
    </p>
</a>
```

### Repeated Directives

* Here is a way we can repeat directives in AngularJS:

**app.js**

```js
myApp.controller('mainController', ['$scope', function($scope) {
    $scope.people = [
        {
        name: 'Siky',
        address: 'First Street'
        },
        {
        name: 'Harry',
        address: 'Second Street'
        }
    ]
}]);
```

**main.html**

```
<label>Search</label>
<input type="text" value="Doe"/>
<h3>Search Results</h3>
<div class="list-group" ng-repeat="person in people">
    <!--
        We can even put ng-repeat on the directive rather than making
        multiple divs. Therefore we will have multiple 
        <search-result></search-result> directives in a single <div>
        element.
    -->
    <search-result person-object="person" formatted-address-function="formattedAddress(aperson)"></search-result>
</div>
```

### Understanding 'Compile'

* **'Compile' and 'Link'**: When building code, the **compiler** converts code to a lower-level language, then the **linker** generates a file the computer will actually interact with. This is very computer science-y terms that are sort of valid, but not familiar to many web developers...and not what AngularJS does. The behavior of custom directives are kind of analogous to compilers and linkers, but not really.
* Here is how we use the compile behavior in custom directives:

**app.js**

```js
myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        scope: {
            personObject: "=",
            formattedAddressFunction: "&"
        },
        compile: function(elem, attrs) {
            /*
                What we have here when we compile is we can gain access to the
                HTML that defines the View in searchresult.html for the custom
                directive. Therefore elem contains the View of the custom
                directive. Compile runs only once.
            */
            console.log('Compiling...');
            console.log(elem.html());
            /*
                Pre-linking and Post-linking run two times because the 
                ng-repeat loops through two objects items in the people 
                array. They run first Pre-link then Post-link then Pre-link
                then Post-link. Each time the directive is looped and 
                displayed on the screen each directive has its own scope.
            */
            return {
                pre: function(scope, elements, attrs) {
                    console.log('Pre-linking...');
                    console.log(elements);
                },
                post: function(scope, elements, attrs) {
                    console.log('Post-linking...');
                    console.log(elements);
                }
            }
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
    <search-result person-object="person" formatted-address-function="formattedAddress(aperson)"
        ng-repeat="person in people">
    </search-result>
</div>
```

* Compile runs once because it defines the HTML of the View, which in this case is searchresult.html. In the compile function we can change the HTML structure if we want to. Then the linking functions \(pre and post\) that we see outputting here, what they do is let us change the HTML and access it as each directive is created through the loop, along the way. In summary compile means we can change our directive on the fly before it gets used, for example in the compile function we can remove the class attribute with standard JS by writing: `elem.removeAttr('class');`. So we are changing the View HTML on the fly before it is used and being bound to the scope and everything else.
* Link on the other hand is run every time the directive is used over and over again. To explain the difference between pre-link and post-link we have to understand that we can nest directives like this:

```
<search-result>
    <search-result></search-result>
</search-result>
```

* So AngularJS **compiles** the directive then runs pre-link then looks for any other directives inside and compiles that directive then runs pre-link so on and so forth until it gets all the way to the bottom and figures everything out. It then runs post-link back up the chain of the nested directives. Therefore post-link is safer than pre-link because by time we run post-link we already know what we have to deal with. AngularJS in its documentation has recommended to avoid pre-link. So in return of the compile function we can take out the pre-link function.
* In:

```js
/*
    The template or View is passed to the post-link function in
    elements and attrs. The Model is passed to the post-link
    function in scope for that particular instance of the directive.
    It doesn't matter if we call the parameters something different
    it passes the scope to the first parameter. Therefore it
    doesn't examine the names of the parameters and thus isn't
    dependency injections.
*/
post: function(scope, elements, attrs) {
    console.log('Post-linking...');
    console.log(scope);
    console.log(elements);
    /*
        Because we get separate scopes and the personObject has
        different values due to the ng-repeat we can do something
        like this.
    */
    if (scope.personObject.name === 'Siky') {
        elements.removeAttr('class');
    }
}
```

* Most likely we will not need to run code in the compile function but just use the post-link function.

### Understanding 'Link'

* The shorthand way of writing compile and only using the post-link is using the link method like this:

```js
myApp.directive("searchResult", function() {
    return {
        restrict:'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        scope: {
            personObject: "=",
            formattedAddressFunction: "&"
        },
        link: function(scope, elements, attrs) {
            //some code
        }
    }
});
```

### Understanding 'Transclusion'

* Transclusion: Include one document inside another. It means to place a copy of one document at a particular point inside another.
* Say we include something in our search result like this:

```
<label>Search</label>
<input type="text" value="Doe"/>
<h3>Search Results</h3>
<div class="list-group">
    <search-result person-object="person" formatted-address-function="formattedAddress(aperson)"
        ng-repeat="person in people">
        <!--
            We inserted this line but for some reason it doesn't appear
            on the main page.
        -->
        *Search results may not be valid.
    </search-result>
</div>
```

* The `*Search results may not be valid` was removed from the DOM because `<search-result></search-result>` is just a placeholder for the contents in searchresult.html and completely replaces the `*Search results may not be valid` line. To include this line as part of the directive is called transclusion and AngularJS provides a directive called `<ng-transclude></ng-transclude>` in order to do transclusion.. Whatever extra content we want to place inside a particular directive AngularJS puts that content inside `<ng-transclude></ng-transclude>`. Here is how we do it:

**searchresult.html**

```
<a href="#" class="list-group-item">
    <h4 class="list-group-item-heading">{{ personObject.name }}</h4>
    <p class="list-group-item-text">
        {{ formattedAddressFunction({ aperson: personObject }) }}
    </p>
    <!--
        Inside <ng-transclude></ng-transclude> is where the extra content is
        placed.
    -->
    <small><ng-transclude></ng-transclude></small>
</a>
```

**app.js**

```js
myApp.directive("searchResult", function() {
    return {
        restrict: 'AECM',
        templateUrl: 'directives/searchresult.html',
        replace: true,
        scope: {
            personObject: "=",
            formattedAddressFunction: "&"
        }
        /*
            Be default transclude is false. We need to set it to true in
            order to do transclusion.
        */
        transclude: true
    }
});
```

* If we inspect the `*Search results may not be valid` line we get:

```
<ng-transclude>
    <span class="ng-scope">
        *Search results may not be valid.
    </span>
</ng-transclude>
```

* Instead of ng-transclude element we can use an attribute like this:

```
<small ng-transclude></small>
```

* We get then in inspect of the `*Search results may not be valid` line:

```
<small ng-transclude>
    <span class="ng-scope">
        *Search results may not be valid.
    </span>
</small>
```

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

