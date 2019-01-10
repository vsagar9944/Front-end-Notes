

# Angular JS Questions

1. ### **What is the difference between angular-route and angular-ui-router?**

  **Ans**:- [ui-router](https://github.com/angular-ui/ui-router) is a 3rd-party module and is very powerful. It supports everything the normal ngRoute can do as well as many extra functions.

  Here are some common reason ui-router is chosen over ngRoute:

  - ui-router allows for [nested views](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views) and [multiple named views](https://github.com/angular-ui/ui-router/wiki/Multiple-Named-Views). This is very useful with larger app where you may have pages that inherit from other sections.
  - ui-router allows for you to have strong-type linking between states based on state names. Change the url in one place will update every link to that state when you build your links with [`ui-sref`](http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.directive:ui-sref). Very useful for larger projects where URLs might change.
  - There is also the concept of the [decorator](http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.$stateProvider#methods_decorator) which could be used to allow your routes to be dynamically created based on the URL that is trying to be accessed. This could mean that you will not need to specify all of your routes before hand.
  - [states](https://github.com/angular-ui/ui-router/wiki#state-manager) allow you to map and access different information about different states and you can easily pass information between states via [`$stateParams`](https://github.com/angular-ui/ui-router/wiki/URL-Routing#stateparams-service).
  - You can easily determine if you are in a state or parent of a state to adjust UI element (highlighting the navigation of the current state) within your templates via [`$state`](http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.$state) provided by ui-router which you can expose via setting it in `$rootScope` on `run`.

  In essence, ui-router is ngRouter with more features, under the sheets it is quite different. These additional features are very useful for larger applications.

  More Information:

  - Github: <https://github.com/angular-ui/ui-router>

  - Documentation:

    - API Reference: <http://angular-ui.github.io/ui-router/site/#/api>
    - Guide: <https://github.com/angular-ui/ui-router/wiki>

  - FAQs: <https://github.com/angular-ui/ui-router/wiki/Frequently-Asked-Questions>

  - Sample Application: <http://angular-ui.github.io/ui-router/sample/#/

    ​

2. ### What is the most efficient way to deep clone an object in JavaScript?

   Ans:-// Shallow copy

   var newObject = jQuery.extend({}, oldObject);

   // Deep copy
   var newObject = jQuery.extend(true, {}, oldObject);

   var clone2 = Object.assign({}, window);

   var arry2 = arry1.slice()

   [`angular.copy`](https://docs.angularjs.org/api/ng/function/angular.copy) 

3. What does “use strict” do in JavaScript, and what is the reasoning behind it?

   https://stackoverflow.com/questions/1335851/what-does-use-strict-do-in-javascript-and-what-is-the-reasoning-behind-it?rq=1

   http://novogeek-archive.azurewebsites.net/post/ECMAScript-5-Strict-mode-support-in-browsers-What-does-this-mean

   https://yuiblog.com/blog/2010/12/14/strict-mode-is-coming-to-town/

   Strict Mode is a new feature in ECMAScript 5 that allows you to place a program, or a function, in a "strict" operating context. This strict context prevents certain actions from being taken and throws more exceptions.

   Strict mode helps out in a couple ways:

   - It catches some common coding bloopers, throwing exceptions.
   - It prevents, or throws errors, when relatively "unsafe" actions are taken (such as gaining access to the global object).
   - It disables features that are confusing or poorly thought out.

   Also note you can apply "strict mode" to the whole file... Or you can use it only for a specific function 

   The statement `"use strict";` instructs the browser to use the Strict mode, which is a reduced and safer feature set of JavaScript.

   ## List of features (non-exhaustive)

   1. Disallows global variables. (Catches missing `var` declarations and typos in variable names)
   2. Silent failing assignments will throw error in strict mode (assigning `NaN = 5;`)
   3. Attempts to delete undeletable properties will throw (`delete Object.prototype`)
   4. Requires all property names in an object literal to be unique (`var x = {x1: "1", x1: "2"}`)
   5. Function parameter names must be unique (`function sum (x, x) {...}`)
   6. Forbids octal syntax (`var x = 023;` some devs assume wrongly that a preceding zero does nothing to change the number.)
   7. Forbids the `with` keyword
   8. `eval` in strict mode does not introduce new variables
   9. Forbids deleting plain names (`delete x;`)
   10. Forbids binding or assignment of the names `eval` and `arguments` in any form
   11. Strict mode does not alias properties of the `arguments` object with the formal parameters. (i.e. in `function sum (a,b) { return arguments[0] + b;}` This works because `arguments[0]`is bound to `a` and so on. )
   12. `arguments.callee` is not supported





