# Data Binding and Directives

### Scope and Interpolation

* **Interpolation**: Creating a string by combining strings and placeholders. `'My name is' + name` is interpolated, and results in `'My name is Sikandar'`.
* Whatever is sitting in the `$scope` object is available in the View.
* One way to do interpolation in AngularJS is to use `{{ }}`. Whatever is inside these double curly braces will be interpolated by AngularJS. Here is an example:

```
<html ng-app="myApp">
    <div ng-controller="mainController">
        <!--
            Angular here will look in the $scope object for the name property and
            interpolate it within the HTML. Therefore $scope is the glue that
            holds the data in the Model and is used by Angular to display that
            data in the View. Note we don't write: $scope.name because we don't
            need to, as it is assumed it is within the $scope object.
        -->
        <h1>Hello {{ name }}!</h1>
    </div>
</html>
```

* We can even do: `{{ name + '. How are you?' }}` and Angular interpolates it for us.
* When we view this h1 element by inspecting element in Google Chrome we get the interpolated result. However, if we inspect source in Google Chrome we get the uninterpolated code, like this: `<h1>Hello {{ name }}!</h1>`. In inspect source we get the code before the DOM is created and in inspect element we get the code after the DOM is created.
* $timeout is a service in Angular that lets us run code after a certain time has elapsed. Here is an example:

```js
myApp.controller('myController', ['$scope', '$timeout', function($scope,$timeout) {
    $scope.name = 'Sikandar Ali';
    $timeout(function() {
        $scope.name = 'Everybody';
    );
}]);
```

* This happens because Angular knows we have a Model and a View binded by a controller. So when the value of name is changed Angular knows to update the View because the data in the Model has changed and Model and View are binded to each other.

### Directives and Two Way Data Binding

* **Directive**: An instruction to AngularJS to manipulate a piece of the DOM. This could be 'ADD A CLASS', 'HIDE THIS', 'CREATE THIS', etc.
* In Javascript we usually manually changed what was going on in the DOM, the in-browser memory representation of the HTML code. So we usually change the website on the fly. AngularJS prefers that we use directives because it is more powerful, more flexible and more easy to use.
* Two examples of directives that we have already used is the ng-app and ng-controller.
* The ng-model directive tells Angular that I want the particular element which has this custom attribute to be bound to a specific property or variable in the $scope object. Here is an example:

**HTML**

```
<div ng-controller="mainController">
    <div>
        <label>What is your twitter handle?<label>
        <!--
            This is a two way data bind meaning whatever is updated in this
            element or View will update the controller or Model and
            vice-versa.
        -->
        <input type="text" ng-model="handle"/>
    </div>
</div>
```

**JS**

```js
myApp.controller('mainController', ['$scope', function($scope) {
    $scope.handle='';
}]);
```

* With the above code Angular can grab the data from the View, update it in the Model, and interpolate it in the View. So we can append this in the div with the custom attribute: ng-controller="mainController" and type: `<h1>twitter.com/{{ handle }}</h1> //This is bound but not two-way bound just data-bound`
* Here is a way to always display the lowercase value typed in the input element:

**HTML**

```
<div ng-controller="myController">
    <div>
        <label>What is your twitter handle?</label>
        <input type="text" ng-model="handle"/>
    </div>
    <h1>twitter.com/{{lowercasehandle()}}</h1>
</div>
```

**JS**

```js
myApp.controller('myController', ['$scope', '$filter', function($scope,$filter) {
    $scope.handle='';
    $scope.lowercasehandle = function() {
        return $filter('lowercase')($scope.handle);
    }
}]);
```

* Here is a visual representation of what we did in the code above:

![](/assets/Model, View, Whatever Example Diagram.png)

* Regarding the question mark we need to understand what this middle piece that make Angular **MV\***.

### The Event Loop \(Javascript\)

* Event Loop is where Javascript continuously checks for events that occur in the browser and if there is code we wrote associated with a certain event Javascript then runs that code, and keeps looking for further events. This is basically known as the Event Loop.

### Watchers and the Digest Loop

