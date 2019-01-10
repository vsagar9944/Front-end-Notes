# this keywork in Javascript

## JavaScript’s *this* Keyword Basics

when a function executes, it gets the *this* property—a variable with the **value of the object that invokes the function where this is used**.

Note that when we use **strict mode**, *this* holds the value of *undefined* in global functions and in anonymous functions that are not bound to any object.

*this* is used inside a function (let’s say function A) and it contains the value of the **object** that invokes function A. We need *this* to access methods and properties of the object that invokes function A, especially since we don’t always know the name of the invoking object, and sometimes there is no name to use to refer to the invoking object. Indeed, *this* is really just a shortcut reference for the “antecedent object”—the invoking object.

```
 var person = {
    firstName   :"Penelope",
    lastName    :"Barrymore",
    // Since the "this" keyword is used inside the showFullName method below, and the showFullName method is defined on the person object,
    // "this" will have the value of the person object because the person object will invoke showFullName ()
    showFullName:function () {
    console.log (this.firstName + " " + this.lastName);
    }

    }

    person.showFullName (); // Penelope Barrymore

// A very common piece of jQuery code

    $ ("button").click (function (event) {
    // $(this) will have the value of the button ($("button")) object
// because the button object invokes the click () method
    console.log ($ (this).prop ("name"));
    });
```





## When *this* is most misunderstood and becomes tricky

The *this* keyword is most misunderstood when we borrow a method that uses *this*, when we assign a method that uses *this* to a variable, when a function that uses *this* is passed as a callback function, and when *this* is used inside a closure—an inner function.



Here are scenarios when the *this* keyword becomes tricky.

## Fix *this* when used in a method passed as a callback

```
 // We have a simple object with a clickHandler method that we want to use when a button on the page is clicked
    var user = {
    data:[
    {name:"T. Woods", age:37},
    {name:"P. Mickelson", age:43}
    ],
    clickHandler:function (event) {
    var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1

    // This line is printing a random person's name and age from the data array
    console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
    }
    }

    // The button is wrapped inside a jQuery $ wrapper, so it is now a jQuery object
    // And the output will be undefined because there is no data property on the button object
    $ ("button").click (user.clickHandler); // Cannot read property '0' of undefined
```



At this point, it should be apparent that when the context changes—when we execute a method on some other object than where the object was originally defined, the *this* keyword no longer refers to the original object where “this” was originally defined, but it now refers to the object that invokes the **method** where *this* was defined.

**Solution to fix this when a method is passed as a callback function:**
Since we really want this.data to refer to the data property on the *user* object, we can use the Bind (), Apply (), or Call () method to specifically set the value of *this*.

```
Instead of this line:

 $ ("button").click (user.clickHandler);
We have to bind the clickHandler method to the user object like this:

    $("button").click (user.clickHandler.bind (user)); // P. Mickelson 43
```

## Fix *this* inside closure

Another instance when *this* is misunderstood is when we use an inner method (a closure). It is important to take note that closures cannot access the outer function’s *this* variable by using the *this* keyword because the *this* variable is accessible only by the function itself, not by inner functions. 

Solution 

```
  // A common practice amongst JavaScript users is to use this code
    var that = this;
```

## Fix *this* when method is assigned to a variable

```
 // This data variable is a global variable
    var data = [
    {name:"Samantha", age:12},
    {name:"Alexis", age:14}
    ];

    var user = {
    // this data variable is a property on the user object
    data    :[
    {name:"T. Woods", age:37},
    {name:"P. Mickelson", age:43}
    ],
    showData:function (event) {
    var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1

    // This line is adding a random person from the data array to the text field
    console.log (this.data[randomNum].name + " " + this.data[randomNum].age);
    }

    }

    // Assign the user.showData to a variable
    var showUserData = user.showData;

    // When we execute the showUserData function, the values printed to the console are from the global data array, not from the data array in the user object
    //
    showUserData (); // Samantha 12 (from the global data array)
```

**Solution for maintaining this when method is assigned to a variable:**
We can fix this problem by specifically setting the *this* value with the bind method:

```
 // Bind the showData method to the user object
    var showUserData = user.showData.bind (user);

    // Now we get the value from the user object, because the <em>this</em> keyword is bound to the user object
    showUserData (); // P. Mickelson 43
```

## Fix *this* when borrowing methods

