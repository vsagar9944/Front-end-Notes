------------------------------------------------------------------------------------------
Angular Modules
    angular.module("Name",[dependencies])   //setter method to declare module
    angular.module("Name")   //getter to get Name module

    Modules Properties
    1. name
    2. requires         The requires property contains a list of modules (as strings) that the injector loads before the module itself is loaded.

------------------------------------------------------------------------------------------
Scopes
    The scopes of the application refer to the application model. Scopes are the execution context for expressions. The $scope object is where we define the business functinality of the application, the methods in our controllers, and properties in the views.
    Scopes are the source of truth for the application state. Because of this live binding, we can rely on the $scope to update immediately when the view modifies it, and we can rely on the view to update when the $scope changes.
    $scope's in AngularJS are arranged in a hierarchical structure that mimics the DOM and thus are nestable: We can reference properties on parent $scopes .
    They give the developer the ability to propagate model changes throughout the application by using the apply mechanism available on the scope.
    This $scope object is the data model in Angular. All properties found on the $scope object are automatically accessible to the view.
    When Angular starts to run and generate the view, it will create a binding from the root ng-app element to the $rootScope . This $rootScope is the eventual parent of all $scope objects.


NOTE:-    
	Through Angular, we can use different types of markup in a template. These types include the following:
		• Directives: the attributes or elements that augment the existing DOM element into a reusable DOM component
		• Value bindings: the template syntax {{ }} binds expressions to the view
		• Filters: formatting functions that are available in the view
		• Form controls: user input validation controls

$scope Lifecycle
    Creation    -----> Linking    -----> Updating     ----->Destruction

    Creation
        When we create a controller or directive, Angular creates a new scope with the $injector and passes this new scope for the controller or directive at runtime.
    Linking
        When the $scope is linked to the view, all directives that create $scope s will register their watches on the parent scope. These watches watch for and propagate model changes from the view to the directive.
    Updating
        During the $digest cycle, which executes on the $rootScope , all of the children scopes will perform dirty digest checking. All of the watching expressions are checked for any changes, and the scope calls the listener callback when they are changed.
    Destruction
        When a $scope is no longer needed, the child scope creator will need to call scope.$destroy() to clean up the child scope.Note that when a scope is destroyed, the $destroy event will be broadcasted.

NOTE:- Directives, which are used all throughout our Angular apps, generally do not create their own $scopes but there are cases when they do. For instance, ng-controller and ng-repeat directives create their own child scopes and attach them to the DOM element.
------------------------------------------------------------------------------------------
CONTROLLERS
    The controller in AngularJS is a function that adds additional functionality to the scope of the view.
    Creating Controller
        app.controller('FirstController', function($scope) {
            $scope.message = "hello";
        });
    NOTE:- One major distinction between AngularJS and other JavaScript frameworks is that the controller is not the appropriate place to do any DOM manipulation or formatting, data manipulation, or state maintenance beyond holding the model data. It is simply the glue between the view and the $scope model.
    Angular uses scopes to isolate the functionality of the view, controllers, and directives.

CONTROLLER HIERARCHY (Scopes Within Scopes)  (scopeHirachy.html)
    Every part of an AngularJS application has a parent scope (as we’ve seen, at the ng-app level, this scope is called the $rootScope ), regardless of the context within which it is rendered.
    NOTE:-There is one exception: A scope created inside of a directive is called the isolate scope
    With the exception of isolate scopes, all scopes are created with prototypal inheritance, meaning that they have access to their parent scopes

------------------------------------------------------------------------------------------
EXPRESSIONS (parseExpression.html)
    The {{ }} notation for showing a variable attached to a $scope is actually an expression: {{ expression }}.
    Expressions are roughly similar to the result of an eval(javascript) Angular processes them; therefore, they have these important, distinct properties:
        • All expressions are executed in the context of the scope and have access to local $scope variables.
        • An expression doesn’t throw errors if it results in a TypeError or a ReferenceError.
        • They do not allow for any control flow functions (conditionals; e.g., if/else).
        • They can accept a filter and/or filter chains.
    • Parsing an Angular Expression
        Angular evaluates expressions by an internal service (called the $parse service) that has knowledge of the current scope. This setup gives us access to the raw JavaScript data and functions that are defined on our $scope .

---------------------------------------------------------------------------------------------
INTERPOLATING A STRING
    Interpolation allows us to live update a string of text based upon conditions of a scope, for instance.
    To run an interpolation on a string template, we need to inject the $interpolate service in our object.
    The $interpolate service takes up to three parameters, with only one required function.
        • text (string) - The text with markup to interpolate.
        • mustHaveExpression (boolean) - If we set parameter to true, then the text will return null if there is no expression.
        • trustedContext (string) - Angular sends the result of the interpolation context through the
        $sce.getTrusted() method, which provides strict contextual escaping.

    The $interpolate service returns an interpolation function that takes a context object against which the expressions are evaluated.
    If it’s desirable to use different beginning and ending symbols in our text, we can modify them by configuring the $interpolateProvider
-------------------------------------------------------------------------------------------------
FILTERS
    In AngularJS, a filter provides a way to format the data we display to the user. Angular gives us several built-in filters as well as an easy way to create our own
    • We invoke filters in our HTML with the | (pipe) character inside the template binding characters {{ }} .
        Example
            {{ name | uppercase }}
    • We can also use filters from within JavaScript by using the $filter service.
        Example
            app.controller('DemoController', ['$scope', '$filter',
                function($scope, $filter) {
                    $scope.name = $filter('lowercase')('Ari');
                }]);
    • To pass an argument to a filter in the HTML form, we pass it with a colon after the filter name (for multiple arguments, we can simply append a colon after each argument)
        Example
            {{ 123.456789 | number:2 }}

    Built in Filter
        • uppercase
        • lowercase
        • currency
        • date      :-date:'FORMAT'
        • number
        • filter        The filter filter selects a subset of items from an array of items and returns a new array
            The filter method takes a string, object, or function that it will run to select or reject array elements.
            1.string
                It will accept all elements that match against the string. If we want all the elements that do notmatch the string, we can prepend a ! to the string.
            2.object
                It will compare objects that have a property name that matches, as with the simple substring match if only a string is passed in. If we want to match against all properties, we can use the $ as the key.
            3.function
            It will run the function over each element of the array, and the results that return as non-falsy will appear in the new array.
            Example
                1.{{ ['Ari', 'Lerner', 'Likes', 'To', 'Eat', 'Pizza'] | filter:'e' }}
                2.{{ [{
                    'name': 'Ari',
                    'City': 'San Francisco',
                    'favorite food': 'Pizza'
                    }, {
                    'name': 'Nate',
                    'City': 'San Francisco',
                    'favorite food': 'indian food'
                    }] | filter:{'favorite food': 'Pizza'} }}
                3.{{ ['Ari', 'likes', 'to', 'travel'] | filter:isCapitalized }}     NOTE:-isCapitalized defined in controller.

        • orderBy
            The orderBy filter orders the specific array using an expression.
            The orderBy function can take two parameters: The first one is required, while the second is optional.
            The first parameter is the predicate used to determine the order of the sorted array.
        • limitTo
            The limitTo filter creates a new array or string that contains only the specified number of elements, either taken from the beginning or end, depending on whether the value is positive or negative.
            If the limit exceeds the value of the string, then the entire array or string will be returned.

    Making Our Own Filter       ->Filters are just functions to which we pass input
        angular.module('myApp', ['myApp.filters'])
        angular.module('myApp.filters', [])
            .filter('capitalize', function() {
            return function(input) {

            }
        });
        Using custom filter
            {{ 'ginger loves dog treats' | lowercase | capitalize }}

