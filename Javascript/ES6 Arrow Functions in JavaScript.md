# ES6 Arrow Functions: Fat and Concise Syntax in JavaScript



## Links

1. https://www.sitepoint.com/es6-arrow-functions-new-fat-concise-syntax-javascript/
2. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions

## What Are Arrow Functions?

Arrow functions – also called “fat arrow” functions, from CoffeeScript ([a transcompiled language](http://blogs.msdn.com/b/cdnstudents/archive/2013/09/17/visual-studio-tips-for-javascript-coders-try-coffeescript.aspx?WT.mc_id=16547-DEV-sitepoint-article83)) — are a more concise syntax for writing function expressions. They utilize a new token, `=>`, that looks like a fat arrow. Arrow functions are anonymous and change the way `this` binds in functions.

Arrow functions make our code more concise, and simplify function scoping and the [this](https://msdn.microsoft.com/en-us/library/w062xezx(v=vs.94).aspx?WT.mc_id=16547-DEV-sitepoint-article83) keyword. They are one-line mini functions which work much like [Lambdas in other languages like C#](https://msdn.microsoft.com/en-us/library/bb397687.aspx?WT.mc_id=16547-DEV-sitepoint-article83) or [Python](http://www.diveintopython.net/power_of_introspection/lambda_functions.html). (See also [lambdas in JavaScript](http://stackoverflow.com/questions/7190439/is-there-a-c-like-lambda-syntax-in-javascript)). By using arrow functions, we avoid having to type the `function` keyword, `return` keyword (it’s implicit in arrow functions), and curly brackets.

An **arrow function expression** has a shorter syntax than a [function expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function) and does not have its own `this`, [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments), [super](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super), or [new.target](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target). These function expressions are best suited for non-method functions, and they cannot be used as constructors.

### No separate `this`

Until arrow functions, every new function defined its own `this` value (a new object in the case of a constructor, undefined in [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) function calls, the base object if the function is called as an "object method", etc.). This proved to be less than ideal with an object-oriented style of programming.

## Using Arrow Functions

There are a variety of syntaxes available in arrow functions, of which [MDN has a thorough list](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions). We’ll cover the common ones here to get you started. Let’s compare how ES5 code with function expressions can now be written in ES6 using arrow functions.

### Basic Syntax with Multiple Parameters ([from MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions))

```
// (param1, param2, paramN) => expression

// ES5
var multiplyES5 = function(x, y) {
  return x * y;
};

// ES6
const multiplyES6 = (x, y) => { return x * y };

```

[Code Example at JSBin](https://sitepointeditors.jsbin.com/zigakis/edit?js,console).

The arrow function example above allows a developer to accomplish the same result with fewer lines of code and approximately half the typing.

Curly brackets aren’t required if only one expression is present. The preceding example could also be written as:

```
const multiplyES6 = (x, y) => x * y;

```

### Basic Syntax with One Parameter

Parentheses are optional when only one parameter is present

```
//ES5
var phraseSplitterEs5 = function phraseSplitter(phrase) {
  return phrase.split(' ');
};

//ES6
const phraseSplitterEs6 = phrase => phrase.split(" ");

console.log(phraseSplitterEs6("ES6 Awesomeness"));  // ["ES6", "Awesomeness"]

```

[Code Example at JSBin](https://sitepointeditors.jsbin.com/vitusiw/edit?js,console).

### No Parameters

Parentheses are required when no parameters are present.

```
//ES5
var docLogEs5 = function docLog() {
    console.log(document);
};

//ES6
var docLogEs6 = () => { console.log(document); };
docLogEs6(); // #document... <html> ….

```

[Code Example at JSBin](https://sitepointeditors.jsbin.com/ratoget/edit?js,console).

### Object Literal Syntax

Arrow functions, like function expressions, can be used to return an object literal expression. The only caveat is that the body needs to be wrapped in parentheses, in order to distinguish between a block and an object (both of which use curly brackets).

```
//ES5
var setNameIdsEs5 = function setNameIds(id, name) {
  return {
    id: id,
    name: name
  };
};

// ES6
var setNameIdsEs6 = (id, name) => ({ id: id, name: name });

console.log(setNameIdsEs6 (4, "Kyle"));   // Object {id: 4, name: "Kyle"}

```

```
// Rest parameters and default parameters are supported
(param1, param2, ...rest) => { statements } 
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { 
statements } 
```

## Arrow functions versus normal functions

1. it always has a bound `this`.
2. it can’t be used as a constructor
3. no property `prototype`
4. as arrow functions are an ECMAScript.next-only construct, they can rely on new-style argument handling ([parameter default values](http://wiki.ecmascript.org/doku.php?id=harmony:parameter_default_values), [rest parameters](http://wiki.ecmascript.org/doku.php?id=harmony:rest_parameters), etc.) and don’t support the special variable `arguments`.

## Use Cases for Arrow Functions

Now that we’ve covered the basic syntaxes, let’s get into how arrow functions are used.

One common use case for arrow functions is array manipulation and the like. It’s common that you’ll need to map or reduce an array. Take this simple array of objects:

```
const smartPhones = [
  { name:'iphone', price:649 },
  { name:'Galaxy S6', price:576 },
  { name:'Galaxy Note 5', price:489 }
];

```

We could create an array of objects with just the names or prices by doing this in ES5:

```
// ES5
var prices = smartPhones.map(function(smartPhone) {
  return smartPhone.price;
});

console.log(prices); // [649, 576, 489]

```

An arrow function is more concise and easier to read:

```
// ES6
const prices = smartPhones.map(smartPhone => smartPhone.price);
console.log(prices); // [649, 576, 489]

```

[Code Example at JSBin](https://sitepointeditors.jsbin.com/jocivor/edit?js,console).

Here’s another example using the [array filter method](https://msdn.microsoft.com/en-us/library/ff679973(v=vs.94).aspx):

```
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];

// ES5
var divisibleByThrreeES5 = array.filter(function (v){
  return v % 3 === 0;
});

// ES6
const divisibleByThrreeES6 = array.filter(v => v % 3 === 0);

console.log(divisibleByThrreeES6); // [3, 6, 9, 12, 15]

```

[Code Example at JSBin](https://sitepointeditors.jsbin.com/mujezav/edit?js,console).

### Promises and Callbacks

Code that makes use of asynchronous callbacks or promises often contains a great deal of `function` and `return` keywords. When using promises, these function expressions will be used for chaining. Here’s a simple example of [chaining promises from the MSDN docs](https://msdn.microsoft.com/en-us/library/windows/apps/hh700334.aspx?WT.mc_id=16547-DEV-sitepoint-article83):

```
// ES5
aAsync().then(function() {
  returnbAsync();
}).then(function() {
  returncAsync();
}).done(function() {
  finish();
});

```

This code is simplified, and arguably easier to read using arrow functions:

```
// ES6
aAsync().then(() => bAsync()).then(() => cAsync()).done(() => finish);

```

Arrow functions should similarly simplify callback-laden NodeJS code.

**What’s the meaning of this?!**

The other benefit of using arrow functions with promises/callbacks is that it reduces the confusion surrounding the `this` keyword. In code with multiple nested functions, it can be difficult to keep track of and remember to bind the correct `this` context. In ES5, you can use workarounds like the `.bind` method ([which is slow](https://jsperf.com/function-bind-performance/5)) or creating a closure using `var self = this;`.

Because arrow functions allow you to retain the scope of the caller inside the function, you don’t need to create `self = this` closures or use bind.

Developer [Jack Franklin](https://twitter.com/jack_franklin) provides an excellent [practical example of using the arrow function lexical `this` to simplify a promise](http://javascriptplayground.com/blog/2014/04/real-life-es6-arrow-fn/):

Without Arrow functions, the promise code needs to be written something like this:

```
// ES5
API.prototype.get = function(resource) {
  var self = this;
  return new Promise(function(resolve, reject) {
    http.get(self.uri + resource, function(data) {
      resolve(data);
    });
  });
};

```

Using an arrow function, the same result can be achieved more concisely and clearly:

```
// ES6
API.prototype.get = function(resource) {
  return new Promise((resolve, reject) => {
    http.get(this.uri + resource, function(data) {
      resolve(data);
    });
  });
};

```

You can use function expressions if you need a dynamic `this` and arrow functions for a lexical `this`.

## Gotchas and Pitfalls of Arrow Functions

The new arrow functions bring a helpful function syntax to ECMAScript, but as with any new feature, they come with their own pitfalls and gotchas.

Kyle Simpson, a JavaScript developer and writer, felt there were enough pitfalls with Arrow Functions to [warrant this flow chart when deciding to use them](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/fig1.png). He argues there are too many confusing rules/syntaxes with arrow functions. Others have suggested that using arrow functions saves typing but ultimately makes code more difficult to read. All those `function` and `return` statements might make it easier to read multiple nested functions or just function expressions in general.

Developer opinions vary on just about everything, including arrow functions. For the sake of brevity, here are a couple things you need to watch out for when using arrow functions.

### More about *this*

As was mentioned previously, the `this` keyword works differently in arrow functions. The methods [call(), apply(), and bind()](http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/) will not change the value of `this` in arrow functions. (In fact, the value of `this` inside a function simply can’t be changed; it will be the same value as when the function was called.) If you need to bind to a different value, you’ll need to use a function expression.

#### Relation with strict mode

Given that `this` comes from the surrounding lexical context, [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) rules with regard to `this` are ignored.

```javascript
function Person() {
  this.age = 0;
  var closure = "123"
  setInterval(function growUp() {
    this.age++;
    console.log(closure)
  }, 1000);
}

var p = new Person();

function PersonX() {
  'use strict'
  this.age = 0;
  var closure = "123"
  setInterval(()=>{
    this.age++;
    console.log(closure)
  }, 1000);
}

var px = new PersonX();
```

#### Invoked through call or apply

Since arrow functions do not have their own `this`, the methods `call()` or `apply()` can only pass in parameters. `thisArg` is ignored.

```javascript
var adder = {
  base: 1,

  add: function(a) {
    var f = v => v + this.base;
    return f(a);
  },

  addThruCall: function(a) {
    var f = v => v + this.base;
    var b = {
      base: 2
    };

    return f.call(b, a);
  }
};

console.log(adder.add(1));         // This would log to 2
console.log(adder.addThruCall(1)); // This would log to 2 still
```



### Constructors

Arrow functions can’t be used as [constructors](https://msdn.microsoft.com/en-us/library/c1hcx253(v=vs.94).aspx?WT.mc_id=16547-DEV-sitepoint-article83) as other functions can. Don’t use them to create similar objects as you would with other functions. If you attempt to use `new`with an arrow function, it will throw an error. Arrow functions, like [built-in functions](https://msdn.microsoft.com/en-us/library/c6hac83s(v=vs.94)?WT.mc_id=16547-DEV-sitepoint-article83)(aka methods), don’t have a prototype property or other internal methods. Because constructors are generally used to create class-like objects in JavaScript, you should use the new [ES6 classes](https://blogs.msdn.microsoft.com/ie/2014/12/15/classes-in-javascript-exploring-the-implementation-in-chakra/?WT.mc_id=16547-DEV-sitepoint-article83) instead.

Arrow functions cannot be used as constructors and will throw an error when used with `new`.

### Use of `prototype` property

Arrow functions do not have a `prototype` property.

### Use of the `yield` keyword

The `yield` keyword may not be used in an arrow function's body (except when permitted within functions further nested within it). As a consequence, arrow functions cannot be used as generators.

### Generators

Arrow functions are designed to be lightweight and can’t be used as [generators](https://msdn.microsoft.com/en-us/library/dn858237(v=vs.94).aspx?WT.mc_id=16547-DEV-sitepoint-article83). Using the `yield` keyword in ES6 will throw an error. Use [ES6 generators](https://davidwalsh.name/es6-generators) instead.

### Arguments object

Arrow functions don’t have the local variable `arguments` as do other functions. The arguments object is an array-like object that allows developers to dynamically discover and access a function’s arguments. This is helpful because JavaScript functions can take an unlimited number of arguments. Arrow functions do not have this object.

## How Much Use Is There for Arrow Functions?

Arrow functions have been [called one of the quickest wins](http://javascriptplayground.com/blog/2014/04/real-life-es6-arrow-fn/) with ES6. Developer [Lars Schöning](http://stackoverflow.com/users/578454/lyschoening) lays out how his team decided [where to use arrow functions](http://stackoverflow.com/questions/22939130/when-should-i-use-arrow-functions-in-ecmascript-6):

- Use `function` in the global scope and for `Object.prototype` properties.
- Use `class` for object constructors.
- Use `=>` everywhere else.

Arrow functions, like [let and const](https://www.sitepoint.com/preparing-ecmascript-6-let-const/), will likely become the default functions unless function expressions or declarations are necessary. To get a sense for how much arrow functions can be used, [Kevin Smith](https://twitter.com/zenparsing), counted [function expressions in various popular libraries/frameworks](https://docs.google.com/spreadsheets/d/1G-zoOMJm3iu_lQF54YRd5BvLGPs8TTwiw7hpaEAw-s8/edit) and found that roughly [55% of function expressions](http://www.2ality.com/2012/04/arrow-functions.html)would be candidates for arrow functions.



## Function body

Arrow functions can have either a "concise body" or the usual "block body".

In a concise body, only an expression is specified, which becomes the explicit return value. In a block body, you must use an explicit `return` statement.

## Line breaks

An arrow function cannot contain a line break between its parameters and its arrow.

## Parsing order

Although the arrow in an arrow function is not an operator, arrow functions have special parsing rules that interact differently with [operator precedence](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence) compared to regular functions.

```javascript
let callback;

callback = callback || function() {}; // ok

callback = callback || () => {};      
// SyntaxError: invalid arrow-function arguments

callback = callback || (() => {});    // ok
```

## Method definitions

1. Method definitions in class declarations
2. Method definitions in object literals