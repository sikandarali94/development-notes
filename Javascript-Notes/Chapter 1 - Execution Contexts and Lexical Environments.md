# Execution Contexts and Lexical Environments

### Syntax Parsers, Execution Contexts and Lexical Environments \(Conceptual\)

* **Syntax Parser**: A program that reads your code and determines what it does and if the code's grammar is valid e.g. compilers and interpreters.
* **Lexical Environment**: Where something sits physically in the code you write. 'Lexical' means 'having to do with words or grammar'. A lexical environment exists in programming languages in which **where** you write something is important.
* **Execution Context**: A wrapper to help manage the code that is running. There are lots of lexical environments. Which one is currently running is managed via execution contexts. It can contain things beyond what we've written in our code. 

### Name-Value Pairs and Objects \(Conceptual\)

* **Name-Value Pair**: A name which maps to a unique value. The name may be defined more than once, but only can have one value in any given context. That value may be more name-value pairs.
* **Object**: A collection of name-value pairs. The simplest definition when talking about Javascript. 

### The Global Environment and the Global Object

* In Javascript one of the base execution contexts is the **Global execution context**. This **execution context** creates a Global Object and the variable 'this.

![](/assets/GlobalExecutionDiagramContext.png)

* The 'this' variable refers to the Global Object.
* In the browser the Global Object is the window object.
* Code or variables that aren't inside a function are Global, meaning they are accessible anywhere within the Javascript code.

### The Execution Context \(Creation and 'Hoisting'\)

* The execution context is created in two phases. The first phase is called the creation phase; it sets up the Global Object, the 'this' variable, the Outer Environment, and setup memory space for variables and functions \(hoisting\).

![](/assets/CreationPhaseDiagram.png)

* A function in its **entirety** is placed into the memory space, including the code inside it during the creation phase. During the creation phase variable names are placed into the memory space; it creates a placeholder for the variable called **undefined**. It is only during the execution phase that variable assignments are placed into memory.

### Javascript and 'undefined' \(Conceptual\)

* **Hoisting**: Variables Setup \(and set equal to 'undefined'\) and Functions Setup.
* When Javascript sets a variable to undefined it doesn't mean that the variable doesn't exist. It simply means that the variable is not set to a value.
* It is important to let Javascript set a variable to undefined rather than writing: `a = undefined; //Legal Javascript.` It helps when debugging the code.

### Code Execution

* In the execution phase -- after variable names and functions as a whole are placed into memory during the creation phase -- Javascript runs our code line by line.

### Single Threaded, Synchronous Execution \(Conceptual\)

* **Single Threaded**: One command is being executed at a time. In a browser many things are being executed at the same time, but in our perspective Javascript is being executed in a single-threaded manner.
* **Synchronous**: One at a time. Within Javascript it is not only one at a time, but in order.

### Function Invocation and The Execution Stack

* **Invocation**: It just means running a function in Javascript, this is done by using a parentheses, \(\).
* When you invoke a function in Javascript a new Execution Context is created and put on top of the stack.

![](/assets/ExecutionStackDiagram.png)

* When `b` is finished it will **pop off** the stack, then `a` will run and finish then pop off the stack, and then finally the Global will run and therefore the execution stack will be complete. This is all during the execution phase.

### Functions, Context, and Variable Environments

* **Variable Environment**: Where the variables live and how they relate to each other in memory.

```js
function b() {
    var myVar; //The variable environment for this myVar is b().
}

function a() {
    var myVar = 2; //The variable environment for this myVar is a().
}

var myVar = 1; //The variable environment for this myVar is Global.
a();
```

* Every execution context has its own variable environment.

### The Scope Chain

* Every execution context has a reference to its outer environment.
* Javascript cares about the lexical environment when it comes to the outer reference that every execution context gets. That is why when we ask for a variable while running a line of code within any particular execution context, if it can't find that variable, it will look at the outer reference and go look for variables there. Therefore in the line of code below it will log `myVar` as `1` because lexically `b()` is sitting within the Global execution context.

```js
//This function b() sits lexically in Global. We say its lexical environment is Global.
function b() {
    console.log(myVar); //It will log myVar as 1.
}

function a() {
    var myVar = 2;
    b();
}

var myVar = 1;
a()
```

* **Scope**: Where can I access a variable?

* **Scope Chain**: The chain where Javascript searches for a variable within the outer environment references of the execution context where it sits lexically, and the outer environment of that execution context where that sits lexically, until it reaches Global where its outer environment reference is empty.

```js
function a() {
    function b() {
        /*
            Logs out 2 to the console because the lexical environment of b() here 
            is a(), and myVar is declared as 2 in there. 
        */
        console.log(myVar);
    }
    var myVar = 2;
    b();
}
a();
/*
    b() gives Reference Error because the b() function's lexical environment is not
    in Global but in a().
*/
b();
```

### Scope, ES6 and let

* **Scope**: Where a variable is available in your code, and if its truly the same variable, or a new copy.

* In ES6 they are introducing a new way to declare variable, and that is called **let**.

```js
if (a > b) {
    /*
        If one tries to use the variable c before the let statement is executed the computer 
        won't allow us to access it even though it is in memory. This is called "block scoping".
    */
    let c = true;
}
```

### Asynchronous Callbacks

* **Asynchronous**: More than one at a time.
* Javascript is synchronous, however the Javascript engine does not exist by itself. The engine exists within the browser. Within the browser the rendering engine, the HTTP request, and so forth are all running at the same time. While the Javascript engine is synchronous the browser is asynchronous.

![](/assets/TheBrowserDiagram.jpg)

* When we make an asynchronous callback what the Javascript engine is doing is telling the browser to run another program within itself while the Javascript engine keeps running. Effectively the browser is running things asynchronously while the Javascript engine remains synchronous.
* Apart from the execution stack Javascript also has an event queue.

![](/assets/ExecutionStackAndEventQueueDiagram.jpg)

* When the execution stack is empty, meaning it has run all the execution contexts, it then looks at the event queue to see if any event has been triggered. If it has then it executes those functions that are associated after the event has been triggered. The browser is the one that is putting the events asynchronously on the event queue, therefore, when the execution stack is complete Javascript then checks the event queue and runs the associated functions synchronously and so forth.
* **Event Loop**: The continuous check that Javascript does for something to be added to the event queue after the execution stack has been completed.

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

