# Funcational Programming in Javascript

**Functional programming** (often abbreviated FP) is the process of building software by composing **pure functions**, avoiding **shared state,** **mutable data, **and **side-effects**. Functional programming is **declarative** rather than **imperative**, and application state flows through pure functions. 

Functional programming is a **programming paradigm**, meaning that it is a way of thinking about software construction based on some fundamental, defining principles

- Pure function :- A **pure function** is a function which:(https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)
  - Given the same inputs, always returns the same output, and
  - Has no side-effects
- Function composition
- Avoid shared state
- Avoid mutating state
- Avoid side effects

## [What is Function Composition?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0)

**Function composition** is the process of combining two or more functions to produce a new function. Composing functions together is like snapping together a series of pipes for our data to flow through.

## Shared State

**Shared state** is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes. A shared scope can include global scope or closure scopes. Often, in object oriented programming, objects are shared between scopes by adding properties to other objects.

race condition — a very common bug associated with shared state.

Another common problem associated with shared state is that changing the order in which functions are called can cause a cascade of failures because functions which act on shared state are timing dependent:

## Immutability

An **immutable** object is an object that can’t be modified after it’s created. Conversely, a **mutable** object is any object which can be modified after it’s created.

## Side Effects

A side effect is any application state change that is observable outside the called function other than its return value. Side effects include:

- Modifying any external variable or object property (e.g., a global variable, or a variable in the parent function scope chain)
- Logging to the console
- Writing to the screen
- Writing to a file
- Writing to the network
- Triggering any external process
- Calling any other functions with side-effects

# Reusability Through Higher Order Functions

FP pulls off its generic utility trickery using **higher order functions**.

JavaScript has **first class functions**, which allows us to treat functions as data — assign them to variables, pass them to other functions, return them from functions,

A **higher order function** is any function which takes a function as an argument, returns a function, or both. Higher order functions are often used to:

- Abstract or isolate actions, effects, or async flow control using callback functions, promises, monads, etc…
- Create utilities which can act on a wide variety of data types
- Partially apply a function to its arguments or create a curried function for the purpose of reuse or function composition
- Take a list of functions and return some composition of those input functions

## Containers, Functors, Lists, and Streams

## Declarative vs Imperative

Functional programming is a declarative paradigm, meaning that the program logic is expressed without explicitly describing the flow control.

**Imperative** programs spend lines of code describing the specific steps used to achieve the desired results — the **flow control: How** to do things.

**Declarative** programs abstract the flow control process, and instead spend lines of code describing the **data flow: What** to do. The *how* gets abstracted away.



## Currying

Informally, **currying** is the process of taking a function that accepts **n** arguments and turning it into **n** functions that each accepts a single argument.Therefore, we can define currying as the process of taking an **n-ary** function and turning it into **n** unary functions

**Partial application** :- Partial application, on the other hand, is when a function has been given some, but not all, of its arguments. Currying is often used to do partial application, but it's not the only way.

The JavaScript language has a built-in mechanism for doing partial application without currying. This is done using the [**function.prototype.bind**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) method. One idiosyncrasy of this method is that it requires you to pass in the value of **this** as the first argument. If you're not doing object-oriented programming, then you can effectively ignore **this** by passing in **null**.





Functional programming favors:

- Pure functions instead of shared state & side effects
- Immutability over mutable data
- Function composition over imperative flow control
- Lots of generic, reusable utilities that use higher order functions to act on many data types instead of methods that only operate on their colocated data
- Declarative rather than imperative code (what to do, rather than how to do it)
- Expressions over statements
- Containers & higher order functions over ad-hoc polymorphism