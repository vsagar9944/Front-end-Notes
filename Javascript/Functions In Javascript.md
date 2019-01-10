# Funtions

## What is a Function?

A **function** is a process which takes some input, called **arguments**, and produces some output called a **return value**. Functions may serve the following purposes:

- **Mapping:** Produce some output based on given inputs. A function **maps**input values to output values.
- **Procedures:** A function may be called to perform a sequence of steps. The sequence is known as a procedure, and programming in this style is known as **procedural programming**.
- **I/O:** Some functions exist to communicate with other parts of the system, such as the screen, storage, system logs, or network.

**Referentially transparent** :- A function is said to be referentially transparent if it, given the same input parameters, always produces the same output (return value). If one is looking for a raison d'être for pure functional programming, referential transparency is a good candidate. When reasoning with formulae in algebra, arithmetic, and logic, this property — also called substitutivity of equals for equals — is so fundamentally important that it is usually taken for granted...

#### Pure Functions

A **pure function** is a function which:

- Given the same input, will always return the same output.
- Produces no side effects.

A dead giveaway that a function is impure is if it makes sense to call it without using its return value. For pure functions, that’s a noop.

Pure functions have many beneficial properties, and form the foundation of **functional programming**. Pure functions are completely independent of outside state, and as such, they are immune to entire classes of bugs that have to do with shared mutable state. Their independent nature also makes them great candidates for parallel processing across many CPUs, and across entire distributed computing clusters, which makes them essential for many types of scientific and resource-intensive computing tasks.

**Pure Functions Produce No Side Effects**

A pure function produces no side effects, which means that it can’t alter any external state.

**Immutability**

JavaScript’s object arguments are references, which means that if a function were to mutate a property on an object or array parameter, that would mutate state that is accessible outside the function. Pure functions must not mutate external state.



There are two ways to defi ne a function: by function declaration and by function expression. The first, function declaration, has the following form:

```javascript
function functionName(arg0, arg1, arg2) {
	//function body
}
```

Firefox, Safari, Chrome, and Opera all feature a nonstandard name property on functions exposing the assigned name. This value is always equivalent to the identifi er that immediately follows the function keyword:

```javascript
functionName.name;
```

One of the key characteristics of function declarations is function declaration hoisting, whereby function declarations are read before the code executes. That means a function declaration may appear after code that calls it and still work:

The second way to create a function is by using a function expression. Function expressions have several forms. The most common is as follows:

```javascript
var functionName = function(arg0, arg1, arg2){
//function body
};
```

The created function is considered to be an anonymous function, because it has no identifi er after the function keyword. (Anonymous functions are also sometimes called lambda functions.) This means the name property is the empty string.

RECURSION
A recursive function typically is formed when a function calls itself by name,



## Closure

NOTE:- ince closures carry with them the containing function’s scope, they take up more memory than other functions. Overuse of closures can lead to excess memory consumption, so it’s recommended you use them only when absolutely necessary. Optimizing JavaScript engines, such as V8, make attempts to reclaim memory that is trapped because of closures, but it’s still recommended to be careful when using closures.



**The this Object**

The this object is bound at runtime based on the context in which a function is executed: when used inside global functions, this is equal to window in nonstrict mode and undefined in strict mode, whereas this is equal to the object when called as an object method. Anonymous functions are not bound to an object in this context, meaning the this object points to window unless executing in strict mode (where this is undefined ). Because of the way closures are written, however, this fact is not always obvious. Consider the following:

```javascript
var name = "The Window";
var object = {
    name: "My Object",
    getNameFunc: function() {
		console.log(this.name) //"My Object"
        return function() {
            return this.name;
        };
    }
};
alert(object.getNameFunc()());//"The Window" (in non-strict mode)
```

Remember that each function automatically gets two special variables as soon as the function is called: this and arguments . An inner function can never access these variables directly from an outer function. It is possible to allow a closure access to a different this object by storing it in another variable that the closure can access

```javascript
var name = "The Window";

var object = {
    name: "My Object",
    getNameFunc: function() {
        var that = this;
        return function() {
            return that.name;
        };
    }
};
alert(object.getNameFunc()()); //"My Object"
```

NOTE:- Both this and arguments behave in this way. If you want access to a containing scope’s arguments object, you’ll need to save a reference into another variable that the closure can access



**Memory Leaks**

**MIMICKING BLOCK SCOPE**

The basic syntax of an anonymous function used as a block scope (often called a private scope) is asfollows:
(function(){
​	//block code here
})();



This pattern limits the closure memory problem, because there is no reference to the anonymous function. Therefore the scope chain can be destroyed immediately after the function has completed.

**PRIVATE VARIABLES**

Any variable defi ned inside a function is considered private since it is inaccessible outside that function. This includes function arguments, local variables, and functions defi ned inside other functions.

A privileged method is a public method that has access to private variables and/or private functions. There are two ways to create privileged methods on objects. The fi rst is to do so inside a constructor, as in this example:

```javascript
function MyObject(){
//private variables and functions
var privateVariable = 10;
function privateFunction(){
return false;
}
//privileged methods
this.publicMethod = function (){
privateVariable++;
return privateFunction();
};
}
```





SUMMARY
Function expressions are useful tools in JavaScript programming. They allow truly dynamic programming where functions need not be named. These anonymous functions, also called lambda functions, are a powerful way to use JavaScript functions. The following is a summary of function expressions:
➤ Function expressions are different from function declarations. Function declarations require names, while function expressions do not. A function expression without a name is also called an anonymous function.
➤ With no defi nitive way to reference a function, recursive functions become more complicated.
➤ Recursive functions running in nonstrict mode may use arguments.callee to call themselves recursively instead of using the function name, which may change. Closures are created when functions are defi ned inside other functions, allowing the closure access to all of the variables inside of the containing function, as follows:
➤ Behind the scenes, the closure’s scope chain contains a variable object for itself, the containing function, and the global context.
➤ Typically a function’s scope and all of its variables are destroyed when the function has fi nished executing.
➤ When a closure is returned from that function, its scope remains in memory until the closure no longer exists.

Using closures, it’s possible to mimic block scoping in JavaScript, which doesn’t exist natively, as follows:
➤ A function can be created and called immediately, executing the code within it but never leaving a reference to the function.
➤ This results in all of the variables inside the function being destroyed unless they are specifi cally set to a variable in the containing scope. Closures can also be used to create private variables in objects, as follows:
➤ Even though JavaScript doesn’t have a formal concept of private object properties, closures can be used to implement public methods that have access to variables defi ned within the containing scope.
➤ Public methods that have access to private variables are called privileged methods.
➤ Privileged methods can be implemented on custom types using the constructor or prototype patterns and on singletons by using the module or module-augmentation patterns.Function expressions and closures are extremely powerful in JavaScript and can be used to
accomplish many things. Keep in mind that closures maintain extra scopes in memory, so overusing them may result in increased memory consumption.