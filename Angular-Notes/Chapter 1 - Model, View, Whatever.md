# Model, View, Whatever

### The Problem\(s\) AngularJS is Trying to Solve

* AngularJS helps us efficiently manipulate the DOM because the problem is that when we have many DOM elements we have to manage it becomes really hairy having to repeat and set rules for each of them. AngularJS helps us with that.

### Model, View, Whatever...

![](/assets/Model, View, Whatever Diagram.png)

* Regarding Model it represents our data. Regarding View it is the thing that the people see. In this diagram Model is bound to View and also View is bound to Model.
* Whatever happens to the Model automatically affects the View. Whatever happens in View automatically affects the Model. Whatever binds them are called **Controllers**, **View Models**, **Whatever...\[MV\*\]**.

### Custom Attributes \(HTML\)

* We can create any attribute we want in HTML. Here is an example:

```
<h1 reply="Hello Back!">Hello World!</h1>
```

* `reply` attribute is therefore known as a custom attribute. It exists in the browser's memory, it is still there in the DOM.
* HTML5 has an official version of this where we add `data-` in front. Here is an example:

```
<!-- This is considered valid HTML5 -->
<h1 data-reply="Hello Back!">Hello World!</h1>
```

* Angular doesn't follow HTML5's pattern. It puts `ng-` at the front. Here is an example:

```
<h1 ng-reply="Hello Back!">Hello World!</h1>
```

* If we really want to be HTML5 compliant we can do something like this:

```
<!-- Angular does still recognize this as a custom attribute it can use. -->
<h1 data-ng-reply="Hello Back!">Hello World!</h1>
```

### The Global Namespace \(Javascript\)

* In this example:

```js
var person = 'Sikandar'; //We've stuck person in the Global namespace.
console.log(person);
```

* In another Javascript file if we used a person variable and included it with the above code in the same HTML file then it will clash, and the variable will be the latest definition one used, meaning say at the first file we had `var person = 'Steve';` and in the second we had `var person = 'Sikandar';` it will use `'Sikandar'` because that has overwritten `'Steve'` because it is at the bottom, the latest definition.
* To avoid the above issue we will need to use something called a namespace. Here is an example:

```js
var stevesApp = {}; //This object acts as a namespace.
stevesApp.person = 'Steve';
```

* The namespace places the values as properties and avoids clashes if someone used a variable, for example in the above code, called `person`.

### Modules, Apps, and Controllers

* Here is an example of declaring an Angular module:

```js
/*
    - It is good convention to name the variable the same as the module name.
    - 'myApp' is the first parameter where we give the name of the app.
    - In the square brackets is where we write the dependencies.
*/
var myApp = angular.module('myApp', []);
```

* We are then going to tell Angular where in the View 'myApp' lives because we need to bind it together. In the HTML file we give the attribute to the View like this:

```
<!--
    Here is where we give the custom attribute to indicate to Angular what
    the View is. Angular looks for the attribute called ng-app and
    everything inside the tag that has this attribute is connected to the
    myApp variable in the Global namespace. Angular looks for the value
    of ng-app and matches it to the module name that we created called
    'myApp'. It looks for the string, not the variable name.
-->
<html lang="en-us" ng-app="myApp">
    <!-- Some Code In Between-->
</html>
```

* After this we are going to declare a controller. Here is an example:

```js
/*
    - 'mainController' is the first parameter where we give the name of the controller.
    - controller takes in a function as the second parameter.
*/
myApp.controller('mainController', function() {

});
```

* We can then define a sub-View within our main View. Here is an example:

```
<html lang="en-us" ng-app="myApp">
    <!-- Some Code Above -->
    <!--
        Angular sees the custom attribute of ng-controller in the DOM and looks for
        the controller with the name 'mainController' within the app named 'myApp'
        given by the custom attribute of ng-app. Any code within the function of
        the controller will be associated with controlling any DOM elements or HTML
        written within the element that has ng-controller. Angular is going to keep
        the Model (which is myApp.controller()) and the View (which is <div
        ng-controller="mainController"> </div>) bound for us without us doing any
        special work.
    -->
    <div ng-controller = "mainController">

    </div>
    <!-- Some Code Below -->
</html> 
```

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

