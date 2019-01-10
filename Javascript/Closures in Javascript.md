**What is a closure?**
A closure is an inner function that has access to the outer (enclosing) function’s variables—scope chain. The closure has three scope chains: it has access to its own scope (variables defined between its curly brackets), it has access to the outer function’s variables, and it has access to the global variables.

The inner function has access not only to the outer function’s variables, but also to the outer function’s parameters. Note that the inner function cannot call the outer function’s *arguments* object, however, even though it can call the outer function’s parameters directly.

You create a closure by adding a function inside another function.

```javascript
function showName(firstName,lastName){
   var nameIntro = 'Your Name is ';
  function makeFullName(){
    return nameIntro + firstName + lastName;
  }
  return makeFullName();
}
showName("Vidyasagar","mishra");
```

**A Classic jQuery Example of Closures:** 

```javascript
$(function() {
    var selections = [];
    $(".niners").click(function() { // this closure has access to the selections variable
        selections.push(this.prop("name")); // update the selections variable in the outer function's scope
    });
});
```

**Closures’ Rules and Side Effects**

1. **Closures have access to the outer function’s variable even after the outer function returns:**

2. **Closures store references to the outer function’s variables**;they do not store the actual value.

3. **Closures Gone Awry**  Because closures have access to the updated values of the outer function’s variables, they can also lead to bugs when the outer function’s variable changes with a for loop. 

   ```javascript
   // This example is explained in detail below (just after this code box).
   function celebrityIDCreator(theCelebrities) {
       var i;
       var uniqueID = 100;
       for (i = 0; i < theCelebrities.length; i++) {
           theCelebrities[i]["id"] = function() {
               return uniqueID + i;
           }
       }

       return theCelebrities;
   }
   var actionCelebs = [{
       name: "Stallone",
       id: 0
   }, {
       name: "Cruise",
       id: 0
   }, {
       name: "Willis",
       id: 0
   }];
   var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
   var stalloneID = createIdForActionCelebs[0];
   console.log(stalloneID.id()); // 103
   ```

   To fix this side effect (bug) in closures, you can use an **Immediately Invoked Function Expression** (IIFE), such as the following:

   ```javascript
   function celebrityIDCreator(theCelebrities) {
       var i;
       var uniqueID = 100;
       for (i = 0; i < theCelebrities.length; i++) {
           theCelebrities[i]["id"] = function(j) { // the j parametric variable is the i passed in on invocation of this IIFE
               return function() {
                   return uniqueID + j; // each iteration of the for loop passes the current value of i into this IIFE and it saves the correct value to the array
               }() // BY adding () at the end of this function, we are executing it immediately and returning just the value of uniqueID + j, instead of returning a function.
           }(i); // immediately invoke the function passing the i variable as a parameter
       }
       return theCelebrities;
   }
   var actionCelebs = [{
       name: "Stallone",
       id: 0
   }, {
       name: "Cruise",
       id: 0
   }, {
       name: "Willis",
       id: 0
   }];
   var createIdForActionCelebs = celebrityIDCreator(actionCelebs);
   var stalloneID = createIdForActionCelebs[0];
   console.log(stalloneID.id); // 100
   var cruiseID = createIdForActionCelebs[1];
   console.log(cruiseID.id); // 101
   ```





## **Variable Scope**

A variable’s scope is the context in which the variable exists. The scope specifies from where you can access a variable and whether you have access to the variable in that context.

1. **Local Variables (Function-level scope)**
2. **If You Don’t Declare Your Local Variables, Trouble is High**
3. **Local Variables Have Priority Over Global Variables in Functions**

**Global Variables**
All variables declared outside a function are in the global scope. In the browser, which is what we are concerned with as front-end developers, the global context or scope is the window object (or the entire HTML document).

If a variable is initialized (assigned a value) without first being declared with the var keyword, it is automatically added to the global context and it is thus a global variable:

**setTimeout Variables are Executed in the Global Scope** Note that all functions in setTimeout are executed in the global scope.

