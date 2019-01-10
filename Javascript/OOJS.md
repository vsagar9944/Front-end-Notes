# Object Oriented Programming in Javascript

Object-oriented (OO) languages typically are identifi ed through their use of classes to create multiple objects that have the same properties and methods.

Object Oriented Programming (OOP) refers to using self-contained pieces of code(**Objects**) to develop applications.We call these self-contained pieces of code **objects**, better known as *Classes* in most OOP programming languages and *Functions* in JavaScript. 

**Object :- "unordered collection of properties each of which contains a primitive value, object, or function."**

Strictly speaking, this means that an object is an array of values in no particular order. Each property or method is identifi ed by a name that is mapped to a value. For this reason (and others yet to be discussed), it helps to think of ECMAScript objects as hash tables: nothing more than a grouping of name-value pairs where the value may be data or a function.

**Each object is created based on a reference type**, either one of the native types discussed in the previous chapter, or a developer-defi ned type.

## Encapsulation and Inheritance Overview

 **Objects** can be thought of as the main actors in an application, or simply the main "things" or building blocks that do all the work. 

**Encapsulation** refers to enclosing all the functionalities of an object within that object so that the object’s internal workings (its methods and properties) are hidden from the rest of the application. This allows us to abstract or localize specific set of functionalities on objects.

**Inheritance** refers to an object being able to inherit methods and properties from a parent object (a Class in other OOP languages, or a Function in JavaScript).

 An **instance** is an implementation of a Function. 

```javascript
// Tree is a constructor function because we will use new keyword to invoke it.
function Tree (typeOfTree) {} 

// bananaTree is an instance of Tree.
var bananaTree = new Tree ("banana");
```

The two important principles with OOP in JavaScript are Object Creation patterns (**Encapsulation**) and Code Reuse patterns (**Inheritance**).

**Why Encapsulation?**

 whenever you want to create objects with similar functionalities (to use the same methods and properties), you encapsulate the main functionalities in a Function and you use that Function’s constructor to create the objects. This is the essence of encapsulation. 

## Types of Properties

ECMA-262 fi fth edition describes characteristics of properties through the use of internal-only attributes. These attributes are defi ned by the specifi cation for implementation in JavaScript engines, and as such, these attributes are not directly accessible in JavaScript. To indicate that an attribute is internal, surround the attribute name with two pairs of square brackets, such as [[Enumerable]] .

There are two types of properties: data properties and accessor properties.

1. Data Properties

   Data properties contain a single location for a data value. Values are read from and written to this location.

   Data properties have four attributes describing their behavior:

      	1. [[Configurable]] — Indicates if the property may be redefi ned by removing the property via delete , changing the property’s 	attributes, or changing the property into an accessor property. By default, this is true for all properties defi ned directly on an object
      	2. [[Enumerable]] — Indicates if the property will be returned in a for-in loop.
      	3. [[Writable]] — Indicates if the property’s value can be changed. By default, this is true for all properties defi ned directly on an object
        4. [[Value]] — Contains the actual data value for the property.

   When a property is explicitly added to an object as in the previous examples, [[Configurable]] , [[Enumerable]] , and [[Writable]] are all set to true while the [[Value]] attribute is set to the assigned value. For example:

   ```javascript
   var person = {
   	name: "Nicholas"
   };
   ```

   To change any of the default property attributes, you must use the ECMAScript 5 Object .defineProperty() method. This method accepts three arguments: the object on which the property should be added or modifi ed, the name of the property, and a descriptor object. The properties on the descriptor object match the attribute names: configurable , enumerable , writable , and value . You can set one or all of these values to change the corresponding attribute values. For example:

   ```javascript
   var person = {};
   Object.defineProperty(person, "name", {
       writable: false,
       value: "Nicholas"
   });
   alert(person.name); //"Nicholas"
   person.name = "Greg";
   alert(person.name); //"Nicholas"
   ```

   When you are using Object.defineProperty() , the values for configurable , enumerable , and writable default to false unless otherwise specified.