FORM VALIDATION
	Out of the box, AngularJS supports form validation with a mix of the HTML5 form validation inputs as well as with its own validation directives.
	1.To use form validations, we first must ensure that the form has a name associated with it, like in the above example.
	2.It is usually a great idea to use the novalidate flag on the form element, as it prevents the browser from natively validating the form.

	Let’s look at all the validation options we have that we can place on an input field:
		1.Required
		2.Minimum Length        <input type="text" ng-minlength=5 />
		3.Maximum Length        <input type="text" ng-maxlength=20 />
		4.Matches a Pattern     <input type="text" ng-pattern="/a-zA-Z/" />
		5.Email                 <input type="email" name="email" ng-model="user.email" />
		6.Number                <input type="number" name="age" ng-model="user.age" />
		7.URL                   <input type="url" name="homepage" ng-model="user.facebook_url" />

	CUSTOM VALIDATIONS
	angular.module('validationExample', []).directive('ensureUnique', function($http) {
		return {
			require: 'ngModel',
			link: function(scope, ele, attrs, c) {
				scope.$watch(attrs.ngModel, function() {
					$http({
						method: 'POST',
						url: '/api/check/' + attrs.ensureUnique,
						data: {
							'field': attrs.ensureUnique
						}
					}).success(function(data, status, headers, cfg) {
						c.$setValidity('unique', data.isUnique);
					}).error(function(data, status, headers, cfg) {
						c.$setValidity('unique', false);
					});
				});
			}
		}
	});
	Control Variables in Forms
	AngularJS makes properties available on the containing $scope object available to us as a result of setting a form inside the DOM.
	formName.inputFieldName.property
	Unmodified Form         formName.inputFieldName.$pristine
	Modified Form           formName.inputFieldName.$dirty
	Valid Form              formName.inputFieldName.$valid
	InValid Form            formName.inputFieldName.$invalid
	Errors                  formName.inputfieldName.$error

	A Little Style Never Hurts
	When AngularJS is handling a form, it adds specific classes to the form based upon the current state of the form
	These classes are:
	.ng-pristine {}
	.ng-dirty {}
	.ng-valid {}
	.ng-invalid {}

	$parsers            When our user interacts with the controller and the $setViewValue() method has been called on the ngModelController , the array of $parsers functions are called as a pipeline. The first $parser is called and passes its value to the next, and so on and so forth.
	$formatters         When the bound ngModel value has changed and has been run through the $parsers array, then the value will be passed through to the $formatters pipeline.These functions have the opportunity to modify and format the value, as well as change the validity state of the control similar to the $parsers array.

	NOTE:-Don’t forget to add a name to the input field. Adding a name to the input is important: That is how we’ll reference the form input when showing validation messages to the user.