* AngularJS is adding Event Listeners for us. It is extending the Javascript Event Loop, doing more with it, in order to automatically control the binding between the Model and the View. AngularJS adds on the Angular Context on top of the Javascript Event Loop that conforms to the Angular architecture. If we follow these patterns, for example we add something to the $scope object and display it on the page, AngularJS knows it needs to keep track of that variable\(s\), or functions. So it adds **Watchers**. Essentially there is a **Watch List** and every time we display a $scope variable or function return on the display page AngularJS automatically adds a Watcher for the Watch List. What it means is that it is keeping track of the original value and the new value every time something might have happened that changed the value. This checking for change is known as the **Digest Loop**, separate to the Javascript Event Loop, that Angular has created.
* When we enter the Digest Loop Angular asks if anything has changed by going through every single variable in the Watch List and compares the new value to the old value. If one of the things on the Watch List has changed then it updates everywhere that is connected to it, anywhere in the DOM and anywhere in the code that is affected by that change. Then it runs one more cycle to see if by changing that has it changed anything else until all of the old values and the new values match and then it stops for the moment until the Event Loop tells us that something else should possibly be checked.

** **![](/assets/Watchers and the Digest Loop Diagram.png)

* Here is an example that illustrates how the watch list and digest cycle work. Please note that the handle variable is being used from the twitter example:

```js
$scope.$watch('handle', function(newValue, oldValue) {
    console.info('Changed!');
    console.log('Old Value: ' + oldValue);
    console.log('New Value: ' + newValue);
});
```

* Here is an example of the limitation of AngularJS:

```js
setTimeout(function() {
    /*
        Here Angular didn't check whether the handle variable change and
        thus didn't update the DOM in the twitter handle example that we
        created before.
    */
    $scope.handle = 'newtwitterhandle';
    console.log('Scope changed!');
}, 3000);
```

* The above conundrum is because `$scope.handle = 'newtwitterhandle';` is outside the Angular Context. It didn't realize that the `handle` variable changes and thus didn't start the Digest Loop through the Watch List. This can cause funky errors in our code.
* One way to fix this is using the Angular $apply method like this:

```js
setTimeout(function() {
    /*
        The $apply method tells Angular that whatever we put inside
        the parameter function should be added to the Angular Context.
    */
    $scope.$apply(function() {
        $scope.handle = 'newtwitterhandle';
        console.log('Scope changed!');
    });
}, 3000);
```

* How do we know when to add code into the $apply function parameter because Angular sometimes adds our code into the Angular Context for us and sometimes it doesn't? Specifically for the above example we can use the Angular $timeout service that does exactly as we have done with the setTimeout\(\) and $apply method. Here is how we do it:

```js
$timeout(function() {
    $scope.handle = 'newtwitterhandle';
    console.log('Scope changed!');
}, 3000);
```

* The caveat to all of this is that we have to buy wholesale into AngularJS either all in or all out and this has been criticized by a lot of programmers.
* Now we understand that Watchers and the Digest Loop binds the Model to the View. Here is a diagram:

![](/assets/Complete Model and View Diagram.png)

### Common Directives

* `ng-if` directive takes in a Javascript expression that either returns true or false. Here is an example:

```
<!--
    The 5 can also be written as a variable in the $scope object that
    we assign a value of 5. The ng-if directive tells the DOM what to
    do under certain circumstances.
-->
<div ng-if="handle.length !== 5">
</div>
```

* When the Digest Loop happens and notices that 'handle' has changed it will notice the expression of the `ng-if` attribute and if it is no longer true AngularJS tells the browser to remove the element from the DOM that has that `ng-if` attribute expression, and puts the comment `<!-- ngIf:handle.length !== characters -->`. If it is true then Angular tells the DOM to add the element back.
* `ng-show` directive shows the element if its attribute expression is true but doesn't remove the element from the DOM when the expression is `false`, instead it adds the class `ng-hide` which has the CSS rule: `display:none !important;`.
* `ng-hide` directive does the opposite of `ng-show`. It adds the ng-hide class if its attribute expression is true and remove the `ng-hide` class if its attribute expression is `false`.

* Here is an example of using the `ng-class` directive:

