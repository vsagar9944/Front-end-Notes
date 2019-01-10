As a manager, your primary contribution is creating a high-performance team and the primary driver of high performance in teams and organizations is a culture of peer accountability.

[Directives Resource to study](https://stackoverflow.com/questions/24615103/angular-directives-when-and-how-to-use-compile-controller-pre-link-and-post/24615137#24615137)

# An overview of good code

Angular directives have access to all those events as well (the $element variable you saw earlier is actually a jQuery wrapped DOM element)
The $emit and $broadcast methods serve to send messages up and down the scope tree respectively,

Angular uses modules to separate different pieces of your application for easier testing and modularity. The recommended approach is to have separate modules for your services, directives, and filters, and one application-level module that imports all of them and then executes your initialization code, if any.


At the time of writing, when developing for Internet Explorer 8 & 9 browsers, the ng-app directive must be declared twice if you attach it to the html node. In addition to using the attribute to declare which module to use, you must also add the id tag of "ngApp", otherwise IE won't notice it during the bootstrap process.

Directives Defifnition
```javascript
angular.module('myApp.directives', [])
    .directive('myAwesomeDirective', ['api', function(api) {
        //Do any one-time directive initialization work here
        return function($scope, $element, $attrs) {
        //Do directive work that needs to be applied to each
        instance here
    };
}]);
```
1. Within your factory function, you can either return another function, as we did here, or a configuration object
2. The function accepts the current scope for the instance of that directive, the jQuery wrapped DOM element, and any attributes attached to the element as parameters, and often that will be all you'll need to use for your directive.
3. This function gets called each time the directive is attached to a DOM element and can be used to attach plugins, retrieve additional data, and so on.


### Naming Conventions for Directives
1. All directives are named using camelCase in JavaScript, and snake case(__Snake case, here, means all lower case, using either : , - , or _ to separate the words.__) within your HTML,Thus, for us, the JavaScript name myAwesomeDirective becomes my-awesome-directive , my:awesome:directive , or my_awesome_directive in the HTML, all of which will properly bind the directive to the DOM node.
2. if you're running your HTML through a validator and don't like seeing warnings about each of your custom directives, you can even prepend x- or data- to the directive name and Angular will still detect it.

### Attachment styles
When developing your own directive, you can control which methods you want to utilize through the restrict property, using a subset of EACM (standing for element, attribute, class, comment).
By far, the most common method for invoking a directive is through an attribute, in large part because it's the default if the restrict property is left undefined.
```html
    <my-awesome-directive></my-awesome-directive>
```
This method has the advantage of being exceptionally semantically accurate, and, let's be honest, it looks the coolest because we're creating our own HTML elements. The disadvantage, as usual, is that Internet Explorer will read these in as div instances, thus preventing Angular from knowing that they even exist. You can get around this by using document.createElement('my-awesome-directive') in the head of your HTML, but you have to do this separately for each directive, and except in rare cases I usually consider it to be more work than it's worth.

### Configuration options
__(directive definition object)__
there are two ways to initialize a directive within the factory function
1. The first is to return our linking function
2. second is to use this definition object to provide more fine-grained control over the way our directive functions.

DDO:-
```javascript
{
    priority : 10,    //:--> which allows us to specify in what order directives should be executed, if there are multiple directives on the same node. The default is 0, and unlike many languages, higher numbers go first here. There is no specification for what order directives of the same priority will execute in, so if order is really important, it's best to make that explicit
    terminal : false,    //:--> terminal dictates whether or not directive execution should stop after the priority level. __directive processing continues after a terminal directive until the end of its priority level, ensuring that the results will be consistent__
    template: '<div><h3>{{title}}</h3></div>',   //:-->Do note that the compile/link process for this directive will be suspended until the template is loaded, so if your custom HTML is minimal, it's usually more efficient to provide it inline.
    templateUrl : 'myDirective.html',
    replace : true,   //:--> Use of the replace property specifies whether the whole element should be replaced with the template, or if the template HTML should just replace the element's inner HTML.
    compile : function (element, attributes, transclude) {},
    link : function ($scope, $element, $attrs) {},    //:-->you can think of compile as performing any tasks that require restructuring the DOM (and possibly adding other directives) regardless of the specific scope , and the link function as attaching a scope to that compiled element.
    scope : true,
    controller : function ($scope, $element, $attrs) {},
    require : 'myAwesomeDirective',
    transclude : true
};
```

__replace__
If you do choose to replace the entire element, note that Angular will do its best to copy over all of the classes/attributes from the original element, including merging the class attributes together. Additionally, if you want to replace the original element, your template must have only one root node. If you try to use a template with multiple root nodes (such as <h2>{{title}}</h2><div>{{content}}</ div> ), Angular will throw an error as there's no way to migrate the attributes over consistently.

__compile/link__
If you set a value for the compile property, Angular will ignore the link property. If you need to use both, you can return an object from the compile function, with pre set to your compile function, and post set to your link function, as demonstrated in the following code:.
```javascript
angular.directive('myAwesomeDirective', function () {
    return {
        compile : function (tElement, tAttrs, transclude) {
        pre : function compile ($scope, $element, $attrs) {},
        post : function link ($scope, $element, $attrs) {}
        return{
            ...//current pre and post text
            }
        }
    }
});
```
__Scope__
1. If left undefined, the scope value is null, which tells Angular to give the directive the same scope as the object its attached to.
2. If, however, you want to generate a new scope for your directive, there are two ways you can do so.
    1. First, simply set the scope parameter to true , which will create a new scope for the directive, but still inherit from it's parent.This means you'll still be able to read all of the values from your parent scope, including adding any new watchers to monitor data changes, but new values you write onto the scope won't affect the parent scope's values.
        NOTE:- this is a bit of an over generalization, as it's simply inherited from it's parent prototypically, like all other JavaScript objects. Which means if you set $scope.name = “bob” on the child scope, the parent won't be touched. If instead, however, you set $scope.data. name = “bob”, it will be changed, as you're actually reading the “data” object first, then setting the value on that, and since that object is shared between parent and child, both will reflect the change.
        $scope.name = “bob”
        $scope.data.name = “bob”
        “data”
        2.if you want to isolate your directive from the rest of your application, you can create an aptly named isolate scope.This scope can be helpful in ensuring modularity and preventing accidental changes to data outside of your directive caused by shared properties or methods.
        NOTE:- As a final note on scope for this chapter, each DOM element can only have one scope applied to it, which means that if you set scope : true for multiple directives on the same node, they'll all share the same new scope. While this is usually fine, do note that only one directive on a node can request an isolate scope, and all other directives will share that scope

__Controllers/require__  
The controller function can store many of the same properties or methods that you might normally attach to the scope discussed earlier, however, if they are attached to the controller itself, they can be shared with other directives in the DOM tree. This sharing is done via the require property, which tells Angular to grab the instance of one directive's controller and make it available to another directive.
Example
```javascript
angular.directive('autocompleteInput', function () {
    return {
        require : 'ngModel',
        link : function ($scope, $element, $attrs, ngModel) {
        ngModel.$render = function () {
        $element.val(ngModel.$viewValue || '');
        };
        $element.autocomplete({
            ... //Define source, etc
            select : function (ev, ui) {
                    $scope.$apply(function() {
                        ngModel.$setViewValue(ui.item.value);
                    });
                }
            });
        }
    }
});
```
    You can see we've used the require property to tell Angular that we want access to the controller instance on the ngModel directive, which then gets passed into our link function as the fourth parameter. Because we've required the ngModel controller, we also have to declare the directive that provides that controller, otherwise Angular will throw an error, which means that our input element needs to look something like this:
```html
<input ng-model="data.property" autocomplete-input />
```
Obviously, throwing an error is often not what we want to happen, so Angular allows you to prepend a question mark, such as ?ngModel , to make that requirement optional. You can also use a caret ( ^ngModel ), to tell Angular to traverse upwards from the element node through the DOM tree and look for the directive in the elements there, such as the following:
```html
<div ng-model="data.property">
    <input autocomplete-input />
</div>
```
NOTE:- If you need access to multiple controllers, you can also pass in an array to the require property, and likewise the fourth parameter of your link function will be an array of those controllers.


__Transclusion__(Chapter 7, Transclusion)
transclusion provides the ability to have an isolate scope


### Compile versus Link
Within Angular directives, there are two primary phases that handle the process of connecting the directive logic to the DOM, as well as performing any necessary DOM manipulation.
1. __compile phase__ :-  which works on the element before it's been inserted into the document, and thus is great for performance, but can't be used to attach any DOM related plugins since the element isn't accessible yet.
2. __linking__ :- which works on the element after it has been inserted into the DOM and has the appropriate scope instance created and initialized for it.

#### Peeking under the covers
The first thing to know is that both returning a function from our directive factory (instead of using the definition object) and defining a function on the link parameter are really just shortcuts to setting the compile property to be a function that returns that same linking function. In other words, no matter how we define our link function, by the time Angular gets around to processing our directive, it will always call the compile function and take what it returns as the linking function(s)
For proof, here are few lines of code that make that initial call:
```javascript
...
linkFn = directive.compile($compileNode, templateAttrs,childTranscludeFn);
if (isFunction(linkFn)) {
    addLinkFns(null, linkFn);
    //Only attach to 'postLink' functions
}else if (linkFn) {
    addLinkFns(linkFn.pre, linkFn.post);
}
```
First, compile is called really early on during this whole process. $compileNode represents the element we're manipulating, but it's not anywhere in the DOM just yet, instead it's much like what you would receive if you called jQuery (<div>) . What this means is while we can manipulate it within the compile function (and if we need to conditionally add other directives or change the pre-parsed HTML, this is where we have to manipulate it), we can't attach any DOM listeners or plugins that need to process a fully-realized node just yet.
Secondly, there are actually two types of linking functions, pre and post. When Angular is processing your directive, it will first execute any and all of the pre-linking functions on the directive element itself, then it will recurse down into any child nodes within the element and compile those, then walk back up the DOM tree and execute all of the post-linking functions. As a result, the whole compilation process proceeds through the following flow:

--------------------
| mainDirective    |
| - compile fn     |
| -preLinking fn   |
--------------------
         |
         |
         |
         V
-----------------------
| firstChildDirective |
| - compile fn        |
| -preLinking fn      |
-----------------------
        |
        |
        |
        V
-----------------------
| lastChildDirective  |
| - compile fn        |
| -preLinking fn      |
| - postLinking fn    |
-----------------------
        |
        |
        |
        V
-----------------------
| firstChildDirective |
| - postLink fn       |
-----------------------
        |
        |
        |
        V
-----------------------
|    mainDirective    |
|    - postLink fn    |
-----------------------



    As a general rule, the same DOM manipulation restrictions apply to both the compile function and the pre-linking function, since the element is still being updated by Angular and cannot be cloned or otherwise modified before creating the final instance that will be inserted into the document.
    The post-linking function, on the other hand, receives the final element and can be manipulated as you need.
    The pre-linking function is only advantageous when you need to perform some individualized preparation on the scope or controller object before any child elements compile, (__Chapter 6, Controllers–Better with Sharing.__)
    
    Compile, on the other hand, is ideal when you need to make use of the transclusion function or conditionally modify your element before it's compiled,

### Working examples of Angular Directives
1. __ng-repeat__ :- The definition object is as follows:
    ```javascript
    ...
    .directive('ngRepeat', function () {
        return {
            priority: 1000,
            transclude: 'element',
            terminal: true,
            compile: function(element, attr, linker) { // Compile Function
                    return function(scope, iterStartElement, attr){ // Linking Function
                };
            }
        }
    });
    ```
    A priority value of 1000 makes sure that this directive executes before any others on the element, and setting terminal to true ensures that Angular's automatic compilation doesn't continue on to any other directives on this element or its children. We want this because the ng-repeat compile function itself will actually be handling the compilation processing for the node tree of child elements. A transclude value of element is used to collect the HTML content of the original element, as we'll need that to be able to compile it later and as such we'll see that it is used here shortly within the compile function.
    __Compile__ :- For the purpose of our examination, however, the final parameter is by far the most valuable. This linker is the transcluded function, which Angular would normally use to attach a scope to this element, interpolate all of the values, and then insert the final object into the DOM. By grabbing this linking function, ng-repeat now has control over when to perform the actual linking, as well as the ability to repeatedly do so, which of course is exactly what we need.
    __Link__ :- The linking function is what the compile function returns. At the heart of the ng-repeat linking function is $scope.$watch , which serves to connect the data and data changes to the rest of our code.
    Within the linker function, the scope gets bound to the compiled, but not yet interpolated, element HTML, and a new "fully transformed" element is generated.

2. __ng-switch__ :- definition object:
```javascript
{
    restrict: 'EA',
    require: 'ngSwitch',
    controller: function () {
        this.cases = {};
    },
    link: function(scope, element, attr, ctrl) {
        var watchExpr = attr.ngSwitch, // Read in the data property we want to monitor
        selectedLinker,
        selectedElement,
        selectedScope;
        scope.$watch(watchExpr, function (value) {
            if (selectedElement) { // remove any prior HTML within $element
                selectedScope.$destroy();
                selectedElement.remove();
                selectedElement = selectedScope = null;
            }
            if ((selectedLinker = ctrl.cases['!' + value] || ctrl.cases['?'])) {
                selectedScope = scope.$new();   
                selectedLinker(selectedScope, function(caseElement) {
                    selectedElement = caseElement;
                    element.append(caseElement);
                });
            }
        });
    }
}
```

# Keeping it Clean with Scope
1. Scope = false
    The scope object within Angular serves as the primary intersection point between our data, our view, and the rest of our code.
    When a new directive is initialized with a scope property of false, that directive receives the same scope as its parent. Not an inherited scope, but the exact same object. This means that all changes to the parent scope will be reflected within our directive, and also any change within our directive's scope will be reflected within the parent.
2. Scope = true
    In this case, a new inherited scope is actually generated, much like the behavior when nesting controllers
3. Scope = {}
    For times when you want to have complete control over which properties and methods are interconnected within your new directive scope, an object hash is usually your best solution.This type of scoping is commonly referred to as an isolate scope, because of the lack of connectedness with the other scopes within the application.
    While there are a few instances where we want our directive to be entirely isolated, more commonly we'll want to maintain access to a few explicitly specified properties and methods from the ancestral scope tree. To do this, Angular provides three symbols for notating what type of access you want to acquire: @, =, and &, which are prepended to the attribute names that you want to derive a value from. As such, we can create an isolate scope that looks like this:
    scope : {
        'myReadonlyVariable' : '@myStringAttr',
        'myTwowayVariable' : '=myParentProperty',
        'myInternalFunction' : '&myParentFunction'
    }
    1. @ – read-only Access :- Using the @ symbol to retrieve a value from your attributes means that the attribute value will be interpolated and whatever is returned will be stored within the scope property that you specify.it is important to realize that even though we've requested only read-only access for this attribute, it will still be dynamically updated when the parent scope changes.
    2. = – two-way binding :- For occasions where you want full access to a specific property on the parent scope, Angular provides the = symbol for use within our isolate scope.
    3. & – method binding :- Sometimes you need to be able to call a method on the parent scope.


### Controllers – Better with Sharing
controllers provide unparalleled communication between directives on the same node, or even just within the same ancestral tree.
Example :- Forms and inputs
Within Angular, every form element is also a directive whose primary purpose is to instantiate a FormController object, attach it to the form element and, if the form is named, register that form controller on the parent scope so that it can be more easily accessed.Likewise, all input and input-esque elements are also built-in directives,although unless they have an ng-model attribute on them, they don't actually have any extra functionality.

```html
<form name="exampleForm">
    <input type="text" ng-model="myName" />
</form>
```
The FormController function is primarily responsible for monitoring the overall validity of the form, based upon the validity of the individual elements within it. It does this through the use of the four main functions within the controller function outlined as shown:
```javascript
    function FormController(element, attrs) {
    //All controller functions and properties you want to export are bound to 'this'
    this.$addControl = function(control) { ...}; //Register an input element
    this.$removeControl = function(control) { ...}; //Unregister an element
    this.$setValidity = function(validationToken,isValid, control) { ...}; //Set the validity for a specific element
    this.$setDirty = function() {...}; //Mark the form as having been modified
}
```
At this point, you may be asking why we're not simply using basic messaging with $broadcast and $emit to communicate between our directives. There are two basic reasons why we prefer controllers over messaging in this scenario:
1. First, idealistically, using controllers is significantly more modular. It allows us to separate out instance code that needs to be shared and available for cross-directive communication from the rest of the scope, while still keeping such methods available for convenient usage.
2. Secondly, more pragmatically, controllers are far easier to utilize for scenarios like this. Messaging is designed primarily for a shotgun notification approach, such that when an event happens, you broadcast it to all of the elements above or below your current node, and then continue on. Controllers, however, allow for a much more targeted communication style, calling methods on specific nodes as necessary, and while we didn't use them here, also allowing for easy callback integration. Doing the same sort of targeted communication via messaging requires significant extra logic on the part of the listeners to determine whether or not they're the intended node.

#### Creating our own controller communication
> __Wrapping our initialization process within setTimeout__ <br/>
> NOTE:- Undoubtedly, your first question is going to be about setTimeout , particularly one with no actual timeout. Because we're in the linking function, our $element is fully instantiated, so this sort of trickery shouldn't be necessary.
> And you're right, it isn't necessary. It is, however, a practice I recommend, for two primary reasons.
> First, on occasion, particularly if your directive or another on your element is applying a template, Angular and jQuery will both try to apply themselves at the same time and you run into a race condition.
> While this is rare, and usually means that your plugin isn't actually working on the $element itself, but trying to clone it or nest something inside, it still can cause a few headaches and this helps guard against that.
```javascript
setTimeout(function () {
    initialized = $element.timepicker()
    .on('changeTime', function (ev, ui) {
        var sec = $element.timepicker('getSecondsFromMidnight')
        ngModel.$setViewValue(sec * 1000);
    });
});
```
Wrapping our initialization process within setTimeout tells the JavaScript interpreter to process this after it's done with the current task,