2. **Accessor Properties**

   Accessor properties do not contain a data value. Instead, they contain a combination of a getter function and a setter function (though both are not necessary).Accessor properties have four attributes:

   1. [[Configurable]] :-  Same as data properties
   2. [[Enumerable]] :- Same as data Properties
   3. [[Get]] — The function to call when the property is read from. The default value is **undefined** .
   4. [[Set]] — The function to call when the property is written to. The default value is **undefined** .

   It is not possible to defi ne an accessor property explicitly; you must use Object.defineProperty() .

   Here’s a simple example

   ```javascript
   var book = {
       _year: 2004,
       edition: 1
   };
   Object.defineProperty(book, "year", {
       get: function() {
           return this._year;
       },
       set: function(newValue) {
           if (newValue > 2004) {
               this._year = newValue;
               this.edition += newValue - 2004;
           }
       }
   });
   book.year = 2005;
   alert(book.edition);
   ```

   It’s not necessary to assign both a getter and a setter. Assigning just a getter means that the property cannot be written to and attempts to do so will be ignored. In strict mode, trying to write to a property with only a getter throws an error.

   ## Reading Property Attributes

   It’s also possible to retrieve the property descriptor for a given property by using the ECMAScript 5 Object.getOwnPropertyDescriptor() method. This method accepts two arguments: the object on which the property resides and the name of the property whose descriptor should be retrieved. The return value is an object with properties for configurable , enumerable , get , and set for accessor properties or configurable , enumerable , writable , and value for data properties.

   ```javascript
   var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
   alert(descriptor.value); //2004
   alert(descriptor.confi gurable); //false
   alert(typeof descriptor.get); //"undefi ned"
   ```

   ​

   ## OBJECT CREATION

   1. ## Factory Pattern

   2. ## Constructor Pattern

      To create a new instance of Person , use the new operator. Calling a constructor in this manner essentially causes the following four steps to be taken:

      1. Create a new object.
      2. Assign the this value of the constructor to the new object (so this points to the new object).
      3. Execute the code inside the constructor (adds properties to the new object).
      4. Return the new object.

      **The constructor property was originally intended for use in identifying the object type. However, the**
      **instanceof operator is considered to be a safer way of determining type.**

      Constructors as Functions
      The only difference between constructor functions and other functions is the way in which they are
      called. Constructors are, after all, just functions; there’s no special syntax to defi ne a constructor
      that automatically makes it behave as such. Any function that is called with the new operator acts
      as a constructor, whereas any function called without it acts just as you would expect a normal
      function call to act. For instance, the Person() function from the previous example may be called
      in any of the following ways:

      ```javascript
      //use as a constructor
      var person = new Person("Nicholas", 29, "Software Engineer");
      person.sayName();//"Nicholas"

      //call as a function
      Person("Greg", 27, "Doctor");
      window.sayName();//"Greg" //adds to window
      //call in the scope of another object
      var o = new Object();
      Person.call(o, "Kristen", 25, "Nurse");
      o.sayName(); //"Kristen"
      ```

      ## Problems with Constructors

      The major downside to constructors is that methods are created once for each instance

      It’s possible to work around this limitation by moving the function defi nition outside of the constructor,
      as follows:

      ```javascript
      function Person(name, age, job) {
          this.name = name;
          this.age = age;
          this.job = job;
          this.sayName = sayName;
      }

      function sayName() {
          alert(this.name);
      }
      ```

      ​

   3. **The Prototype Pattern**

      Each function is created with a prototype property, which is an object containing properties and
      methods that should be available to instances of a particular reference type.



## How Prototypes Work

Whenever a function is created, its prototype property is also created according to a specifi c set of rules. By default, all prototypes automatically get a property called constructor that points back to the function on which it is a property. When defi ning a custom constructor, the prototype gets the constructor property only by default; all other methods are inherited from Object .. In ECMA-262 fi fth
edition, this is called [[Prototype]] . There is no standard way to access [[Prototype]] from script, but Firefox, Safari, and Chrome all support a property on every object called __proto__ .; in other implementations, this property is completely hidden from script.

**NOTE:-** The important thing to understand is that a direct link exists between the instance and the constructor’s prototype but not between the instance and the constructor.

Once a property is added to the object instance, it shadows any properties of the same name on the prototype, which means that it blocks access to the property on the prototype without altering it. Even setting the property to null only sets the property on the instance and doesn’t restore the link to the prototype. The delete operator, however, completely removes the instance property and allows the prototype property to be accessed again as follows:

