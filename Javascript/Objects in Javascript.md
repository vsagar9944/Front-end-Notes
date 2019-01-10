# Javascript Objects

JavaScript has one complex data type, the Object data type, and it has five simple data types: Number, String, Boolean, Undefined, and Null.Note that these simple (primitive) data types are immutable (cannot be changed), while objects are mutable (can be changed).

Everything in JavaScript acts like an object, with the only two exceptions being[`null`](http://bonsaiden.github.io/JavaScript-Garden/#core.undefined) and [`undefined`](http://bonsaiden.github.io/JavaScript-Garden/#core.undefined).

## **What is an Object**

An object is an unordered list of primitive data types (and sometimes reference data types) that is stored as a series of name-value pairs. Each item in the list is called a *property* (functions are called *methods*).

Property names can be a string or a number, but if the property name is a number, it has to be accessed with the bracket notation

## **Reference Data Type and Primitive Data Types**

One of the main differences between reference data type and primitive data types is reference data type’s value is stored as a reference, it is not stored directly on the variable, as a value, as the primitive data types are.

```javascript
// The primitive data type String is stored as a value
var person = "Kobe";  
var anotherPerson = person; // anotherPerson = the value of person
person = "Bryant"; // value of person changed
console.log(anotherPerson); // Kobe
console.log(person); // Bryant
```

Compare the primitive data saved-as-value demonstrated above with the save-as-reference for objects:

```javascript
var person = {name: "Kobe"};
var anotherPerson = person;
person.name = "Bryant";
console.log(anotherPerson.name); // Bryant
console.log(person.name); // Bryant
```



**Object Data Properties Have Attributes**
Each data property (object property that store data) has not only the name-value pair, but also 3 attributes (the three attributes are set to true by default):
**—  Configurable Attribute:** Specifies whether the property can be deleted or changed.
**— Enumerable: **Specifies whether the property can be returned in a for/in loop.
**— Writable: **Specifies whether the property can be changed.

A common misconception is that number literals cannot be used as objects. That is because a flaw in JavaScript's parser tries to parse the *dot notation* on a number as a floating point literal.

```javascript
2.toString(); // raises SyntaxError
```

There are a couple of workarounds that can be used to make number literals act as objects too.

```javascript
2..toString(); // the second point is correctly recognized
2 .toString(); // note the space left to the dot
(2).toString(); // 2 is evaluated first
```

## **Creating Objects**

These are the two common ways to create objects:

1. **Object Literals**

   ```javascript
   // This is an empty object initialized using the object literal notation
   var myBooks = {}; // This is an object with 4 items, again using object literal
   var mango = {
       color: "yellow",
       shape: "round",
       sweetness: 8,
       howSweetAmI: function() {
           console.log("Hmm Hmm Good");
       }
   }
   ```

2. **Object Constructor**

   ```javascript
   var mango =  new Object ();
   mango.color = "yellow";
   mango.shape= "round";
   mango.sweetness = 8;
   mango.howSweetAmI = function () {
   console.log("Hmm Hmm Good");
   }
   ```

3. **Constructor Pattern for Creating Objects**

   ```javascript
   function Fruit (theColor, theSweetness, theFruitName, theNativeToLand) {
       this.color = theColor;
       this.sweetness = theSweetness;
       this.fruitName = theFruitName;
       this.nativeToLand = theNativeToLand;
       this.showName = function () {
           console.log("This is a " + this.fruitName);
       }
       this.nativeTo = function () {
       this.nativeToLand.forEach(function (eachCountry)  {
          console.log("Grown in:" + eachCountry);
           });
       }
   }
    var mangoFruit = new Fruit ("Yellow", 8, "Mango", ["South America", "Central America", "West Africa"]);
   mangoFruit.showName(); // This is a Mango.
   mangoFruit.nativeTo();
   //Grown in:South America
   // Grown in:Central America
   // Grown in:West Africa
   var pineappleFruit = new Fruit ("Brown", 5, "Pineapple", ["United States"]);
   pineappleFruit.showName(); // This is a Pineapple.
   ```


 **Notes: **

   **— An inherited property is defined on the object’s prototype property.**

   **—An own property is defined directly on the object itself**

   — To access a property of an object, we use object.property,

   — To invoke a method of an object, we use object.method()

4. **Prototype Pattern for Creating Objects**

   ```javascript
   function Fruit() {}
   Fruit.prototype.color = "Yellow";
   Fruit.prototype.sweetness = 7;
   Fruit.prototype.fruitName = "Generic Fruit";
   Fruit.prototype.nativeToLand = "USA";
   Fruit.prototype.showName = function() {
       console.log("This is a " + this.fruitName);
   }
   Fruit.prototype.nativeTo = function() {
       console.log("Grown in:" + this.nativeToLand);
   }

   var mangoFruit = new Fruit();
   mangoFruit.showName(); //
   mangoFruit.nativeTo();
   ```

     

## **How to Access Properties on an Object**

The two primary ways of accessing properties of an object are with dot notation and bracket notation.

1. Dot Notation

   ```javascript
   // We have been using dot notation so far in the examples above, here is another example again:
   var book = {title: "Ways to Go", pages: 280, bookMark1:"Page 20"};// To access the properties of the book object with dot notation, you do this:
   console.log ( book.title); // Ways to Go
   console.log ( book.pages); // 280
   ```

2. Bracket Notation

   ```javascript
   // To access the properties of the book object with bracket notation, you do this:
   console.log ( book["title"]); //Ways to Go
   console.log ( book["pages"]); // 280
   //Or, in case you have the property name in a variable:
   var bookTitle = "title";
   console.log ( book[bookTitle]); // Ways to Go
   console.log (book["bookMark" + 1]); // Page 20
   ```

   Accessing a property on an object that does not exist will result in *undefined*.

   The notations work almost identically, with the only difference being that the square bracket notation allows for dynamic setting of properties and the use of property names that would otherwise lead to a syntax error.

## **Deleting Properties**

The only way to remove a property from an object is to use the `delete` operator; setting the property to `undefined` or `null` only removes the *value* associated with the property, but not the *key*.

```javascript
var obj = {
    bar: 1,
    foo: 2,
    baz: 3
};
obj.bar = undefined;
obj.foo = null;
delete obj.baz;

for(var i in obj) {
    if (obj.hasOwnProperty(i)) {
        console.log(i, '' + obj[i]);
    }
}
```

You cannot delete properties that were inherited, nor can you delete properties with their attributes set to configurable.

You must delete the inherited properties on the prototype object (where the properties were defined). Also, you cannot delete properties of the global object, which were declared with the var keyword.

The delete operator returns true if the delete was successful. And surprisingly, it also returns true if the property to delete was nonexistent or the property could not be deleted (such as non-configurable or not owned by the object).

## **Own and Inherited Properties**

Objects have inherited properties and own properties. The own properties are properties that were defined on the object, while the inherited properties were inherited from the object’s Prototype object.

To find out if a property exists on an object (either as an inherited or an own property), you use the in operator:

```javascript
// Create a new school object with a property name schoolName
var school = {schoolName:"MIT"};
// Prints true because schoolName is an own property on the school object
console.log("schoolName" in school);  // true
// Prints false because we did not define a schoolType property on the school object, and neither did the object inherit a schoolType property from its prototype object Object.prototype.
console.log("schoolType" in school);  // false
 
// Prints true because the school object inherited the toString method from Object.prototype.
console.log("toString" in school);  // true
```

**hasOwnProperty**

```javascript
// Create a new school object with a property name schoolName
var school = {schoolName:"MIT"};
// Prints true because schoolName is an own property on the school object
console.log(school.hasOwnProperty ("schoolName"));  // true
 
// Prints false because the school object inherited the toString method from Object.prototype, therefore toString is not an own property of the school object.
console.log(school.hasOwnProperty ("toString"));  // false 
```

**Accessing and Enumerating Properties on Objects**
To access the enumerable (own and inherited) properties on objects, you use the for/in loop or a general for loop.

**Accessing Inherited Properties**
Properties inherited from Object.prototype are not enumerable, so the for/in loop does not show them. However, inherited properties that are enumerable are revealed in the for/in loop iteration.
For example:

```javascript
//Use of the for/in loop to access the properties in the school object
for (var eachItem in school) {
    console.log(eachItem); // Prints schoolName, schoolAccredited, schoolLocation
} // Create a new HigherLearning function that the school object will inherit from.
/* SIDE NOTE: As Wilson (an astute reader) correctly pointed out in the comments below, the educationLevel property is not actually inherited by objects that use the HigherLearning constructor; instead, the educationLevel property is created as a new property on each object that uses the HigherLearning constructor. The reason the property is not inherited is because we use of the "this" keyword to define the property.
 */
function HigherLearning() {
    this.educationLevel = "University";
} // Implement inheritance with the HigherLearning constructor
var school = new HigherLearning();
school.schoolName = "MIT";
school.schoolAccredited = true;
school.schoolLocation = "Massachusetts"; //Use of the for/in loop to access the properties in the school object
for (var eachItem in school) {
    console.log(eachItem); // Prints educationLevel, schoolName, schoolAccredited, and schoolLocation
}
```



## **Serialize and Deserialize Objects**

serialize it (convert it to a string);   **JSON.stringify**

Deserialize your object (convert it to an object from a string)  **JSON.parse**

## JavaScript Prototype

There are two interrelated concepts with **prototype** in JavaScript:

1. First, every JavaScript function has a **prototype property** (this property is empty by default), and you attach properties and methods on this prototype property when you want to implement inheritance**.This prototype property is not enumerable; that is, it isn’t accessible in a for/in loop.**But Firefox and most versions of Safari and Chrome have a __proto__ “pseudo” property (an alternative syntax) that allows you to access an object’s prototype property. You will likely never use this __proto__ pseudo property, but you should know that it exists and it is simply a way to access an object’s prototype property in some browsers.

   The prototype property is used primarily for inheritance; you add methods and properties on a function’s prototype property to make those methods and properties available to instances of that function.

2. The second concept with prototype in JavaScript is the **prototype attribute**. An object’s prototype attribute points to the object’s “parent”—the object it inherited its properties from.

   Also note that the __proto__ “pseudo” property contains an object’s prototype object (the parent object it inherited its methods and properties from).

**Prototype Attribute of Objects Created with new Object () or Object Literal** Object.prototype is the prototype attribute (or the prototype object) of all objects created with new Object () or with {}. Object.prototype itself does not inherit any methods or properties from any other object.

```javascript
// The userAccount object inherits from Object and as such its prototype attribute is Object.prototype.
var userAccount = new Object ();
// This demonstrates the use of an object literal to create the userAccount object; the userAccount object inherits from Object; therefore, its prototype attribute is Object.prototype just as the userAccount object does above.
var userAccount = {name: “Mike”} 
```



**Prototype Attribute of Objects Created With a Constructor Function** Objects created with the new keyword and any constructor other than the Object () constructor, get their prototype from the constructor function

```javascript
function Account () {
}
var userAccount = new Account () // userAccount initialized with the Account () constructor and as such its prototype attribute (or prototype object) is Account.prototype.

```

### **Why is Prototype Important and When is it Used?**

1. **Prototype Property: Prototype-based Inheritance**
2. **Prototype Attribute: Accessing Properties on Objects** if you want to access a property of an object, the search for the property begins directly on the object. **prototype chain** And JavaScript uses this prototype chain to look for properties and methods of an object.If the property does not exist on any of the object’s prototype in its prototype chain, then the property does not exist and *undefined* is returned.

**NOTE:-** **Object.prototype Properties Inherited by all Objects**
All objects in JavaScript inherit properties and methods from Object.prototype. These inherited properties and methods are *constructor, hasOwnProperty (), isPrototypeOf (), propertyIsEnumerable (), toLocaleString (), toString (), and valueOf ()*. ECMAScript 5 also adds 4 accessor methods to Object.prototype.

### Property Lookup

When accessing the properties of an object, JavaScript will traverse the prototype chain **upwards** until it finds a property with the requested name.

If it reaches the top of the chain - namely `Object.prototype` - and still hasn't found the specified property, it will return the value [undefined](http://bonsaiden.github.io/JavaScript-Garden/#core.undefined) instead

### Performance

The lookup time for properties that are high up on the prototype chain can have a negative impact on performance, and this may be significant in code where performance is critical. Additionally, trying to access non-existent properties will always traverse the full prototype chain.

Also, when [iterating](http://bonsaiden.github.io/JavaScript-Garden/#object.forinloop) over the properties of an object **every** property that is on the prototype chain will be enumerated

The **only** good reason for extending a built-in prototype is to backport the features of newer JavaScript engines; for example, [`Array.forEach`](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/forEach).

### `hasOwnProperty` as a Property

JavaScript does not protect the property name `hasOwnProperty`; thus, if the possibility exists that an object might have a property with this name, it is necessary to use an *external* `hasOwnProperty` to get correct results.

```javascript
var foo = {
    hasOwnProperty: function() {
        return false;
    },
    bar: 'Here be dragons'
};
foo.hasOwnProperty('bar'); // always returns false
// Use another Object's hasOwnProperty and call it with 'this' set to foo
({}).hasOwnProperty.call(foo, 'bar'); // true
// It's also possible to use hasOwnProperty from the Object
// prototype for this purpose
Object.prototype.hasOwnProperty.call(foo, 'bar'); // true

```

Just like the `in` operator, the `for in` loop traverses the prototype chain when iterating over the properties of an object.





## What are mutable and immutable data structures?

A mutable object is an object whose state can be modified after it is created. An immutable object is an object whose state cannot be modified after it is created. Examples of native JavaScript values that are immutable are numbers and strings. Examples of native JavaScript values that are mutable include objects, arrays, functions, classes, sets, and maps.

1. Mutable Object

   ```javascript
   let a = {
       foo: 'bar'
   };

   let b = a;

   a.foo = 'test';

   console.log(b.foo); // test
   console.log(a === b) // true
   ```

   ​

2. Immutable Objects

   ```javascript
   let a = 'test';
   let b = a;
   a = a.substring(2);

   console.log(a) //st
   console.log(b) //test
   console.log(a === b) //false


   let a = 1;
   let b = a;
   a++;
   console.log(a) //2
   console.log(b) //1
   console.log(a === b) //false
   ```



What we see is that for mutable values, updating state applies across all *references* to that variable. So changing a value in one place, changes it for all references to that object. 

For the immutable data types, we have no way of changing the internal state of the data, so the reference always gets reassigned to a new object. The biggest implication of this is that for immutable data, equality is more reliable since we know that a value’s state won’t be changed out from under us.

Finally, its worth noting that it’s still possible to treat JavaScript objects as immutable. This can first be done through [Object.freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze), which shallowly renders a JavaScript object immutable. But it can also be done with programmer discipline. If we want to rely on object’s being immutable, it’s possible to enforce that all object updates are done through something like `Object.assign(a, {foo: 'bar'})` rather than `a.foo = 'bar'`, and all array updates are done through functions that generate new arrays like `Array.prototype.map`, `Array.prototype.filter`, or `Array.prototype.concat`, rather than mutating methods like `Array.prototype.push`, `Array.prototye.pop`, or `Array.prototype.sort`. This is less reliable without language level constraints, but has become popular in the React ecosystem for dealing with data for folks who don’t want to introduce abstractions like Immutable.js.