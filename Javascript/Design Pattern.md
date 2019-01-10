# Learning JavaScript Design Patterns

https://addyosmani.com/resources/essentialjsdesignpatterns/book/

# What is a Pattern?

A pattern is a reusable solution that can be applied to commonly occurring problems in software design - in our case - in writing JavaScript web applications.

Patterns are **not** an exact solution. It’s important that we remember the role of a pattern is merely to provide us with a solution scheme. Patterns don’t solve all design problems nor do they replace good software designers, however, they **do** support them.

Design patterns have three main benefits:

1. **Patterns are proven solutions**
2. **Patterns can be easily reused**
3. **Patterns can be expressive**
4. **Reusing patterns assists in preventing minor issues that can cause major problems in the application development process**
5. **Patterns can provide generalized solutions which are documented in a fashion that doesn't require them to be tied to a specific problem.**
6. **Certain patterns can actually decrease the overall file-size footprint of our code by avoiding repetition.**
7. **Patterns add to a developer's vocabulary, which makes communication faster.**
8. **Patterns that are frequently used can be improved over time by harnessing the collective experiences other developers using those patterns contribute back to the design pattern community.**

# "Pattern"-ity Testing, Proto-Patterns & The Rule Of Three

**Proto-Pattern** a pattern that has not yet been known to pass the "pattern"-ity tests is usually referred to as a proto-pattern.Proto-patterns may result from the work of someone that has established a particular solution that is worthy of sharing with the community, but may not have yet had the opportunity to have been vetted heavily due to its very young age.

Brief descriptions or snippets of this type of pattern are known as patlets.

a pattern may be considered "good" if it does the following:

1. **Solves a particular problem**:
2. **The solution to this problem cannot be obvious**
3. **The concept described must have been proven**
4. **It must describe a relationship**

# rule of three

One of the additional requirements for a pattern to be valid is that they display some recurring phenomenon. This is often something that can be qualified in at least three key areas, referred to as the *rule of three*. To show recurrence using this rule, one must demonstrate:

1. **Fitness of purpose** - how is the pattern considered successful?
2. **Usefulness** - why is the pattern considered successful?
3. **Applicability** - is the design worthy of being a pattern because it has wider applicability? If so, this needs to be explained. When reviewing or defining a pattern, it is important to keep the above in mind.

# The Structure Of A Design Pattern

A pattern is initially presented in the form of a **rule** that establishes a relationship between:

- A **context**
- A system of **forces** that arises in that context and
- A **configuration** that allows these forces to resolve themselves in context

## component elements for a design pattern

- **Pattern name** and a **description**
- **Context outline** – the contexts in which the pattern is effective in responding to the users needs.
- **Problem statement** – a statement of the problem being addressed so we can understand the intent of the pattern.
- **Solution** – a description of how the user’s problem is being solved in an understandable list of steps and perceptions.
- **Design** – a description of the pattern’s design and in particular, the user’s behavior in interacting with it
- **Implementation **– a guide to how the pattern would be implemented
- **Illustrations** – a visual representation of classes in the pattern (e.g. a diagram)
- **Examples** – an implementation of the pattern in a minimal form
- **Co-requisites** – what other patterns may be needed to support use of the pattern being described?
- **Relations** – what patterns does this pattern resemble? does it closely mimic any others?
- **Known usage** – is the pattern being used in the *wild*? If so, where and how?
- **Discussions** – the team or author’s thoughts on the exciting benefits of the pattern

# Writing Design Patterns

Remember - solutions in which neither interactions nor defined rules appear are *not* patterns.

Explore structure and semantics - this can be done by examining the interactions and context of the patterns you are interested in so you can identify the principles that assist in organizing those patterns together in useful configurations.

The following are tips I would suggest if interested in creating a new design pattern:

- **How practical is the pattern?**: Ensure the pattern describes proven solutions to recurring problems rather than just speculative solutions which haven’t been qualified.
- **Keep best practices in mind:** The design decisions we make should be based on principles we derive from an understanding of best practices.
- **Our design patterns should be transparent to the user**: Design patterns should be entirely transparent to any type of user-experience. They are primarily there to serve the developers using them and should not force changes to behavior in the user-experience that would not be incurred without the use of a pattern.
- **Remember that originality is not key in pattern design**: When writing a pattern, we do not need to be the original discoverer of the solutions being documented nor do you have to worry about our design overlapping with minor pieces of other patterns. If the approach is strong enough to have broad useful applicability, it has a chance of being recognized as a valid pattern.
- **Patterns need a strong set of examples:** A good pattern description needs to be followed by an equally strong set of examples demonstrating the successful application of our pattern. To show broad usage, examples that exhibit good design principles are ideal.