```javascript
function Person() {}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function() {
    alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
person1.name = "Greg";
alert(person1.name);//"Greg" - from instance
alert(person2.name);  //"Nicholas" - from prototype
delete person1.name;
alert(person1.name);  //"Nicholas" - from the prototype
```

The hasOwnProperty() method determines if a property exists on the instance or on the prototype. This method, which is inherited from Object , returns true only if a property of the given name exists on the object instance

**Prototypes and the in Operator**

There are two ways to use the in operator: on its own or as a for-in loop. When used on its own, the in operator returns true when a property of the given name is accessible by the object, which is to say that the property may exist on the instance or on the prototype. Consider the following example:

```javascript
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
alert(person1.hasOwnProperty("name"));
alert("name" in person1); //true
```

## Important Note

Therefore, calling "name" in person1 always returns true , regardless of whether the property exists on the instance.

Since the in operator always returns true so long as the property is accessible by the object, and hasOwnProperty() returns true only if the property exists on the instance, a prototype property can be determined if the in operator returns true but hasOwnProperty() returns false .

When using a for-in loop, all properties that are accessible by the object and can be enumerated will be returned, which includes properties both on the instance and on the prototype.

Instance properties that shadow a non-enumerable prototype property (a property that has [[Enumerable]] set to false) will be returned in the for-in loop, since all developer-defi ned properties are enumerable by rule, except in Internet Explorer 8 and earlier.

To retrieve a list of all enumerable instance properties on an object, you can use the ECMAScript 5 Object.keys() method, which accepts an object as its argument and returns an array of strings containing the names of all enumerable properties.

If you’d like a list of all instance properties, whether enumerable or not, you can use Object .getOwnPropertyNames() in the same way:
var keys = Object.getOwnPropertyNames(Person.prototype);
alert(keys);//"constructor,name,age,job,sayName"



## Alternate Prototype Syntax

You may have noticed in the previous example that Person.prototype had to be typed out for each property and method. To limit this redundancy and to better visually encapsulate functionality on the prototype, it has become more common to simply overwrite the prototype with an object literal that contains all of the properties and methods.

Keep in mind that restoring the constructor in this manner creates a property with [[Enumerable]] set to true . Native constructor properties are not enumerable by default

## Dynamic Nature of Prototypes

Since the process of looking up values on a prototype is a search, changes made to the prototype at any point are immediately refl ected on instances, even the instances that existed before the change was made.

This happens because of the loose link between the instance and the prototype.Since the link between the instance and the prototype is simply a pointer, not a copy,

Although properties and methods may be added to the prototype at any time, and they are refl ected instantly by all object instances, you cannot overwrite the entire prototype and expect the same behavior.

The [[Prototype]] pointer is assigned when the constructor is called, so changing the prototype to a different object severs the tie between the constructor and the original prototype. Remember: the instance has a pointer to only the prototype, not to the constructor.

```javascript
function Person(){
}
var friend = new Person();
Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName: function() {
        alert(this.name);
    }
};
friend.sayName(); //error
```

Overwriting the prototype on the constructor means that new instances will reference the new prototype while any previously existing object instances still reference the old prototype.

## Native Object Prototypes

The prototype pattern is important not just for defi ning custom types but also because it is the pattern used to implement all of the native reference types. Each of these (including Object , Array , String , and so on) has its methods defi ned on the constructor’s prototype. For instance, the sort() method can be found on Array.prototype , and substring() can be found on String.prototype

## Problems with Prototypes

For one, it negates the ability to pass initialization arguments into the constructor, meaning that all instances get the same property values by default.The real problem occurs when a property contains a reference value

```javascript
function Person() {}
Person.prototype = {
    constructor: Person,
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    friends: ["Shelby", "Court"],
    sayName: function() {
        alert(this.name);
    }
};
var person1 = new Person();
var person2 = new Person();
person1.friends.push("Van");
alert(person1.friends); //"Shelby,Court,Van"
alert(person2.friends);//"Shelby,Court,Van"
alert(person1.friends === person2.friends); //true
```