```
 // We have two objects. One of them has a method called avg () that the other doesn't have
    // So we will borrow the (avg()) method
    var gameController = {
    scores  :[20, 34, 55, 46, 77],
    avgScore:null,
    players :[
    {name:"Tommy", playerID:987, age:23},
    {name:"Pau", playerID:87, age:33}
    ]
    }

    var appController = {
    scores  :[900, 845, 809, 950],
    avgScore:null,
    avg     :function () {

    var sumOfScores = this.scores.reduce (function (prev, cur, index, array) {
    return prev + cur;
    });

    this.avgScore = sumOfScores / this.scores.length;
    }
    }

    //If we run the code below,
    // the gameController.avgScore property will be set to the average score from the appController object "scores" array
   
    // Don't run this code, for it is just for illustration; we want the appController.avgScore to remain null
    gameController.avgScore = appController.avg();
```



**Solution for fixing this when borrowing methods:**
To fix the issue and make sure that *this* inside the appController.avg () method refers to gameController, we can use the *apply ()* method thus:

```
// Note that we are using the apply () method, so the 2nd argument has to be an array—the arguments to pass to the appController.avg () method.
    appController.avg.apply (gameController, gameController.scores);

    // The avgScore property was successfully set on the gameController object, even though we borrowed the avg () method from the appController object
    console.log (gameController.avgScore); // 46.4

    // appController.avgScore is still null; it was not updated, only gameController.avgScore was updated
    console.log (appController.avgScore); // null
```

The first parameter in the apply method always sets the value of “this” explicitly.



-------------------------------------------------------------------------

# JavaScript’s Apply, Call, and Bind Methods are Essential for JavaScript Professionals

http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/

Functions are objects in JavaScript And as objects, functions have methods, including the powerful *Apply*, *Call*, and *Bind* methods. On the one hand, *Apply* and *Call* are nearly identical and are frequently used in JavaScript for borrowing methods and for setting the *this* value explicitly. 

On the other hand, we use Bind for setting the *this* value in methods and for currying functions.



## JavaScript’s Bind Method

We use the *Bind ()* method primarily to call a function with the *this* value set explicitly. It other words, *bind ()* allows us to easily set which specific object will be bound to *this* when a function or method is invoked.

The need for *bind* usually occurs when we use the *this* keyword in a method and we call that method from a receiver object; in such cases, sometimes *this* is not bound to the object that we expect it to be bound to, resulting in errors in our applications

## Bind () Allows us to Borrow Methods

## JavaScript’s Bind Allows Us to Curry a Function 





## JavaScript’s Apply and Call Methods

The **Apply** and **Call** methods are two of the most often used Function methods in JavaScript, and for good reason: they allow us to borrow functions and set the *this* value in function invocation. In addition, the *apply* function in particular allows us to execute a function with an array of parameters, such that each parameter is passed to the function individually when the function executes—great for **variadic** functions; a *variadic* function takes varying number of arguments, not a set number of arguments as most functions do.

## Set the *this* value with Apply or Call

Just as in the *bind ()* example, we can also set the *this* value when invoking functions by using the Apply or Call methods. The first parameter in the call and apply methods set the this value to the object that the function is invoked upon.

The apply and call methods are almost identical when setting the *this* value except that you pass the function parameters to apply () as an array, while you have to list the parameters individually to pass them to the call () method.

**Use Call or Apply To Set this in Callback Functions**

## Borrowing Functions with Apply and Call (A Must Know)

**Borrowing Array Methods**

**Borrowing String Methods with Apply and Call**

**Borrow Other Methods and Functions**



## Use Apply () to Execute Variable-Arity Functions

This technique is especially used for creating **variable-arity**, also known as **variadic** functions.These are functions that accept any number of arguments instead of a fixed number of arguments. The *arity* of a function specifies the number of arguments the function was defined to accept.

- We can pass an array with of arguments to a function and, by virtue of using the apply () method, the function will execute the
  items in the array as if we called the function like this:

- ```
      createAccount (arrayOfItems[0], arrayOfItems[1], arrayOfItems[2], arrayOfItems[3]);
  ```


- The *Math.max()* method is an example of a common variable-arity function in JavaScript:

- ```
      // We can pass any number of arguments to the Math.max () method    console.log (Math.max (23, 11, 34, 56)); // 56
       var allNumbers = [23, 11, 34, 56];
      // Using the apply () method, we can pass the array of numbers:
      console.log (Math.max.apply (null, allNumbers)); // 56
  ```