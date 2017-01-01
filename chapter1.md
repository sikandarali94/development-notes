# Execution Contexts and Lexical Environments



### Syntax Parsers, Execution Contexts and Lexical Environments

* **Syntax Parser**: A program that reads your code and determines what it does and if its grammar is valid e.g. compilers and interpreters.
* **Lexical Environment**: Where something sits physically in the code you write. 'Lexical' means 'having to do with words or grammar'. A lexical environment exists in programming languages in which **where** you write something is important.
* **Execution Context**: A wrapper to help manage the code that is running. There are lots of lexical environments. Which one is currently running is managed via execution contexts. It can contain things beyond what we've written in our code. 

### Name-Value Pairs and Objects

* **Name-Value Pair**: A name which maps to a unique value. The name may be defined more than once, but only can have one value in any given context. That value may be more name-value pairs.
* **Object**: A collection of name-value pairs. The simplest definition when talking about Javascript. 

### The Global Environment and the Global Object

* In Javascript one of the base execution contexts is the Global execution context. This execution context creates a Global Object and the variable 'this.

![](/assets/Global Execution Diagram Context.png)

* In the browser the Global Object is the window object.
* Code or variables that aren't inside a function are Global, meaning they are accessible anywhere within the Javascript code.

---

The main credit for these notes goes to Anthony Alicea. He has a great set of courses which can be found at, [https://www.udemy.com/user/anthonypalicea/](https://www.udemy.com/user/anthonypalicea/)