Here, the Person.prototype object has a property called friends that contains an array of strings. Two instances of Person are then created. The person1.friends array is altered by adding another string. Because the friends array exists on Person.prototype , not on person1 , the changes made are also refl ected on person2.friends (which points to the same array). If the intention is to have an array shared by all instances, then this outcome is okay. Typically, though, instances want to have their own copies of all properties. This is why the prototype pattern is rarely used on its own.



## Combination Constructor/Prototype Pattern

The most common way of defi ning custom types is to combine the constructor and prototype patterns. The constructor pattern defi nes instance properties, whereas the prototype pattern defi nes methods and shared properties.



## Dynamic Prototype Pattern

The dynamic prototype pattern seeks to solve this problem by encapsulating all of the information within the constructor while maintaining the benefi ts of using both a constructor and a prototype by initializing the prototype inside the constructor, but only if it is needed.

NOTE:- You cannot overwrite the prototype using an object literal when using the dynamic prototype pattern. As described previously, overwriting a prototype when an instance already exists effectively cuts off that instance from the new prototype.



# INHERITANCE

The concept most often discussed in relation to OO programming is inheritance. Many OO languages support two types of inheritance: interface inheritance, where only the method signatures are inherited, and implementation inheritance, where actual methods are inherited.Interface inheritance is not possible in ECMAScript, because, as mentioned previously, functions do not have signatures. Implementation inheritance is the only type of inheritance supported by ECMAScript,and this is done primarily through the use of prototype chaining.

## Prototype Chaining

The basic idea is to use the concept of prototypes to inherit properties and methods between two reference types.

Recall the relationship between constructors, prototypes, and instances: each constructor has a prototype object that points back to the constructor, and instances have an internal pointer to the prototype.

**Prototype chaining** :- What if the prototype were actually an instance of another type? That would mean the prototype itself would have a pointer to a different prototype that, in turn, would have a pointer to another constructor. If that prototype were also an instance of another type, then the pattern would continue, forming a chain between instances and prototypes. This is the basic idea behind prototype chaining

```javascript
function SuperType() {
    this.property = true;
}
SuperType.prototype.getSuperValue = function() {
    return this.property;
};

function SubType() {
    this.subproperty = false;
}
//inherit from SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function() {
    return this.subproperty;
};
var instance = new SubType();
alert(instance.getSuperValue()); //true
```

This code defi nes two types: SuperType and SubType . Each type has a single property and a single method. The main difference between the two is that SubType inherits from SuperType by creating a new instance of SuperType and assigning it to SubType.prototype .This overwrites the original prototype and replaces it with a new object, which means that all properties and methods that typically exist on an instance of SuperType now also exist on SubType.prototype . After the inheritance takes place, a method is assigned to SubType.prototype , adding a new method on top of what was inherited from SuperType .

That new prototype happens to be an instance of SuperType , so it not only gets the properties and methods of a SuperType instance but also points back to the SuperType ’s prototype. So instance points to SubType.prototype , and SubType.prototype points to SuperType.prototype .Also note that instance.constructor points to SuperType , because the constructor property on the SubType.prototype was overwritten.

## Default Prototypes

All reference types inherit from Object by default, which is accomplished through prototype chaining. The default prototype for any function is an instance of Object , meaning that its internal prototype pointer points to Object .prototype . This is how custom types inherit all of the default methods such as toString() and valueOf() .

## Prototype and Instance Relationships

The relationship between prototypes and instances is discernible in two ways. The fi rst way is to use the instanceof operator, which returns true whenever an instance is used with a constructor that appears in its prototype chain

The second way to determine this relationship is to use the isPrototypeOf() method

## Working with Methods

Often a subtype will need to either override a supertype method or introduce new methods that don’t exist on the supertype. To accomplish this, the methods must be added to the prototype after the prototype has been assigned.

Another important thing to understand is that the object literal approach to creating prototype methods cannot be used with prototype chaining, because you end up overwriting the chain

## Problems with Prototype Chaining

The major issue revolves around prototypes that contain reference values When implementing inheritance using prototypes, the prototype actually becomes an instance of another type, meaning that what once were instance properties are now prototype properties

```javascript
function SuperType() {
    this.colors = ["red", "blue", "green"];
}

function SubType() {}
//inherit from SuperType
SubType.prototype = new SuperType();
var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green,black"
```

