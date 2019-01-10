Being loosely typed, a variable is literally just a name for a particular value at a particular time. Because there are no rules defi ning the type of data that a variable must hold, a variable’s value and data type can change during the lifetime of a script.

## PRIMITIVE AND REFERENCE VALUES

ECMAScript variables may contain two different types of data: primitive values and reference values. 

**Primitive values** are simple atomic pieces of data, **(Undefined,Null, Boolean, Number, and String)**

**reference values** are objects that may be made up of multiple values.

These variables are said to be **accessed by value**, because you are manipulating the actual value stored in the variable.

Reference values are objects stored in memory. When you manipulate an object, you’re really working on a reference to that object rather than the actual object itself. For this reason, such values are said to be accessed by reference.

### Dynamic Properties

When you work with reference values, you can add, change, or delete properties and methods at any time

```javascript
//Reference values
var person = new Object();
person.name = "Nicholas";
alert(person.name); //"Nicholas"

//Primitive values can’t have properties added to them even though attempting to do so won’t cause an error. Here’s an example:
var name = “Nicholas”;
name.age = 27;
alert(name.age); //undefined
```

### Copying Values

When a primitive value is assigned from one variable to another, the value stored on the variable object is created and copied into the location for the new variable.

When a reference value is assigned from one variable to another, the value stored on the variable object is also copied into the location for the new variable. The difference is that this value is actually a pointer to an object stored on the heap. Once the operation is complete, two variables point to exactly the same object, so changes to one are refl ected on the other, as in the following example:

```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name);
//"Nicholas"
```

### Argument Passing

All function arguments in ECMAScript are passed by value If the value is primitive, then it acts just like a primitive variable copy, and if the value is a reference, it acts just like a reference variable copy. This is often a point of confusion for developers, because variables are accessed both by value and by reference, but arguments are passed only by value.

**-->** To prove that objects are passed by value, consider the following modifi ed code:

```javascript
function setName(obj) {
	obj.name = "Nicholas";
	obj = new Object();
	obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name);
//"Nicholas"
```

When obj is overwritten inside the function, it becomes a pointer to a local object. That local object is destroyed as soon as the function fi nishes executing.

### Determining Type

More specifi cally, it’s the best way to determine if a variable is a string, number, Boolean, or undefined . If the value is an object or null , then typeof returns “object”

The instanceof operator returns true if the variable is an instance of the given reference type.All reference values, by defi nition, are instances of Object , so the instanceof operator always returns true when used with a reference value and the Object constructor. Similarly, if instanceof is used with a primitive value, it will always return false , because primitives aren’t objects.

he typeof operator also returns “function” when used on a function. When used on a regular expression in Safari (through version 5) and Chrome (through version 7), typeof returns “function” because of an implementation detail.



## EXECUTION CONTEXT AND SCOPE

The execution context of a variable or function defines what other data it has access to, as well as how it should behave. Each execution context has an associated variable object upon which all of its defi ned variables and functions exist. This object is not accessible by code but is used behind the scenes to handle data.

In web browsers, the global context is said to be that of the window object, so all global variables and functions are created as properties and methods on the window object.

When an execution context has executed all of its code, it is destroyed, taking with it all of the variables and functions defined within it (the global context isn’t destroyed until the application exits, such as when a web page is closed or a web browser is shut down).

Each function call has its own execution context. Whenever code execution flows into a function, the function’s context is pushed onto a context stack. After the function has finished executing the stack is popped, returning control to the previously executing context.

When code is executed in a context, a scope chain of variable objects is created. The purpose of the scope chain is to provide ordered access to all variables and functions that an execution context has access to. The front of the scope chain is always the variable object of the context whose code is executing.

Identifiers are resolved by navigating the scope chain in search of the identifier name. The search always begins at the front of the chain and proceeds to the back until the identifi er is found

An inner context can access everything from all outer contexts through the scope chain, but the outer contexts cannot access anything within an inner context.The connection between the contexts is linear and ordered. Each context can search up the scope chain for variables and functions, but no context can search down the scope chain into another execution context.

### Scope Chain Augmentation

There are two times when this occurs, specifi cally when execution enters either of the following:
➤ The catch block in a try-catch statement
➤ A with statement

### No Block-Level Scopes

```javascript
if (true) {
var color = "blue";
}
alert(color);//"blue"
```

**Variable Declaration**

When a variable is declared using var , it is automatically added to the most immediate context available.

If a variable is initialized without first being declared, it gets added to the global context automatically



**Identifier Lookup**

The search starts at the front of the scope chain, looking for an identifi er with the given name. If it fi nds that identifi er name in the local context, then the search stops and the variable is set; if the search doesn’t fi nd the variable name, it continues along the scope chain (note that objects in the scope chain also have a prototype chain, so searching may include each object’s prototype chain). This process continues until the search reaches the global context’s variable object. If the identifi er isn’t found there, it hasn’t been declared.

Variable lookup doesn’t come without a price. It’s faster to access local variables than global variables because there’s no search up the scope chain. JavaScript engines are getting better at optimizing identifi er lookup, though, so this difference may end up negligible in the future.