-------------------------------------------------------------------------------------------------
Introduction to Directives
    COMPOSITION:- directives can be combined with other directives and attributes; this combination is called composition.
    NOTE:-A built-in directive is one that ships out of the box with Angular. All built-in directives are prefixed with the ng namespace. In order to avoid namespace collisions, do not prefix the name of your own directives with ng .

        1.Invoking a directive means to run the associated JavaScript that sits behind our directive
        2.we do not need to make a new custom element to declare our directive.
        3.Declaring a directive is the act of placing a function within our HTML as an element, attribute, class, or comment.
    CREATING DIRECTIVE
    Example  :-
    1.directive definition
        angular.module('myApp', []).directive('myDirective', function() {
            return {
                restrict: 'E',
                //replace: true,We can remove our custom element ( <my-directive> ) from the generated DOM completely and output only the link we’re providing to the template option
                template: '<a href="http://google.com">Click me to go to Google < /a>'
            }
        });
        NOTE:-The name of the directive should always be pascalCased, and the function we provide should return an object.
        In our case, we declare the directive in HTML using my-directive , the directive definition must be myDirective .
        we can specify that we want our directive to be invoked if it is an element ( E ), an attribute ( A ), a class ( C ), or a comment ( M ):
            restrict: 'EAC'
        NOTE:-Thus, a good rule of thumb to follow is to always declare our directive as an attribute  *****

        Declaring Our Directive with an Expression
            the valid ways of declaring an expression:
                <my-directive="someExpression">
                </my-directive>
                <div my-directive="someExpression">
                </div>
                <div class="my-directive:someExpression">
                </div>
                <!-- directive: my-directive someExpression -->

        CURRENT SCOPE INTRODUCTION
            ng-controller:-This directive exists for the purpose of creating a new child scope in the DOM.
            NOTE:-
            Be aware that there are other built-in directives, like ng-include and ng-view, that also create a new child scope, meaning they behave similar to ng-controller when invoked. We can even create a new child scope when building a custom directive of our own.

        Passing Data into a Directive
            template: '<a href="{{myUrl}}">{{myLinkText}}</a>'
        Using directive
            <div my-directive my-url="http://google.com" my-link-text="Click me to go to Google"> </div>

        In contrast to inherited scope (child scope), discussed earlier (in current scope intro- duction), an isolate scope is completely separate from the current scope of the DOM.In order to set properties on this fresh object, we’ll need explicitly pass in data via attributes, similar to the way we pass arguments into a method in JavaScript or Ruby.
        We’re creating what is referred to as an isolate scope. Essentially, that means the directive gets its own $scope object, which we can only use inside other methods of the directive or inside the directive’s template string
        To copy data from the DOM into the isolate scope of our directive, we use attributes:

            <div my-directive my-url="http://google.com" my-link-text="Click me to go to Google"></div>

            Example
            angular.module('myApp', []).directive('myDirective', function() {
                return {
                    restrict: 'A',
                    replace: true,
                    scope: {
                        myUrl: '@',
                        // binding strategy
                        myLinkText: '@' // binding strategy
                    },
                    template: '<a href="{{myUrl}}">{{myLinkText}}</a>'
                }
            }

        TWO-WAY BINDINGS



------------------------------------------------------------------------------------------------
BUILT-IN DIRECTIVES
    NOTE:- that all directives prefixed with the ng namespace are part of the built-in library of directives that ship with Angular. For this reason, never prefix directives you make with this namespace.
    1.Basic ng Attribute Directives
        • ng-href
        • ng-src
        • ng-disabled
        • ng-readonly
        • ng-checked
        • ng-selected
        • ng-class
        • ng-style
    2.Boolean Attributes
        • ng-disabled
        • ng-readonly
        • ng-checked
        • ng-selected
    3.Boolean-like Attributes
        • ng-href
        • ng-src
    4.Directives with Child Scope
        • ng-app
            Placing ng-app on any DOM element marks that element as the beginning of the $rootScope
            $rootScope is the beginning of the scope chain, and all directives nested under the ng-app in your HTML inherit from it. In your JavaScript code you can access the $rootScope via the run method:
        • ng-controller
            whose purpose is to provide a child scopes for the directives that are nested inside
            A child $scope is simply a JavaScript object that prototypically inherits methods and properties from its parent $scope (s), including the application’s $rootScope .
            Directives that are nested within an ng-controller have access to this new child $scope , but be mindful that the scoping rules for each directive do apply.
            Recall that the $scope object within a controller should be responsible for the actions and models shared by directives in the DOM.
                1.ACTION:-An action refers to a traditional JavaScript method on the $scope object.
                2.MODEL:-A model refers to a traditional JavaScript object {} where transient state should be stored. Persistent state should be bound to a service, which is then responsible for dealing with persisting that model.

            NOTE:-It’s important to not to set a value object (string, boolean, or number) directly on the $scope of a controller for a number of technological and architectural reasons. Data in the DOM should always use a . (dot). Following this rule will keep you out of unexpected trouble.

            NOTE:-JavaScript objects are either copy by value or copy by reference. String, Number, and Boolean are copy by value. Array, Object, and Function are copy by reference.

        • ng-include            Use ng-include to fetch, compile, and include an external HTML fragment into your current application
            Use the onload attribute within the same element to run an expression when the template is loaded.
            Keep in mind that when using ng-include , Angular automatically creates a new child scope. If you want to use a particular scope, for instance the scope of ControllerA , you must invoke the ng-controller="ControllerA" directive on the same DOM element itself; it will not be inherited from the surrounding scope like usual because a new scope is created when the template loads.
            Example
            <div ng-include="/myTemplateName.html" ng-controller="MyController" ng-init="name = 'World'"> Hello {{ name }} </div>

        • ng-switch
        • ng-repeat
        • ng-init
        • ng-view        The ng-view directive sets the view location in the HTML where the router will manage and place the view elements for different routes
        • ng-if            Use ng-if to completely remove or recreate an element in the DOM based on an expression.When an element is removed from the DOM using ng-if , its associated scope is destroyed
        • {{ }}             The {{ }} syntax is a templating syntax that’s built into Angular.
            Although it doesn’t look like a normal directive, it is, in fact, a shortcut for using ng-bind without needing to create an element; therefore, it is most commonly used with inline text.
        • ng-bind
            NOTE:When we use the {{ }} syntax, our HTML document loads the element and does not render it immediately, causing a “flash of unrendered content” (FOUC, for short). We can prevent this FOUC from being exposed by using ng-bind and binding our content to the element. The content will then render as the child text node of the element on which ng-bind is declared.
        • ng-cloak
            An alternative to using to using ng-bind to prevent a flash of unrendered content is to use ng-cloak on the element containing {{ }} :
            <body ng-init="greeting = 'Hello World'">
                <p ng-cloak>{{ greeting }}</p>
            </body>
        • ng-bind-template
        • ng-model
        • ng-show
        • ng-hide
        • ng-change
        • ng-form
        • ng-click
        • ng-class
        • ng-attr-(suffix)
--------------------------------------------------------------------------------------------
DIRECTIVES EXPLAINED
    The simplest way to think about a directive is that it is simply a function that we run on a particular DOM element. The function is expected to provide extra functionality on the element.
    Defining Directives
        angular.module('myApp')
            .directive('myDirective',
                function($timeout, UserDefinedService) {
            // directive definition goes here
        });
        factory_function:-The factory function returns an object that defines how the directive behaves

        The factory function we define for a directive is only invoked once, when the compiler matches the directive the first time.
        we invoke a directive’s factory function using $injector.invoke.
        1.When Angular encounters the named directive in the DOM, it will invoke the directive definition we’ve registered, using the name to look up the object we’ve registered. At this point, the directive lifecycle begins,

        Possible Options for Directive Definition Object as shown below.
        {
            restrict: String,
            priority: Number,
            terminal: Boolean,
            template: String or Template Function:
            function(tElement, tAttrs) (...},
            templateUrl: String,
            replace: Boolean or String,
            scope: Boolean or Object,
            transclude: Boolean,
            controller: String or
            function(scope, element, attrs, transclude, otherInjectables) { ... },
            controllerAs: String,
            require: String,
            link: function(scope, iElement, iAttrs) { ... },
            compile: return an Object OR
            function(tElement, tAttrs, transclude) {
                return {
                    pre: function(scope, iElement, iAttrs, controller) { ... },
                    post: function(scope, iElement, iAttrs, controller) { ... }
                }
                // or
                return function postLink(...) { ... }
            }
        };

        Restrict:- It is an optional argument. It is responsible for telling angular in which format our directive will be declared in DOM.
            Default restrict option is set to "A"---attribute,avilable options are E(element)A(attribute)C(class)M(coment)
        Priority:(Number)(default 0)
            If an element is decorated with two directives that have the same priority, then the first directive declared on the element will be invoked first
            NOTE:- ngRepeat has highest priority on built-in directives
        Terminal (boolean) :-We use the terminal option to tell Angular to stop invoking any further directives on an element that have a higher priority. All directives with the same priority will be executed, however.
        Template (string|function)  (Optional)
            If provided it must set to either
            1.html string
            2. function that takes two arguments (tElement,tAttr)   nd returns a string value representing the template.

            NOTE:- When a template string contains more than one DOM element or only a single text node, it must be wrapped in a parent element. In other words, a root DOM element must exist:
        TemplateUrl (string|function)
            templateUrl is optional. If provided, it must be set to either:
                • the path to an HTML file, as a string
                • a function that takes two arguments: tElement and tAttrs . The function must return the path to an HTML file as a string.
        replace (boolean)
        Scope Option:
            Optional,Default false,When scope is set to true, a new scope object is created that prototypically inherits from its parent scope
            If multiple directives on an element provide an isolate scope, only one new scope is applied. Root elements within the template of a directive always get a new scope; thus, for those objects, scope is set to true by default.
            NOTE:- Isolate Scope:-To create a directive with isolate scope we’ll need to set the scope property of the directive to an empty object, {} . Once we’ve done that, no outer scope is available in the template of the directive:
        Transclude
            transclude is optional. If provided, it must be set to true . It is set to false by default.
        Controller (string|function)
            NOTE:-The main use case for a controller is when we want to provide reusable behavior between directives. As the link function is only available inside the current directive, any behavior defined within is not shareable.
            The link function provides isolation between directives, while the controller defines shareable behavior.
            Using the controller option is good when we want to expose an API to other directives; otherwise, we can rely on link to provide us local functionality for the directive element
            NOTE:-Use the scope argument passed into the link function when expecting to interact with the instance of the scope on screen.
        ControllerAs (string)
            This option may seem trivial, but it gives us a lot of power in how we can use and create anonymous controllers in our routes and directives. That power allows us to create dynamic objects as controllers that are isolated and easy to test.
        require(string/Array):-
            The require option can be set to a string or an array of strings. The string(s) contain the name of another directive. require is used to inject the controller of the required directive as the fourth parameter of the current directive’s linking function.
            (For more detials refer page no 122 of ng-book)
        link:- We use link function to create directive that transforms DOM.
            The link function is optional. If the compile function is defined, it returns the link function; therefore, the compile     function will overwrite the link function when both are defined.
        Directive Scope

        Example of directive definition
            1.angular.module('myApp' [])
                .directive('myDirective', function() {
                    return {
                        pre: function(tElement, tAttrs, transclude) {
                            // executed before child elements are linked
                            // NOT safe to do DOM transformations here b/c the `link` function
                            // called afterward will fail to locate the elements for linking
                        },
                        post: function(scope, iElement, iAttrs, controller) {
                            // executed after the child elements are linked
                            // IS safe to do DOM transformations here
                            // same as the link function, if the compile option here we're
                            // omitted
                        }
                    }
                });
            2.angular.module('myApp' [])
                .directive('myDirective', function() {
                    return {
                        link:function(){
                            return {
                                pre: function(tElement, tAttrs, transclude) {
                                    // executed before child elements are linked
                                    // NOT safe to do DOM transformations here b/c the `link` function
                                    // called afterward will fail to locate the elements for linking
                                },
                                post: function(scope, iElement, iAttrs, controller) {
                                    // executed after the child elements are linked
                                    // IS safe to do DOM transformations here
                                    // same as the link function, if the compile option here we're
                                    // omitted
                                }
                            }
                        }
                    }
                });

        NOTE:- Example 1 and 2 both are functionaly eqquvalent.

    Link function has following signature
        link: function(scope,element,attrs){

        }
    If the directive definition has requre option then this method signature gain fourth parameter as Controller or Controllers of required directives.

-----------------------------------------------------------------------------------------------
AngularJS Life Cycle
    1.Compile Phase
        NOTE:-only the template belonging to the higest-priority directive will be parsed and added to the tree of templates.
        -> TEMPLATE FUNCTION Once a directive and its child templates have been walked or compiled, a function is returned for the compiled template known as the template functionBefore the template function for a directive is returned, however, we have the opportunity to modify the compiled DOM tree.
        -> Finally, the template function is passed to the link function, where scope, determined by the directive definition rules of each directive in the compiled DOM tree, is applied all at once.
        The compile option can return an object or a function.
        NOTE:-Understanding the compile vs link option is one of the more advanced topics we’ll run across in Angular, and it provides us with considerable context about how Angular really works.
        -> The compile option and the link option are mutually exclusive. If both are set, then the compile option will be expected to return the link function, while the link option will simply be ignored.
        -> The template instance and link instance may be different objects if the template has been cloned. Thus, we can only do DOM transformations that are safe to do to all cloned DOM nodes within the compile function. Don’t do DOM listener registration: That should be done in the linking function.

        NOTE:-THE COMPILE FUNCTION DEALS WITH TRANSFORMING THE TEMPLATE DOM.
        NOTE:-THE LINK FUNCTION DEALS WITH LINKING SCOPE TO THE DOM

    2. Link
        link:- We use link function to create directive that transforms DOM.
            The link function is optional. If the compile function is defined, it returns the link function; therefore, the compile     function will overwrite the link function when both are defined.
        Example of directive definition
            1.angular.module('myApp' [])
                .directive('myDirective', function() {
                    return {
                        pre: function(tElement, tAttrs, transclude) {
                            // executed before child elements are linked
                            // NOT safe to do DOM transformations here b/c the `link` function
                            // called afterward will fail to locate the elements for linking
                        },
                        post: function(scope, iElement, iAttrs, controller) {
                            // executed after the child elements are linked
                            // IS safe to do DOM transformations here
                            // same as the link function, if the compile option here we're
                            // omitted
                        }
                    }
                });
            2.angular.module('myApp' [])
                .directive('myDirective', function() {
                    return {
                        link:function(){
                            return {
                                pre: function(tElement, tAttrs, transclude) {
                                    // executed before child elements are linked
                                    // NOT safe to do DOM transformations here b/c the `link` function
                                    // called afterward will fail to locate the elements for linking
                                },
                                post: function(scope, iElement, iAttrs, controller) {
                                    // executed after the child elements are linked
                                    // IS safe to do DOM transformations here
                                    // same as the link function, if the compile option here we're
                                    // omitted
                                }
                            }
                        }
                    }
                });

        NOTE:- Example 1 and 2 both are functionaly eqquvalent.

        Link function has following signature
            link: function(scope,element,attrs){

            }
        If the directive definition has requre option then this method signature gain fourth parameter as Controller or Controllers of required directives.

-----------------------------------------------------------------------------------------------
ngModel
    When we use the ngModel attribute from within a directive, it will get access to a special API that deals with data binding, validations, CSS updates, and other things that don’t deal with the actual DOM.
    The ngModel controller, which is injected along with ngModel when we use it in our directive, contains several methods. In order to gain access to this ngModelController , we must use the require option (as we see above):Directives Explained

        angular.module('myApp')
            .directive('myDirective', function() {
                return {
                    require: '?ngModel',
                    link: function(scope, ele, attrs, ngModel) {
                        if (!ngModel) return;
                            // Now we have a hold of the
                            // ngModelController instance
                            // inside of our directive
                    }
                }
            });
        NOTE:-
            Without passing the require option, the ngModelController will not be passed into our directive.
            Notice that this directive does not have an isolated scope. If we do set the directive to have the isolate scope, then the ngModel value will not update the outer ngModel value: Angular looks up this value outside of the local scope.
            In order to set the view value of a scope, we must call the API function ngModel.$setViewValue() . The $setViewValue() function takes a single argument:
            Note that simply by calling $setViewValue() alone does not invoke a new digest cycle, so even after we set the $viewValue , we need to trigger a digest when we want the directive to update.

    CUSTOMRENDERING
        It is possible to define how the view actually get rendered by defining $render  method in controller.This method will be applied after the $parser pipelinehas completed

    ngModelController Properties
        $viewValue:-
        $modelValue:-
        $parsers:- value is an array of functions that get executed in pipeline.These are used to sanitize and modify the value.
        $formatters:-value is an array of functions that get executed in pipeline when model value chages.
        $viewChangeListners:-
        $error
        $pristine
        $dirty
        $valid
        $invalid

-----------------------------------------------------------------------------------------------
Angular Module Loading
    Angular modules, themselves, have the opportunity to configure themselves before the module actually bootstraps and starts to run. We can apply different sets of logic during the bootstrap phase of the app.

    Configuration
        Angular executes blocks of configuration during the provider registration and configuration phases in the bootstrapping of the module. This phase is the only part of the Angular flow that may be modified before the app starts up.

        angular.module('myApp', [])
            .config(function($provide) {
        });

        Throughout this book, we use methods that are syntactic sugar around the .config() function and get executed at configuration time. For instance, when we create a factory or a directive on top of the module:
            angular.module('myApp', [])
                .factory('myFactory', function() {
                    var service = {};
                        return service;
                })
                .directive('myDirective', function() {
                    return {
                        template: '<button>Click me</button>'
                    }
                })
        Angular executes these helper functions at compile time. They are functionally equivalent to:Angular Module Loading
            angular.module('myApp', [])
                .config(function($provide, $compileProvider) {
                    $provide.factory('myFactory', function() {
                        var service = {};
                        return service;
                    });
                    $compileProvider.directive('myDirective',
                        function() {
                            return {
                                template: '<button>Click me</button>'
                            }
                    });
                });

        NOTE:-
            1.In particular, it’s also important to note that Angular runs these functions in the order in which they are written and registered. That is to say that we cannot inject a provider that has not yet been defined.
            2.The only exception to the rule of in-order definitions is the constant() method. We always place these at the beginning of all configuration blocks.
            3.When writing configuration for a module, it’s important to note that there are only a few types of objects that we can inject into the .config() function: providers and constants.
            4.We can also define multiple configuration blocks, which are executed in order and allow us to focus our configuration in the different phases of the app.
            5.configFunction (function) The function that Angular executes on module load.

    Run Blocks
        Unlike the configuration blocks, run blocks are executed after the injector is created and are the first methods that are executed in any Angular app.
        Typically, these run blocks are places where we’ll set up event listeners that should happen at the global scale of the app. For example, we’ll use the .run() block to set up listeners for routing events or unauthenticated requests.
        initializeFn (function) Angular executes this function after it creates the injector.
-----------------------------------------------------------------------------------------------
Multiple View and Routing
    angular-route.js

    Referencing ngRoute in our app module
        angular.module('App',["ngRoute"]);
    Layout Template
        To make a layout template, we need to change the HTML in order to tell AngularJS where to render the template. Using the ng-view directive in combination with the router, we can specify exactly where in the DOM we want to place the rendered template of the current route.

        For instance, a layout template might look something like:
        <header>
            <h1>Header</h1>
        </header>
        <div class="content">
            <div ng-view></div>
        </div>
        <footer>
            <h5>Footer</h5>
        </footer>

        NOTE:-
            1.The ng-view directive is a special directive that’s included with the ngRoute module. Its specific responsibility is to stand as a placeholder for $route view content. It creates its own scope and nests the template inside of it.
            2.The ng-view directive is a terminal directive at a 1000 priority. Angular will not run any directives on the element at a lower priority, which is most directives (i.e., all other directives on the <div ng-view></div> element are meaningless).

            The ngView directive follows these specific steps:
                • Any time the $routeChangeSuccess event is fired, the view will update
                • If there is a template associated with the current route:
                    – Create a new scope
                    – Remove the last view, which cleans up the last scope
                    – Link the new scope and the new template
                    – Link the controller to the scope, if specified in the routes
                    – Emits the $viewContentLoaded event
                    – Run the onload attribute function, if provided

    Route
        We use one of two methods to declare all application routes in AngularJS: the when method and the otherwise method.
        To create a route on a specific module or app, we use the config function.
            angular.module('myApp', []).
            config(['$routeProvider', function($routeProvider) {

            }]);

            Now, to add a specific route, we can use the when method. This method takes two parameters ( when(path, route) ).This block shows how can create a single route:
                angular.module('myApp', []).
                    config(['$routeProvider', function($routeProvider) {
                        $routeProvider
                            .when('/', {
                                templateUrl: 'views/home.html',
                                controller: 'HomeController'
                            })
                            .otherwise({        //Catch all that redirect to '/' path
                                redirectTo: '/'
                            });
                        }]);
                NOTE:-The configuration object properties that we can set are controller, template, templateURL, resolve, redirectTo, and reloadOnSearch.



-----------------------------------------------------------------------------------------------
Routing Modes
    1.Hashbang Mode
    2.HTML5 Mode

Routing Events
    To set up an event listener to listen for routing events, we use the $rootScope to listen for the event.
        1.$routeChangeStart
            The $routeChangeStart event fires with two parameters:
                • The next URL to which we are attempting to navigate
                • The URL that we are on before the route change
            Example
            angular.module('myApp', [])
            .run(['$rootScope', '$location', function($rootScope, $location) {
                $rootScope.$on('$routeChangeStart',
                    function(evt, next, current) {
                    })
                }])
        2.$routeChangeSuccess
            Angular broadcasts the $routeChangeSuccess event after the route dependencies have been re- solved.
            The $routeChangeSuccess event fires with three parameters:
                • The raw Angular evt object
                • The route where the user currently is
                • The previous route (or undefined if the current route is the first route)
            Example
            $rootScope.$on('$routeChangeSuccess', function(evt, next, previous) {
            })
        3.$routeChangeError
            Angular broadcasts the $routeChangeError event if any of the promises are rejected or fail.
                angular.module('myApp', [])
                    .run(['$rootScope', '$location', function($rootScope, $location) {
                        $rootScope.$on('$routeChangeError', function(current, previous, rejection) {
                        })
                    }])
            The $routeChangeError event fires with three parameters:
                • The current route information
                • The previous route information
                • The rejection promise error
        4.$routeUpdate
            Angular broadcasts the $routeUpdate event if the reloadOnSearch property has been set to false and we’re reusing the same instance of a controller.

Other Advanced Routing Topics
    Page Reloading
        The $location service does not reload the entire page; it simply changes the URL. If we need to cause a full page reload, we have to set the location using the $window service: $window.location.href = "/reload/page";
    Async Location Changes

-----------------------------------------------------------------------------------------------
Dependency Injection
    Dependency injection is a design pattern that allows for the removal of hard-coded dependencies, thus making it possible to remove or change them at run time.
    Angular uses the $injector for managing lookups and instantiation of dependencies for this reason.
    When any of our modules boot up at run time, the injector is responsible for actually instantiating the instance of the object and passing in any of its required dependencies.

    AngularJS uses an annotate function to pull properties off of the passed-in array during instantia- tion. You can view this function by typing the following in the Chrome developer tools
    In every Angular app, the $injector has been at work, whether we know it or not. When we write a controller without the [] bracket notation or through explicitly setting them, the $injector will infer the dependencies based on the name of the arguments.

    Annotation by Inference
        Angular assumes that the function parameter names are the names of the dependencies, if not otherwise specified. Thus, it will call toString() on the function, parse and extract the function arguments, and then use the $injector to inject these arguments into the instantiation of the object. The injection process looks like:
            injector.invoke(function($http, greeter) {});
        Note that this process will only work with non-minified, non-obfuscated code, as Angular needs to parse the arguments intact.         With this JavaScript inference, order is not important: Angular will figure it out for us and inject the right properties in the “right” order.
    Explicit Annotation
        Angular provides a method for us to explicitly define the dependencies that a function needs upon invocation. This method allows for minifiers to rename the function parameters and still be able to inject the proper services into the function.
        The injection process uses the $inject property to annotation the function. The $inject property of a function is an array of service names to inject as dependencies.
        To use the $inject property method, we set it on the function or name.
        NOTE:-With this annotation style, order is important, as the $inject array must match the ordering of the arguments to inject. This method of injection does work with minification, because the annotation information will be packaged with the function.
    Inline Annotation
        Inline annotation allows us to pass an array of arguments instead of a function when defining an Angular object. The elements inside this array are the list of injectable dependencies as strings, the last argument being the function definition of the object
        Example
            angular.module('myApp')
                .controller('MyController',['$scope', 'greeter',function($scope, greeter) {
                }]);
        The inline annotation method works with minifiers, as we are passing a list of strings. We often refer this method as the bracket or array notation []

    $inject API
        1.annotate()
            The annotate() function returns an array of service names that are to be injected into the function when instantiated. The annotate() function is used by the injector to determine which services will be injected into the function at invocation time.
                The annotate() function takes a single argument:
                • fn (function or array)
                The fn argument is either given a function or an array in the bracket notation of a function definition.
                The annotate() method returns a single array of the names of services that will be injected into the function at the time of invocation.
                    var injector = angular.injector(['ng', 'myApp']);
                    injector.annotate(function($q, greeter) {});
        2.get()
            The name argument is the name of the instance we want to get. get() returns an instance of the service by name .
        3.has()
            The has() method returns true if the injector knows that a service exists in its registry and false if it does not. It takes a single argument:
                • name (string)
            The string is the name of the service we want to look up in the injector’s registry.
        4.instantiate()
        5.invoke()
            The invoke() method invokes the method and adds the method arguments from the $injector
            The invoke() method returns the value that the fn function returns.
            This invoke() method takes three arguments:
                • fn (function)     This function is the one to invoke. The arguments for the function are set with the function annotation.
                • self (object – optional)  The self argument allows for us to set the this argument for the invoke method.
                • locals (object – optional)        This optional argument provides another way to pass argument names into the function when it is invoked.

    ngMin *******
-----------------------------------------------------------------------------------------------
SERVICES
    For memory and performance purposes, controllers are instantiated only when they are needed and discarded when they are not. That   means that every time we switch a route or reload a view, the current controller gets cleaned up by Angular.
    Services provide a method for us to keep data around for the lifetime of the app and communicate across controllers in a consistent manner.
    Services are singleton objects that are instantiated only once per app (by the $injector ) and lazy- loaded (created only when necessary). They provide an interface to keep together those methods that relate to a specific function.

    Registering a Service
        1.The most common and flexible way to create a service uses the angular.module API factory
            angular.module('myApp.services', [])
                .factory('githubService', function() {
                    var serviceInstance = {};
                    // Our first service
                    return serviceInstance;
                });
    Using Services
    NOTE:-It’s conventional to inject any Angular services before our own custom services.
    NOTE:-To share data across controllers, we need to add a method to our service that stores the username. Remember, the service is a singleton service that lives for the lifetime of the app, so we can store the username safely inside of it.

    Options for Creating Services
        The five different methods for creating services are:
            • factory()
                The factory() function takes two arguments:
                    • name (string)
                    • getFn (function)
                    Example
                        angular.module('myApp')
                            .factory('myService', function() {
                                return {
                                    'username': 'auser'
                                    }
                            });
                    NOTE:-The getFn function can return anything from a primitive value to a function to an object
            • service()
                The service() method takes two arguments:
                    • name (string)
                    • constructor (function)
            • provider()
                1.These factories are all created through the $provide service, which is responsible for instantiating these providers at run time.
                2.A provider is an object with a $get() method. The $injector calls the $get method to create a new instance of the service. The $provider exposes several different API methods for creating a service, each with a different intended use.
                3.At the root of all the methods for creating a service is the provider method. The provider() method is responsible for registering services in the $providerCache .
                4.The two method calls are functionally equivalent and will create the same service.Services
                    angular.module('myApp')
                        .factory('myService', function() {
                            return {
                                'username': 'auser'
                                }
                        })
                        // This is equivalent to the
                        // above use of factory
                        .provider('myService', {
                            $get: function() {
                                return {
                                    'username': 'auser'
                                    }
                                }
                            });
                5.Why would we ever need to use the .provider() method when we can just use the .factory() method?
                    The answer lies in whether we need the ability to externally configure a service returned by the .provider() method using the Angular .config() function. Unlike the other methods of service creation, we can inject a special attribute into the config() method.

                    NOTE:-If we want to be able to configure the service in the config() function, we must use provider() to define our service.
                    For instance, if we define a service as githubService , then the provider will be available as githubServiceProvider .
                    The aProvider argument can take a few different forms:(Second argument to provider() method)
                        If the aProvider argument is a function, then the function is called through dependency injection and is responsible for returning an object with the $get method.
                        If the aProvider argument is an array, then it’s treated just like a function with inline dependency injection annotation. It will expect the last argument to be a function that returns an object with the $get method.
                        If the aProvider argument is an object, then it is expected to have a $get method. The provider() function returns an object that is a registered provider instance.
                    Using .provide() is very powerful and gives us the ability to use and share our services across our applications.
            • constant()
            • value()
                NOTE:-The major difference between the value() method and the constant() method is that you can inject a constant into a config function, whereas you cannot inject a value.
                Typically, a good rule of thumb is that we should use value() to register a service object or function, while we should use constant() for configuration data.
            • decorator()
                In fact, a lot of the core Angular testing functionality is built using $provide.decorator() .

-----------------------------------------------------------------------------------------------
Architecture
    1.Every Angular object should have its own file, named appropriately for its function.
    Modules     A module is the kernel of functionality for Angular apps.
        1.Modularize on Functionality
            We need to inject these modules as dependencies for our main app, which makes it incredibly easy to set up tests for each module type and also isolates and subdivides the functionality that we’ll need to account for when writing specs.
                angular.module('myApp.directives', []);
                angular.module('myApp.services', []);
                angular.module('myApp.filters', []);
        2.Modularize on Routes
            Another method we can use to break up our app is to divide our modules by route
                angular.module('myApp.home', []);
                angular.module('myApp.login', []);
                angular.module('myApp.account', []);
    Controllers
        Scope creep can be one of the most confusing aspects of the Angular framework where we have a lot of functionality defined on our $scope in our controllers.
        Sharing Data between Controllers

    Directives
-----------------------------------------------------------------------------------------------
Communicating with the Outside World: XHR and Server-Side Communication
    1.Using $http
        The $http service is simply a wrapper around the browser’s raw XMLHttpRequest object.
        The $http service is a function that takes a single argument: a configuration object that is used to generate a HTTP request. The function returns a promise that has two helper methods: success and error .
        Example
            $http({
                method: 'GET',
                url: '/api/users.json'
                }).success(function(data, status, headers, config) {
                    // This is called when the response is
                    // ready
                }).error(function(data, status, headers, config) {
                    // This is called when the response
                    // comes back with an error status
                });
            1.The method actually returns a promise .
            2.We’ll use this technique quite often when we build services so that they can return a promise instead of requiring a callback.
            3.If the response status code is between 200 and 299, the response is considered successful, and the success callback will be called. Otherwise, the error callback will be invoked.
            4.The difference between using the then() method and the convenience helpers is that the success() and error() functions contain a destructured representation of the response object, which the then() method receives in whole.
            5.When we call the $http method, it won’t actually execute until the next $digest loop runs. Although most of the time we’ll be calling $http inside of an $apply block, we can also execute the method outside of the Angular digest loop.
            To execute an $http function outside of the $digest loop, we need to wrap it in an $apply block. That will force the digest loop to run, and our promises will resolve as we expect them to.
                $scope.$apply(function() {
                    $http({
                        method: 'GET',
                        url: '/api/users.json'
                    });
                });
        Shortcut Methods
            The $http service also provides handy methods that allow us to shorten any method calls that don’t require more customization than a URL and a method name
            1.$http.get('/api/users.json',config);
                config:-This object is an optional configuration object
                The get() method returns a HttpPromise object.
            2.$http.delete()
            3.$http.head()
            4.$http.jsonp()     In order to send the JSONP request, it must contain the string JSON_CALLBACK. For instance
                $http.jsonp("/api/users.json?callback=JSON_CALLBACK");
            5.$http.put()
            6.$http.post()
        Configuration Object
            var config = {};
            config.method   = ‘GET’, ‘DELETE’, ‘HEAD’, ‘JSONP’, ‘POST’, ‘PUT’.
            config.url  = "url"
            config.params  = {"name": "ari"}
            config.data  = {"name": "ari"}
            config.headers  = {"name": "ari"}
            config.xsrfHeaderName  = This string is the name of the HTTP header to populate with the XSRF token
            config.xsrfCookieName  = This string contains the name of the cookie that holds the XSRF token.
            config.transformRequest  = This function or array of functions takes the HTTP request body and headers and returns their transformed versions. We generally use it to serialize data before it is sent to the server.
            config.cache = If this boolean value is true, then the default $http cache will cache the GET request. If it is a cache object built with $cacheFactory , then Angular will use this new cache to cache the response.
            config.transformResponse = This function or array of functions takes the HTTP response body and headers and returns their transformed versions. We generally use it to deserialize data after it has received returned data.
            config.timeout = This key is a timeout in milliseconds or a promise that should abort the request when the promise is resolved.
            config.withCredentials = If this boolean value is true, then the withCredentials flag on the XHR request object will be set. By default, CORS requests will not set any cookies. The withCredentials flag sets a custom header called Access-Control-Allow-Credentials , which makes the request with any cookies from the remote domain in the request.
            config.responseType = (String)It can be set to one of the different types of available HTTP response types:
                •”” (string – default)
                •“arraybuffer” (ArrayBuffer)
                •“blob” (blob object)
                •“document” (HTTP document)
                •“json” (JSON object parsed from a JSON string)
                •“text” (string)
                •“moz-chunked-text” (streaming text)
                •“moz-blob” (Firefox to receive progress events)
                •“moz-chunked-arraybuffer” (streaming ArrayBuffer)

    Response Object
        The response object that Angular passes to the then() method contains four properties:
        • data (string/object)      This data represents the transformed response body (if any transformations are defined).
            • status (number)           This number is the HTTP status code of the response.
            • headers (function)        This function is the header getter function that takes a single parameter to get the header value for the header name.
            • config (object)           This object is the full, generated configuration object that was used to generate the original request.
    Caching HTTP Requests
        By default, the $http service does not cache requests in a local cache. We can enable caching per request by passing either a boolean or a cache instance in our $http requests.
            $http.get('/api/users.json', { cache: true })
                .success(function(data) {})
                .error(function(data) {});
            1.By passing true , for this particular request, Angular will use the default cache using the $cacheFactory , which Angular creates for us automatically at bootstrap time.
            2.We can set a default cache for all $http requests across our application in the .config() function of our app:
                angular.module('myApp')
                    .config(function($httpProvider, $cacheFactory) {
                        $httpProvider.defaults.cache =
                            $cacheFactory('lru', {
                                capacity: 20
                            });
                        });
    Interceptors
        Anytime that we want to provide global functionality on all of our requests, such as authentication, error handling, etc., it’s useful to be able to provide the ability to intercept all requests before they pass to the server and back from the server.
        At their core, interceptors are service factories (see services for more information about services) that we register through the $httpProvider by adding them to the $httpProvider.interceptors array.

        There are four types of interceptors – two success interceptors and two rejection interceptors:
            • request       Angular calls the request interceptor with the $http config object. It can modify the config object or create a new one, and it is responsible for returning the updated config object or a promise that resolves a new config object.
            • response      Angular calls the response interceptor with the $http response object. The function can modify the response or create a new one. It’s responsible for returning the response or a promise that resolves a new response object.
            • requestError Angular calls this interceptor when the previous request interceptor throws an error or is resolved with a rejection.
            • responseError We receive this error when the previous respone interceptor throws an error or is resolved with a rejection.
        To create an interceptor, we use the .factory() method on our module and add one or more of the four methods to our service.
            Example
                function($q) {
                    var interceptor = {
                        'request': function(config) {
                            // Successful request method
                            return config; // or $q.when(config);
                        },
                        'response': function(response) {
                            // successful response
                            return response; // or $q.when(config);
                        },
                        'requestError': function(rejection) {
                            // an error happened on the request
                            // if we can recover from the error
                            // we can return a new request
                            // or promise
                            return response; // or new promise
                            // Otherwise, we can reject the next
                            // by returning a rejection
                            // return $q.reject(rejection);
                        },
                        'responseError': function(rejection) {
                            // an error happened on the request
                            // if we can recover from the error
                            // we can return a new response
                            // or promise
                            return rejection; // or new promise
                            // Otherwise, we can reject the next
                            // by returning a rejection
                            // return $q.reject(rejection);
                        }
                    };
                    return interceptor;
                });
        We then need to register our interceptor with the $httpProvider in a .config() function:
            angular.module('myApp')
            .config(function($httpProvider) {
                $httpProvider.interceptors.push('myInterceptor');
            });

    Configuring the $httpProvider
        1.Using the .config() option, we can add certain HTTP headers to every request, which is useful when we want to send authentication headers alongside every request or set a response type, etc
        2.We can also manipulate these defaults at run time using the defaults property of the $http object.

