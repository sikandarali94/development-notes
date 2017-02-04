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





