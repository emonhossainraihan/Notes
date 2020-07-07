## Scope

scope refers to the **visibility** of variables, functions, and objects in some particular part of your code during runtime

**The Principle of Least Access**: This principle is also applied to programming language designs, 
where it is called scope in most programming languages including JavaScript

Variables are statically scoped in JavaScript. Fundamentally, scope is function-based while context is object-based. 
In other words, scope pertains to the variable access of a function when it is invoked 
and is unique to each invocation. Context is always the value of the this keyword which 
is a reference to the object that “owns” the currently executing code.
- A context of a function is the value of this for that function
- Scope on the other hand defines the way JavaScript resolves a variable at run time. 

In the JavaScript language there are two types of scopes:

- Global Scope
- Local Scope

Global scope lives as long as your application lives. Local Scope lives as long as your functions are called and executed. Block statements like 
**if** and **switch** conditions or **for** and **while** loops, unlike functions, don't create a new scope.

> The exception is the 'this' keyword which behaves like a dynamically scoped variable

**ECMAScript 6:** Contrary to the `var` keyword, the `let` and `const` keywords support the declaration of local scope inside block statements.

## Context

Is most often determined by how a function is invoked with the value of **this** keyword

Scope refers to the visibility of variables and context refers to the value of this in the same scope.

## Execution Context

To remove all confusions and from what we studied above, the word context in Execution Context refers to scope and not context. 
This is a weird naming convention but because of the JavaScipt specification, we are tied to it. 

JavaScript is a single-threaded language so it can only execute a single task at a time. The rest of the tasks are queued in the Execution Context. 
As I told you earlier that when the JavaScript interpreter starts to execute your code, the context (scope) is by default set to be global. 
This global context is appended to your execution context which is actually the first context that starts the execution context. 

Once the browser is done with the code in that context, that context will then be popped off from the execution context and the state of the current context in 
the execution context will be transferred to the parent context. The execution context has two phases of creation and code execution.

> Changing Context with .call(), .apply() and .bind()

## Creation Phase

The first phase that is the creation phase is present when a function is called but its code is not yet executed. 
Three main things that happen in the creation phase are:

> Setup memory space for variables and functions **"Hoisting"**

- Creation of the Variable (Activation) Object
- Creation of the Scope Chain **(Reference to outer 💡Lexical environment)**
- Setting of the value of context `this`

The execution context can be represented as an abstract object like this:
```js
executionContextObject = {
    'scopeChain': {}, // contains its own variableObject and other variableObject of the parent execution contexts
    'variableObject': {}, // contains function arguments, inner variable and function declarations
    'this': valueOfThis
}
```

## Lexical Scope

Lexical Scope means that in a nested group of functions, the inner functions have access to the variables and other resources of their parent scope.

## Closures

> A closure can not only access the variables defined in its outer function but also the arguments of the outer function. 