# Anti-Patterns

If we consider that a pattern represents a best practice, an anti-pattern represents a lesson that has been learned. The term anti-patterns was coined in 1995 by Andrew Koenig in the November C++ Report that year, inspired by the GoF's book *Design Patterns*. In Koenig’s report, there are two notions of anti-patterns that are presented. Anti-Patterns:

- Describe a* bad* solution to a particular problem which resulted in a bad situation occurring
- Describe *how* to get out of said situation and how to go from there to a good solution

Examples of anti-patterns in JavaScript are the following:

- Polluting the global namespace by defining a large number of variables in the global context
- Passing strings rather than functions to either setTimeout or setInterval as this triggers the use of `eval()` internally.
- Modifying the `Object` class prototype (this is a particularly bad anti-pattern)
- Using JavaScript in an inline form as this is inflexible
- The use of document.write where native DOM alternatives such as document.createElement are more appropriate

# Categories Of Design Pattern

*“A design pattern names, abstracts, and identifies the key aspects of a common design structure that make it useful for creating a reusable object-oriented design. The design pattern identifies the participating classes and their instances, their roles and collaborations, and the distribution of responsibilities.*

*Each design pattern focuses on a particular object-oriented design problem or issue. It describes when it applies, whether or not it can be applied in view of other design constraints, and the consequences and trade-offs of its use. Since we must eventually implement our designs, a design pattern also provides sample ... code to illustrate an implementation.*

*Although design patterns describe object-oriented designs, they are based on practical solutions that have been implemented in mainstream object-oriented programming languages ...*"

## 1. Creational Design Patterns

Creational design patterns focus on handling object creation mechanisms where objects are created in a manner suitable for the situation we're working in. The basic approach to object creation might otherwise lead to added complexity in a project whilst these patterns aim to solve this problem by *controlling* the creation process.

Some of the patterns that fall under this category are: Constructor, Factory, Abstract, Prototype, Singleton and Builder.

## 2. Structural Design Patterns

Structural patterns are concerned with object composition and typically identify simple ways to realize relationships between different objects. They help ensure that when one part of a system changes, the entire structure of the system doesn't need to do the same. They also assist in recasting parts of the system which don't fit a particular purpose into those that do.

Patterns that fall under this category include: Decorator, Facade, Flyweight, Adapter and Proxy.

## 3. Behavioral Design Patterns

Behavioral patterns focus on improving or streamlining the communication between disparate objects in a system.

Some behavioral patterns include: Iterator, Mediator, Observer and Visitor.



# Design Pattern Categorization

**A brief note on classes**  :- JavaScript is a class-less language, however classes can be simulated using functions.

The most common approach to achieving this is by defining a JavaScript function where we then create an object using the `new` keyword. `this` can be used to help define new properties and methods for the object as follows:

```javascript
// A car "class"
function Car( model ) {
  this.model = model;
  this.color = "silver";
  this.year = "2012";
  this.getInfo = function () {
    return this.model + " " + this.year;
  };
}
```

NOTE:- [3 ways to define a JavaScript class](http://www.phpied.com/3-ways-to-define-a-javascript-class/)

# JavaScript Design Patterns

## 1. The Constructor Pattern

Object Creation:-The three common ways to create new objects in JavaScript are as follows:

`var` `newObject = {};`

`// or`

`var` `newObject = Object.create( Object.prototype );`

`// or`

`var` `newObject = ``new` `Object();`

There are then four ways in which keys and values can then be assigned to an object:

1. Dot Syntax              (newObject.name = "Appple")

2. Square Bracket Syntax          (newObject["name"] = "Apple")

3. Object.defineProperty           

   ```javascript
   Object.defineProperty( newObject, "someKey", {

   "value": "for more control of the property's behavior",

   "writable": true,

   "enumerable": true,

   "configurable": true

   });
   ```

4. Object.defineProperties

```javascript
Object.defineProperties( newObject, {
  "someKey": {
    value: "Hello World",
    writable: true
  },
  "anotherKey": {
    value: "Foo bar",
    writable: false
  }
});
```



Basic Constructors

Constructors With Prototypes

## 2. The Module Pattern

## Modules:-

In JavaScript, there are several options for implementing modules. These include:

- The Module pattern
- Object literal notation
- AMD modules
- CommonJS modules
- ECMAScript Harmony modules

**Object Literals** http://rmurphey.com/blog/2009/10/15/using-objects-to-organize-your-code/

**The Module Pattern** :- The Module pattern was originally defined as a way to provide both private and public encapsulation for classes in conventional software engineering.

​	Module Pattern Variations:-

​	1. **Import mixins**                	2.**Exports**