## GARBAGE COLLECTION

JavaScript is a garbage-collected language, meaning that the execution environment is responsible for managing the memory required during code execution.

The strategy for identifying the unused variables may differ on an implementation basis, though two strategies have traditionally been used in browsers.

1. **Mark-and-Sweep** :- When a variable comes into context, such as when a variable is declared inside a function, it is fl agged as being in context. Variables that are in context, logically, should never have their memory freed, because they may be used as long as execution continues in that context. When a variable goes out of context, it is also fl agged as being out of context.

   When the garbage collector runs, it marks all variables stored in memory (once again, in any number of ways). It then clears its mark off of variables that are in context and variables that are referenced by in-context variables. The variables that are marked after that are considered ready for deletion, because they can’t be reached by any in-context variables. The garbage collector then does a memory sweep, destroying each of the marked values and reclaiming the memory associated with them.

2. **Reference Counting** :- The idea is that every value keeps track of how many references are made to it. When a variable is declared and a reference value is assigned, the reference count is one. If another variable is then assigned to the same value, the reference count is incremented.Likewise, if a variable with a reference to that value is overwritten with another value, then the reference count is decremented. When the reference count of a value reaches zero, there is no way to reach that value and it is safe to reclaim the associated memory. The garbage collector frees the memory for values with a reference count of zero the next time it runs

   Reference counting was initially used by Netscape Navigator 3.0 and was immediately met with a serious issue: circular references

**Performance**

Internet Explorer was infamous for its performance issues related to how often the garbage collector ran — it ran based on the number of allocations, specifi cally 256 variable allocations, 4,096 object/array literals and array slots, or 64kb of strings. If any of these thresholds were reached, the garbage collector would run.

t’s possible, though not recommended, to trigger the garbage-collection process in some browsers. In Internet Explorer, the window.CollectGarbage() method causes garbage collection to occur immediately. In Opera 7 and higher, calling window.opera.collect() initiates the garbage-collection process.

**Managing Memory**

The amount of memory available for use in web browsers is typically much less than is available for desktop applications. This is more of a security feature than anything else, ensuring that a web page running JavaScript can’t crash the operating system by using up all the system memory. The memory limits affect not only variable allocation but also the call stack and the number of statements that can be executed in a single thread.

Keeping the amount of used memory to a minimum leads to better page performance. The best way to optimize memory usage is to ensure that you’re keeping around only data that is necessary for the execution of your code. When data is no longer necessary, it’s best to set the value to null , freeing up the reference — this is called dereferencing the value.This advice applies mostly to global values
and properties of global objects. Local variables are dereferenced automatically when they go out of context

Keep in mind that dereferencing a value doesn’t automatically reclaim the memory associated with it. The point of dereferencing is to make sure the value is out of context and will be reclaimed the next time garbage collection occurs.

---------

SUMMARY
Two types of values can be stored in JavaScript variables: primitive values and reference values. Primitive values have one of the fi ve primitive data types: Undefi ned, Null, Boolean, Number, and String. Primitive and reference values have the following characteristics:
➤ Primitive values are of a fi xed size and so are stored in memory on the stack.
➤ Copying primitive values from one variable to another creates a second copy of the value.
➤ Reference values are objects and are stored in memory on the heap.
➤ A variable containing a reference value actually contains just a pointer to the object, not the object itself.
➤ Copying a reference value to another variable copies just the pointer, so both variables end up referencing the same object.
➤ The typeof operator determines a value’s primitive type, whereas the instanceof operator is used to determine the reference type of a value.

All variables, primitive and reference, exist within an execution context (also called a scope) that determines the lifetime of the variable and which parts of the code can access it. Execution context can be summarized as follows:
➤ Execution contexts exist globally (called the global context) and within functions.
➤ Each time a new execution context is entered, it creates a scope chain to search for variables and functions.
➤ Contexts that are local to a function have access not only to variables in that scope but also to variables in any containing contexts and the global context.
➤ The global context has access only to variables and functions in the global context and cannot directly access any data inside local contexts.
➤ The execution context of variables helps to determine when memory will be freed. JavaScript is a garbage-collected programming environment where the developer need not be concerned with memory allocation or reclamation. JavaScript’s garbage-collection routine can be summarized as follows:
➤ Values that go out of scope will automatically be marked for reclamation and will be deleted during the garbage-collection process.
➤ The predominant garbage-collection algorithm is called mark-and-sweep, which marks values that aren’t currently being used and then goes back to reclaim that memory.

➤ Another algorithm is reference counting, which keeps track of how many references there are to a particular value. JavaScript engines no longer use this algorithm, but it still affects Internet Explorer because of nonnative JavaScript objects (such as DOM elements) being accessed in JavaScript

➤ Reference counting causes problems when circular references exist in code.

➤ Dereferencing variables helps not only with circular references but also with garbage collec- tion in general. To aid in memory reclamation, global objects, properties on global objects, and circular references should all be dereferenced when no longer needed.

