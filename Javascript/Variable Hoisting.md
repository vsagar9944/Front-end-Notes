# Variable Hoisting

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