A second issue with prototype chaining is that you cannot pass arguments into the supertype constructor when the subtype instance is being created. In fact, there is no way to pass arguments into the supertype constructor without affecting all of the object instances. Because of this and the aforementioned issue with reference values on the prototype, prototype chaining is rarely used alone



## Constructor Stealing

In an attempt to solve the inheritance problem with reference values on prototypes, developers began using a technique called constructor stealing (also sometimes called object masquerading or classical inheritance).The basic idea is quite simple: call the supertype constructor from within the subtype constructor. Keeping in mind that functions are simply objects that execute code in a particular context, the apply() and call() methods can be used to execute a constructor on the newly created object as in this example:

```javascript
function SuperType() {
    this.colors = ["red", "blue", "green"];
}

function SubType() {
    //inherit from SuperType
    SuperType.call(this);   //The highlighted lines in this example show the single call that is used in constructor stealing
}
var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);
//"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors);
//"red,blue,green"
```

By using the call() method (or alternately, apply() ), the SuperType constructor is called in the context of the newly created instance of SubType . Doing this effectively runs all of the object- initialization code in the SuperType() function on the new SubType object. The result is that each instance has its own copy of the colors property.

**Passing Arguments**

One advantage that constructor stealing offers over prototype chaining is the ability to pass arguments into the supertype constructor from within the subtype constructor. Consider the following:

```javascript
function SuperType(name) {
    this.name = name;
}

function SubType() {
    //inherit from SuperType passing in an argument
    SuperType.call(this, "Nicholas"); //instance property
    this.age = 29;
}
var instance = new SubType();
alert(instance.name); //"Nicholas";
alert(instance.age); //29
```

## Problems with Constructor Stealing

The downside to using constructor stealing exclusively is that it introduces the same problems as the constructor pattern for custom types: methods must be defi ned inside the constructor, so there’s no function reuse.



## Combination Inheritance

combines prototype chaining and constructor stealing to get the best of each approach.The basic idea is to use prototype chaining to inherit properties and methods on the prototype and to use constructor stealing to inherit instance properties

```javascript
function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
    alert(this.name);
};

function SubType(name, age) {
    //inherit properties
    SuperType.call(this, name);
    this.age = age;
}
//inherit methods
SubType.prototype = new SuperType();
SubType.prototype.sayAge = function() {
    alert(this.age);
};
var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29
var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27
```

Addressing the downsides of both prototype chaining and constructor stealing, combination inheritance is the most frequently used inheritance pattern in JavaScript. It also preserves the behavior of instanceof and isPrototypeOf() for identifying the composition of objects.



## Prototypal Inheritance

ECMAScript 5 formalized the concept of prototypal inheritance by adding the Object.create() method. This method accepts two arguments, an object to use as the prototype for a new object and an optional object defi ning additional properties to apply to the new object. When used with one argument, Object.create() behaves the same as the object() method:

```javascript
var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = Object.create(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");
var yetAnotherPerson = Object.create(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends); //"Shelby,Court,Van,Rob,Barbie"
```



## Parasitic Inheritance

The basic parasitic inheritance pattern looks like this:

```javascript
function createAnother(original) {
        var clone = object(original);
        clone.sayHi = function() {
            alert("hi");
        };
        return clone;
    }

var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"
```

Parasitic inheritance is another pattern to use when you are concerned primarily with objects and not with custom types and constructors. The object() method is not required for parasitic inheritance; any function that returns a new object fi ts the pattern.

## Parasitic Combination Inheritance

Parasitic combination inheritance uses constructor stealing to inherit properties but uses a hybrid form of prototype chaining to inherit methods The basic idea is this: instead of calling the
supertype constructor to assign the subtype’s prototype, all you need is a copy of the supertype’s prototype. Essentially, use parasitic inheritance to inherit from the supertype’s prototype and then assign the result to the subtype’s prototype. The basic pattern for parasitic combination inheritance is as follows

```javascript
function inheritPrototype(subType, superType) {
    var prototype = object(superType.prototype);  //create object
    prototype.constructor = subType;   //augment object
    subType.prototype = prototype;     //assign object
}

function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
    alert(this.name);
};

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function() {
    alert(this.age);
};
```

Parasitic combination inheritance is considered the most optimal inheritance paradigm for reference types.