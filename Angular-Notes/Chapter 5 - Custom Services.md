# Custom Services

### Singletons \(Javascript\)

* **Singleton**: The one and only copy of an object. It is a pattern in object-oriented programming.
* Here is a visual diagram:

![](/assets/Singleton Comparison Diagram.png)

* Regarding Person, with the Person object we have created instantiations of that Person object named Laura, Steve and Tony. Regarding Singleton, with a singleton when we say we have this object there is **only** one. We can never ever have any copies of it. When we ask for the object we are never getting a copy we are  only getting one.
* AngularJS services are implemented as singletons. Here is an example proving it:

```js
myApp.controller('mainController', ['$scope', '$log', function($scope, $log) {
    $scope.name = 'Main';
    $log.main = 'Property from main';
    $log.log($log);
}]);

myApp.controller('secondController', ['$scope', '$log', function($scope, $log) {
    /*
        When $log.main line is executed and $log.second is executed and $log
        is logged to the console we see that the main property and second
        property exist in the same $log object. This is because the $log
        service and subsequently every service is a singleton.
    */
    $log.second = 'Property from second';
    $log.log($log);
}]); 
```

* The fact that services are singletons saves a lot of memory because it avoids copying objects multiple times onto different areas in memory.
* `$scope` is an exception to the rule that every service in Angular is a singleton. If we did the previous example with `$scope` then we get the main variable existing on one `$scope` object and the second variable existing on a separate `$scope` object.
* `$scope` in AngularJS is a child `$scope`. Certain circumstances, like passing it to a controller, cause a new child `$scope` to be created. They all inherit from the same object, a root `$scope` that is attached to our app, because it has all this functionality. We get a new child $scope for each controller. Therefore `$scope` is not a singleton.
* When we create our own service we are creating a singleton, a piece of functionality or data that can be shared across pages in our single page application.

### Creating a Service

* Here is an example of creating a service:

```js
var myApp = angular.module('myApp', []);
myApp.service('nameService', function() {
    var self = this;
    this.name = 'Sikandar Ali';
    this.namelength = function() {
        return self.name.length;
    };
});
```

* The way we access the service is how we access any service in Angular; we use dependency injection. Here is an example leading on from the above code:

```js
myApp.controller('mainController', ['$scope','$log','nameService', function($scope,$log,nameService) {
    $log.log(nameService.name); //This will log 'Sikandar Ali'.
    $log.log(nameService.namelength()); //This will log 12.
}]);
```

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