```
<!--
    The value to the left of the colon is the class name that we want to add and the
    value to the right of the colon is the expression that either returns true or 
    false. If it is true it will add in this case the 'alert-warning' class and if
    false it will remove this class.
-->
<div class="alert" ng-class="{'alert-warning': handle.length < characters}" 
    ng-show="handle.length !== characters">

    Must be 5 characters!

</div>
```

* We can do as many of these as we want, like this:

```
<!-- These are separated by comma. -->
ng-class="{ 'alert-warning': handle.length < characters, 'alert-danger': handle.length > characters}"
```

* If we want to display multiple elements with several different values we can use the** **`ng-repeat` directive like this:

**JS**

```js
//We have to hold values in an array with objects of the same key but different values.
$scope.rules = [
    { rulename: "a" },
    { rulename: "ab" },
    { rulename: "abc" }
];
```

**HTML**

```
<h3>Rules</h3>
<ul>
    <li ng-repeat="rule in rules">
        {{ rule.rulename }}
    </li>
</ul>
```

* Here is an example of using the `ng-click` directive:

**HTML**

```
<input type="button" value="Click me" ng-click="alertClick()" />
```

**JS**

```js
js
$scope.alertClick = function() {
    alert("Clicked.");
};                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               
```

* On some computers interpolationof AngularJS kicks in a little late and for a split second we see something for example `{{ name }}` pop on the screen before it is interpolated. We can use the `ng-cloak` directive to avoid this:

```
//ng-cloak hides the element until AngularJS has worked on it.
<div ng-cloak>{{ name }}</div>
```

* We can look for more directives like these at: [https://docs.angularjs.org/api](https://docs.angularjs.org/api) and look under **directive** heading in the list.

### The XMLHTTPRequest Object \(Javascript\)

* `XMLHttpRequest` object is native to the browser and was invented by Microsoft when they were building Outlook Web Access which still exists. The object can go out and make internet requests on its own as part of code, as opposed to the browser doing it via going to a particular URL in the browser and refreshing the page. It is now a standard in all browsers and in Javascript. Here is an example:

```js
var rulesrequest = new XMLHttpRequest();
/*
    This means that something has happened, the request has been made and
    the request is finished and now run this function.
*/
rulesrequest.onreadystatechange = function() {
    /*
        4 means it is ready to go and 200 means it has found something
        then we can do something.
    */
    if (rulesrequest.readyState == 4 && rulesrequest.status == 200) {
        /*
            This won't work as it is outside the Angular Context and so
            we have to wrap the if statement in 
            $scope.$apply(function(){});
        */
        $scope.rules = JSON.parse(rulesrequest.responseText);
    }
rulesrequest.open("GET", "http://localhost:54765/api", true);
/*
    Once this sends the request to the URL the onreadystatechange fires
    up and runs the function.
*/
rulesrequest.send();
```

* This all went out asynchronously, meaning simultaneously while we were doing other things and the code went out and fired off towards the URL address and got the data back from that URL. However, we can see that `XMLHttpRequest()` is complex to use and so Angular will make this easy for us.

### External Data and $http

* Angular helps us wrap the difficult code in Javascript for http requests and packages it into the `$http` service.
* Here is an example of using the $http service:

```js
//We put the URL as a string in the get parameter.
$http.get('/api')
    /*
        When it successfully gets some data back it then runs the function
        code. The result parameter is whatever data it got back, however
        we can name it whatever we want.
    */
    .success(function(result) {
        /*
            We don't need to wrap this up in the $apply method because
            $http is already within the Angular context.
        */
        $scope.rules = result;
    })
    /*
        If something goes wrong trying to get it then the data parameter
        is whatever data we might get from the request and status is the
        code indicating what the actual error code was so we can debug it.
        We can name data and status whatever we want.
        
    */
    .error(function(data, status) {
        console.log(data);
    });
```

* We don't always want to 'get' data but we sometimes want to 'post' data to the server and maybe save it to a database. Here is an example:

```js
$scope.newRule = '';
$scope.addRule = function() {
    $http.post('/api', { newRule: $scope.newRule } )
        .success(function(result) {
            //This updates
            $scope.rules = result;
            $scope.newRule = '';
        })
        .error(function(data, status) {
            console.log(data);
        });
};
```