```javascript
 // The use of the "this" object inside the setTimeout function refers to the Window object, not to myObj
var highValue = 200;
var constantVal = 2;
var myObj = {
	highValue: 20,
	constantVal: 5,
	calculateIt: function () {
 setTimeout (function  () {
	console.log(this.constantVal * this.highValue);
}, 2000);
	}
}
// The "this" object in the setTimeout function used the global highValue and constantVal variables, because the reference to "this" in the setTimeout function refers to the global window object, not to the myObj object as we might expect.
myObj.calculateIt(); // 400
// This is an important point to remember.
```



**Do not Pollute the Global Scope**

**Variable Hoisting**
All variable declarations are hoisted (lifted and declared) to the top of the function, if defined in a function, or the top of the global context, if outside a function.

It is important to know that only variable declarations are hoisted to the top, not variable initialization or assignments (when the variable is assigned a value).

```javascript
function showName() {
    console.log("First Name: " + name);
    var name = "Ford";
    console.log("Last Name: " + name);
}
showName(); // First Name: undefined
// Last Name: Ford
// The reason undefined prints first is because the local variable name was hoisted to the top of the function
// Which means it is this local variable that get calls the first time.
// This is how the code is actually processed by the JavaScript engine:

function showName() {
    var name; // name is hoisted (note that is undefined at this point, since the assignment happens below)
    console.log("First Name: " + name); // First Name: undefined
    name = "Ford"; // name is assigned a value
    // now name is Ford
    console.log("Last Name: " + name); // Last Name: Ford
}
```

### A Word On Hoisting

You can find many resources online defining the term `hoisting` in JavaScript, explaining that variable and function declarations are *hoisted* to the top of their function scope. However, none explain in detail why this happens, and armed with your new knowledge about how the interpreter creates the `activation object`, it is easy to see why. Take the following code example:

```
(function() {

    console.log(typeof foo); // function pointer
    console.log(typeof bar); // undefined

    var foo = 'hello',
        bar = function() {
            return 'world';
        };

    function foo() {
        return 'hello';
    }

}());

```

The questions we can now answer are:

- **Why can we access foo before we have declared it?**

- - If we follow the `creation stage`, we know the variables have already been created before the `activation / code execution stage`. So as the function flow started executing, `foo` had already been defined in the `activation object`.

- **Foo is declared twice, why is foo shown to be** `function` **and not** `undefined` **or** `string`**?**

- - Even though `foo` is declared twice, we know from the `creation stage` that functions are created on the `activation object` before variables, and if the property name already exists on the `activation object`, we simply bypass the decleration.
  - Therefore, a reference to `function foo()` is first created on the `activation object`, and when the interpreter gets to `var foo`, we already see the property name `foo` exists so the code does nothing and proceeds.

- **Why is bar** `undefined`**?**

- - `bar` is actually a variable that has a function assignment, and we know the variables are created in the `creation stage` but they are initialized with the value of `undefined`.

**Function Declaration Overrides Variable Declaration When Hoisted**
Both function declaration and variable declarations are hoisted to the top of the containing scope. And function declaration takes precedence over variable declarations (but not over variable assignment). As is noted above, variable assignment is not hoisted, and neither is function assignment. As a reminder, this is a function assignment: var myFunction = function () {}.
Here is a basic example to demonstrate:

```javascript
// Both the variable and the function are named myName
var myName;
function myName() {
    console.log("Rich");
}// The function declaration overrides the variable name
console.log(typeof myName); // function


// But in this example, the variable assignment overrides the function declaration
var myName = "Richard"; // This is the variable assignment (initialization) that overrides the function declaration.
function myName() {
    console.log("Rich");
}
console.log(typeof myName); // string
```

It is important to note that function expressions, such as the example below, are not hoisted.

```javascript
var myName = function () {console.log ("Rich");} 
```

In strict mode, an error will occur if you assign a variable a value without first declaring the variable. Always declare your variables.



NOTE:- ince closures carry with them the containing function’s scope, they take up more memory than other functions. Overuse of closures can lead to excess memory consumption, so it’s recommended you use them only when absolutely necessary. Optimizing JavaScript engines, such as V8, make attempts to reclaim memory that is trapped because of closures, but it’s still recommended to be careful when using closures.

There is one notable side effect of this scope-chain confi guration. The closure always gets the last value of any variable from the containing function. Remember that the closure stores a reference to the entire variable object, not just to a particular variable