-----------------------------------------------------------------------------------------------
    Using $resource
        1.This service creates a resource object that allows us to intelligently work with RESTful server-side data sources;
        2.The $resource service allows us to turn our $http requests into simple methods like save or update . Rather than requiring us to be repetitive or to write tedious code, we can use the $resource method to handle it for us.
        Installation
            ngResource      angular-resource.js
            angular.module('myApp', ['ngResource']);
        Using $resource
            The $resource service itself is a factory that creates a resource object. The returned $resource object has methods that provide a high-level API to interact with back-end servers.
            var User = $resource('/api/users/:userId.json',
                    {
                        userId: '@id'
                    }
                );
            $resource returns a resource class object with a few methods for default actions.
            By default, this object generates five methods that allow us to interact with a collection of resources or to generate an instance of a resource object. It creates two methods that are HTTP GET-based methods and three that are non-GET methods.
                HTTP GET Methods        get(params, successFn, errorFn),query(params, successFn, errorFn)
                    The two HTTP GET methods it creates expect the following three parameters:
                        • params (object)           These are the parameters that are sent with the request. They can be named parameters in the URL, or they can be query parameters.
                        • successFn (function)      This function is the callback function that will be called upon a successful HTTP response.
                        • errorFn (function)        This callback function is called upon a non-successful HTTP response.
                    NOTE:-The only major difference between the query() method and the get() method is that Angular expects the query() method to return an array.
                HTTP Non-GET Methods
                    The three HTTP non-GET methods it creates expect the following four parameters:
                        • params (object) These are the parameters that are sent with the request. They can be named parameters in the URL, or they can be query parameters.
                        • postData (object) This object is the payload sent with the request.
                        • successFn (function) This callback function is called upon a successful HTTP response.
                        • errorFn (function) This callback function is called upon a non-successful HTTP response.
                    save(params, payload, successFn, errorFn)
                    delete(params, payload, successFn, errorFn)
                    remove(params, payload, successFn, errorFn)

                    NOTE:-The remove method is synonymous with the delete() method and primarily exists because delete is a reserved word in JavaScript that can cause problems when we use in Internet Explorer.

        $resource instances
            The three instance methods that are available on the instance objects are:
                • $save()
                • $remove()
                • $delete()
            $resource Instances Are Asynchronous

    Additional Properties
        The $resource collections and instances have two special properties that enable us to interact with the underlying resource definitions.
        • $promise (promise)
        • $resolved (boolean)
    Custom $resource Methods

-----------------------------------------------------------------------------------------------
2.prototypical inheritance
3.binding strategies explained
4.We can only use ng-app once per document. If we want to place multiple apps in a page, we’ll need to manually bootstrap our applications. We will talk more about manually bootstrapping apps in the under the hood chapter.
5.$getTrustedResourceUrl
6.$templateCache
7.Two way data-binding
8. Directives linking function
9.$timeout service
10.Representational state transfer, or REST for short
