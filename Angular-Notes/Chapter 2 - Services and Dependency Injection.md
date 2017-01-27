# Services and Dependency Injection

### Dependency Injection \(Javascript\)

* **Dependency Injection**: Giving a function an object. Rather than creating an object inside a function, we pass it to the function.
* Here is an example:

```js
var Person = function(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

/*
    logPerson() is dependent on siky. If something were to change about
    Sikandar we would have to do it in logPerson(). That is bad, we
    prefer not to do that it makes for really complicated or difficult
    to deal with code.
*/
function logPerson() {
    var siky = new Person('Sikandar', 'Ali');
    console.log(siky);
}

logPerson();
```

* To make the code less difficult above we are going to use dependency injection. Here is what we do:

```js
var Person = function(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
}

function logPerson(person) {
    console.log(person);
}

/*
    Instead we create the object siky outside of logPerson() and pass
    it into logPerson() as a parameter. This is known as Dependency
    Injection. Even though this is simple it is important because 
    now  logPerson() is not dependent on how siky is created. We can
    get siky from a database or create siky with the new keyword or
    whatever. The logPerson() function doesn't care, just give me the
    person. Here we are injecting the dependency. 
*/
var siky = new Person('Sikandar', 'Ali');
logPerson(john);
```

* AngularJS uses dependency injection when it comes to its controllers and some other instances. Its a very simple concept but again enforces a strong, stable architecture, for our web applications. The way Angular does dependency injection is very interesting.

### The Scope Service

* Scope is an object from the Scope Service and is a very important concept that Angular uses to bind Model and View together. It involves dependency injection. Here is an example:

```js
var myApp = angular.module('myApp', []);

/*
    Angular defines $scope object even though in this code we might
    have suspected $scope to be empty. However, Angular has defined
    $scope. All AngularJS services start with a '$' sign and $scope
    is a service. It helps us to recognize that $scope is a 
    service. 
*/
myApp.controller('mainController', function($scope) {
    console.log($scope);
});
```

* We can add properties or methods to the $scope object inside the function parameter of `myApp.controller()` method. Here is an example:

```js
var myApp = angular.module('myApp', []);
myApp.controller('mainController', function($scope) {
    $scope.name = 'Sikandar Ali';
    $scope.occupation = 'Student';
    $scope.getname = function() {
        return 'Sikandar Ali';
    }
    console.log($scope);
});
```

* $scope then becomes that middle piece, that piece between the View and the controller. We have this data, the controller says "I've tied what's inside my function parameter to the View." The data in the scope object is the data that will go back and forth between what is in the function parameter of the controller and what appears in the View.

### Functions and Strings\(Javascript\)

* In Javascript we can get a function as a string. Here is an example:

```js
var searchPeople = function(firstName, lastName, height, age, occupation) {
    return 'Sikandar Ali';
}
/*
    What this will log is the searchPeople() function as a string
    exactly how the coder typed it out.
*/
console.log(searchPeople);
```

* With this we could parse or do something fancy to break apart a function represented as a string and figure out what sort of parameters the function accepts how many parameters it accepts and what are the name for the parameters we can then make a decision based in what information we glean from the function represented as a string. This is exactly what Angular does.

### How Does Angular Do Dependency Injection?

* If we pass a function to a special method within the Angular object that is created by AngularJS we get an array with each value in the array being the name of parameters of the function. Here is an example:

```js
var searchPeople = function(firstName, lastName, height, age, occupation) {
    return 'Sikandar Ali';
}

/*
    We will never need to use a method like this, but
    nevertheless this will log:
    ["firstName", "lastName", "height", "age", "occupation"]
*/
console.log(angular.injector().annotate(searchPeople));
```

* That is how Angular knows to dependency inject the `$scope` object from its services because it knows the parameters being passed to the controller's parameter function by passing the parameter of the controller's parameter function and recognizing `$scope` and it provides the `$scope` object and dependency injects it into the controller's parameter function.
* This is extremely powerful. This means we need to write a parameter correctly that Angular recognizes and Angular automatically knows to create that parameter, as in the object `$scope` because we write it as a parameter.

### Getting Other Services

* Angular has created numerous services like `$scope`. Some include are: `$animate`, `$http` \(important for getting data\), `$document` and many many more. They all use the method of Dependency Injection and parsing the function to recognize the parameters given as discussed in the previous topic.
* We can also find a list of these services at: [https://docs.angularjs.org/api](https://docs.angularjs.org/api) and under service they are listed on the list at the left.
* We can ask for multiple services. Here is an example:

```js
myApp.controller('mainController', function($scope, $log) {
    console.log($scope);
    console.log($log);
});
```

* Here is an example of using the log service:

```js
myApp.controller('mainController', function($scope, $log) {
    $log.log("Hello");
    $log.info("Some info");
    $log.warn("Warning");
    $log.debug("Debug these errors.");
    $log.error("This was an error!!!");
});
```

* This makes console logging safer and expands its capabilities.
* We can download extra services, other than the standard that comes with Angular, from [https://code.angularjs.org/](https://code.angularjs.org/). We choose the Angular version and extra service files appear, for example: angular-animate.js. We reference it in our HTML by local directory or CDN and we get access to the extra services. We should make sure to put it after the script tag that references the standard Angular framework file.
* We don't get access to these new services immediately. We have to tell Angular that we want this new module, which contains the extra services. Here is an example of telling Angular that we want to use the services within the angular-messages.js file:

```js
/*
    As we noted before, when we declare the Angular module we put
    in the square brackets the dependencies. These dependencies
    are the modules that we tell Angular that are dependent by
    the app because it needs the service. In this example 
    'ngMessages' tells Angular that 'myApp' is dependent upon
    the services included in the angular-messages.js file.
*/
var myApp = angular.module('myApp', ['ngMessages']);
```

* The services within angular-messages.js file are very helpful for when we want to do form validation.
* Say for example we write dependency for an Angular script file within `angular.module()`, but we did not reference it in our HTML then we get this error: Uncaught Error: \[$injector:modulerr\]....
* If we reference an Angular script file, which includes extra services, but we forget to write the dependency within `angular.module()`, and we write the service that is included within the Angular script file we will get an error. Here is an example:

```js
/*
    We are missing here 'ngResource' because we referenced the
    file angular-resource.js in our HTML.
*/
var myApp = angular.module('myApp', ['ngMessages']);
/*
    Because we did not write 'ngResource' in angular.module()
    Angular sees that we are trying to inject a Angular
    service because it starts with '$' but doesn't have it
    because it did not include the services in 
    angular-resource.js within 'myApp'.
*/
myApp.controller('mainController', function($resource) {
    console.log($resource);
});
```

* We can find popular Angular modules at: [https://www.ngmodules.org](https://www.ngmodules.org).

### Arrays and Functions \(Javascript\)



