# Single Page Applications

### Multiple Controllers, Multiple Views \(Angular\)

* We can have as many areas of the page attached to different controllers and different models as we want. Here is an example:

**JS**

```js
var myApp = angular.module('myApp', []);
//Multiple Controllers [Model]
myApp.controller('mainController', ['$scope', function($scope) {
    $scope.name = 'Main';
}]);
myApp.controller('secondController', ['$scope', function($scope) {
    $scope.name = 'Second';
}]);
```

**HTML**

```
<html ng-app="myApp">
    <!-- some code above -->
    <div class="container">
        <!-- Multiple Controllers [View] -->
        <div ng-controller="mainController">
            <!-- This will interpolate 'Main'. -->
            <h1>{{ name }}</h1>
        </div>
        <div ng-controllers="secondController">
            <!-- This will interpolate 'Second'. -->
            <h1>{{ name }}</h1>
        </div>
    </div>
    <!-- some code below -->
</html>
```

* The `$scope.name` of `'mainController'` is in a different memory space as compared to `$scope.name` of `'secondController'`. This is because Angular injects a different version of the `$scope` object for different controllers to avoid values overriding one another.

### Single Pass Apps and the Hash \(HTML and Javascript\)

* For `<a href="#bookmark">Go to Bookmark</a>`, the `#` tag and what comes after is known as a fragment identifier because it identifies a pattern in the HTML document that has `id="bookmark"`.
* In Javascript there is an event known as 'hashchange' that fires when the hash changes, when the fragment identifier changes. Here is how we use the event listener:

```js
window.addEventListener('hashchange', function() {
    //This property stores the value of the current hash.
    console.log('Hash Changed!: ' + window.location.hash);
});
```

* The event listener above can still be fired even if the element with the id of `id="bookmark"` does not exist on the page and we still click the anchor `<a href="#bookmark">Go to Bookmark</a>`.
* If in the URL we change `127.0.0.1:53403/index.htm#bookmark3` to `127.0.0.1:53403/index.htm#/bookmark/3` the `window.location.hash` stores `#/bookmark/3`. We can then do something with this behavior to make `#/bookmark/3` mean something, like this:

```js
window.addEventListener('hashchange', function() {
    if(window.location.hash === '#/bookmark/3') {
        console.log('Page 3 is great.');
    }
}
```

* Therefore we can run code based on hash changes, for example `#/bookmark/2` hash change can get data for Page 2. These hash changes in the URL can therefore mimic real pages when we decide to do something with it, meaning in Javascript we can look at the value of the hash and do whatever we want.
* This concept of fragment identifiers is key to implementing single page applications.
* A **single page application** is one where we only officially download, have the browser do a complete http request of a html page once, so when we look at the URL it's just one page and then we use asynchronous requests or AJAX or have the browser get values off the Internet behind the scenes and specify what it should get using fragment identifiers. By using Javascript we get the particular contents that are associated with the hash values that change to mimic URL paths, so we can pretend they are pages.
* AngularJS uses this concept to build singe page applications.

### Routing, Templates and Controllers

* AngularJS has a service called $location that lets it pick up what the hash value is in the URL. Here is an example:

```js
myApp.controller('mainController', ['$scope', '$location', '$log', function($scope, $location, $log) {
    /*
        The path() method in the $location service returns the
        hash value.
    */
    $log.info($location.path());
}]);
```

* `angular-route.js` helps us build single page applications It helps us route whatever is the hash value and then run the proper code and grab the proper HTML. The module is called `ngRoute`. This is how we use it:

**JS**

```js
var myApp = angular.module('myApp', ['ngRoute']);

/*
    This injects the $routeProvider service, which is within
    angular-route.js, and is available because we wrote
    'ngRoute' as a dependency in angular.module().
*/
myApp.config(function($routeProvider) {
    /*
        $routeProvider lets us specify routes meaning what should
        Angular do when it sees a particular thing in the hash.
        Not just anything it will also match patterns.
    */
    $routeProvider
        /*
            When takes in two things in particular. Firstly the
            templateUrl, which is the physical location of a
            particular template. Secondly, the controller which
            specifies what controller it should be connected to.
            Altogether it tells Angular that if it sees '/' in
            the hash use the template we specified and connect
            it or bind it to the controller that we specified.
            So, the template is the View and the Model is in
            the controller.
        */
        .when('/', {
            templateUrl:'pages/main.html',
            controller:'mainController'
        })
        .when('/second', {
            templateUrl:'pages/second.html',
            controller:'secondController'
        })
});

myApp.controller('mainController', ['$scope', '$log', function($scope, $log) {
    $scope.name = 'Main';
}

myApp.controller('secondController', ['$scope', '$log', function($scope, $log) {
    $scope.name = 'Second';
}
```

**index.html**

```
<!--
    The ng-view directive says is whenever we are doing single
    page applications and the user specifies in the route to
    get a specific template that template goes into this
    element.
-->
<div ng-view></div>
```

**main.html**

```
<h1>This is Main</h1>
<!--
    This will interpolate the $scope value of name from
    whatever controller that we specified in the route
    to be bound to and in this case it is 'mainController.'
-->
<h3>Scope Value: {{ name }}</h3>
```

**second.html**

```
<h1>This is second.</h1>
<h3>Scope Value: {{ name }}</h3>
```

* In AngularJS v1.6.0 the `$location` hash-bang URLs has changed from empty string \(' '\) to the bang \('!'\). For the previous examples to go between main.html and second.html we need to write the anchor tags like this:

```
<ul class="nav navbar-nav navbar-right">
    <!-- Please note the '#!' -->
    <li><a href="#!"> Home</a></li>
    <li><a href="#!/second"> Second</a></li>
</ul>
```

* We can do pattern matching in Angular. Say we put a question mark at the end of the URL string and pass it values. Essentially we want to give pages values. Here is an example with Angular routing:

```js
myApp.config(function($routeProvider) {
    $routeProvider
        /*
            :num is pattern matching. We can have as many of these as
            we want, for example '/second/:num/something/:id'. After
            the slash after 'second' Angular expects a value.
        */
        .when('/second/:num', {
            templateUrl: 'pages/second.html',
            controller: 'secondController'
        })
});
```

* The value Angular expects at ':num', when we provide it, Angular can get that value in the controller model with `$routeParams`, which is available due to the `ngRoute` module. Here is an example continuing on from the above code:

```js
myApp.controller('secondController', ['$scope', '$log', '$routeParams', 
    function($scope,$log,$routeParams) {
        /*
            Whatever pattern we set out with the name after the : the value is
            stored by Angular with the same name. In this case the value is
            stored under num because we wrote the pattern ':num'. Therefore
            the variable name is num.
        */
        $scope.num = $routeParams.num;
}]);
```

* The routing will not work if we provide the URL say, '127.0.0.1:61971/index.html/\#!/second' because it did not match the pattern of '/second/:num'. We need to provide a value in the URL matching our pattern, for example, '127.0.0.1:61971/index.html/\#!/second/3'. This is route and the value stored in $routeParams.num will be '3'.
* One reason routing is the preferred way to build modern web applications is because it avoids blinking and thus is better for the UI. Another reason is speed; under normal circumstances when we go to the second page we will have to download the whole HTML but with routing we only download a fraction of the website instead of re-downloading the header and footer and so forth.

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

