ng-repeat

* When we use the ng-repeat directive over an object instead of an array, the keys of the object will be sorted in a case-sensitive, alphabetic order.That is, uppercase first, and then sorted by alphabet.
* The ng-repeat directive takes an argument in the form variable in arrayExpression or (key, value) in objectExpression
### Helper Variables in ng-repeat
    * {{$first}}
    * {{$middle}}
    * {{$last}}
    * {{$index}}
    * {{$even}}
    * {{$odd}}
    * {{note.$$hashKey}}
    
    Each of the $ prefixed variables we use within the context of the ng-repeat are provided
by AngularJS, and refer to the state of the repeater for that particular element.

* __ng-repeat-start__ and __ng-repeat-end__ directives

# Working with Forms

1. it is also recom‐mended to structure your model and bindings in such a way to reduce your own effort

# Form Validation and States
1. When you use forms (and give them names), AngularJS creates a FormController that
holds the current state of the form as well as some helper methods.

## Form states in AngularJS
| Form state    | Description   |
| ------------- | ------------- |
| __$invalid__  | AngularJS sets this state when any of the validations ( required , ng-minlength , and others) mark any of the fields within the form as invalid.  |
| __$valid__    | The inverse of the previous state, which states that all the validations in the form are currently evaluating to correct. |
| __$pristine__ | All forms in AngularJS start with this state. This allows you to figure out if a user has started typing in and modifying any of the form elements. Possible usage: disabling the reset button if a form is pristine. |
| __$dirty__    | The inverse of $pristine , which states that the user made some changes (he can revert it, but the $dirty bit is set). |
| __$error__    | This field on the form houses all the individual fields and the errors on each form element. We will talk more about this in the following section. |

>Each of the states mentioned in the table (except $error ) are Booleans and can be used to conditionally hide, show, disable, or enable HTML elements in the UI

## Error Handling with Forms
#### Built-in AngularJS validators

| Validator | Description |
| ------------- | ------------- |
| required |  As previously discussed, this ensures that the field is required, and the field is marked invalid until it is filled out. |
| ng-required | Unlike required , which marks a field as always required, the ng-required directive allows us to conditionally mark an input field as required based on a Boolean condition in the controller. |
| ng-minlength | We can set the minimum length of the value in the input field with this directive.|
| ng-maxlength | We can set the maximum length of the value in the input field with this directive.|
| ng-pattern | The validity of an input field can be checked against the regular expression pattern specified as part of this directive.|
| type="email" | Text input with built-in email validation.|
| type="number" | Text input with number validation. Can also have additional attributes for min and max values of the number itself.|
| type="date" | If the browser supports it, shows an HTML datepicker. Otherwise, defaults to a text input. The ng-model that this binds to will be a date object. This expects the date to be in yyyy-mm-dd format (e.g., 2009-10-24).|
| type="url" | Text input with URL validation.|

>In addition to this, we can write our own validators,
#TODO How to write custom validator

#### Displaying Error Messages
AngularJS offers two things to solve this problem:
* A model that reflects what exactly is wrong in the form, which we can use to display nicer error messages
* CSS classes automatically added and removed from each of these fields allow us to highlight problems in the form

Examples
```html
<span ng-show="myForm.uname.$error.required">
This is a required field
</span>
```
* Error models
formName.fieldName.$error.validatorName

#### Styling Forms and States
For each of the states we described previously, AngularJS adds and removes the CSS classes shown in Table 4-3 to and from the forms and input elements.

>Form state CSS classes

| Form state | CSS class applied |
| ------------- | ------------- |
| $invalid | ng-invalid |
| $valid | ng-valid |
| $pristine | ng-pristine |
| $dirty | ng-dirty |

>for Input state CSS classes

| Input state  | CSS class applied |
|--------------|---------------|
| $ invalid      | ng -invalid |
| $ valid         | ng -valid |
| $ pristine | ng -pristine |
| $ dirty   | ng -dirty |
| required | ng -valid-required or ng -invalid-required |
| min | ng-valid-min or ng-invalid-min |
| max | ng-valid-max or ng-invalid-max |
| minlength | ng-valid-minlength or ng-invalid-minlength |
| maxlength | ng-valid-maxlength or ng-invalid-maxlength |
| pattern | ng -valid-pattern or ng -invalid-pattern |
| url | ng -valid-url or ng -invalid-url |
| email | ng -valid-email or ng -invalid-email |
| date | ng -valid-date or ng -invalid-date |
| number | ng -valid-number or ng -invalid-number |

>AngularJS takes the name of the validator (number, maxlength, pattern, etc.) and depending on whether or not that particular validator has been satisfied, adds the __ng-valid-validator_name__ or __ng-invalid-validator_name__ class, respectively.


# Nested Forms with ng-form

<ng-form name="profile">

### Checkbox
```html
<input type="checkbox" ng-model="ctrl.user.agree" ng-true-value="YES" ng-false-value="NO">
```
We can accomplish this using the ng-checked directive, which binds to an AngularJS
expression.
<input type="checkbox" ng-checked="sport.selected === 'YES'">
> So if you need two-way data-binding, use ng-model . If you need one-way data-binding
 ith checkboxes, use ng-checked .

### Radio buttons
<input type="radio" name="gender" ng-model="user.gender" value="male">
We gave them both the same name so that
when one is selected, the other gets deselected
### Combo Boxes/Drop-Downs
```html
<select ng-model="location">
    <option value="USA">USA</option>
    <option value="India">India</option>
    <option value="Other">None of the above</option>
</select>
```
This has a few restrictions, though:
• You need to know the values in the drop-down up front.
• They need to be hardcoded.
• The values can only be strings.
* We use the ng-options attribute on the select dialog, which allows us to repeat an
array (or object, similar to ng-repeat )and display dynamic options.

* You can also optionally give a grouping clause, for which the syntax would be ng-
options="modelValue as labelValue group by groupValue for item in ar
ray" . Similar to how we specified the model and label values, we can point the
groupValue at another key in the object (say, continent).


>__NOTE__ AngularJS compares the ng-options individual values with the ng-model by reference. Thus, even if the two are objects that have the same keys and values, AngularJS will not show that item as selected n the drop-down unless and until they are the same object. We ac‐omplished this in our example by using an item from the array countries to assign the initial value of the model. there is a better way to accomplish this, which is through the use of the track by syntax with ng-options . We could have written the ng-options as:
>ng-options="c.label for c in ctrl.countries track by c.id" this would ensure that the object c is compared using the ID field, instead of by reference, which is the default.

------------------------------------------------------
# All About AngularJS Services
## AngularJS Services
* AngularJS services are functions or objects that can hold behavior or state across our application.
* Each AngularJS service is instantiated only once, so each part of our ap‐plication gets access to the same instance of the AngularJS service.
* Repeated behavior, shared state, caches, factories, etc. are all functionality that can be implemented using AngularJS services.
* A service in AngularJS can be implemented as a factory, service, or provider.
### Why Do We Need AngularJS Services?

>Controllers are stateful, but ephemeral. That is, they can be destroyed and re-created multiple times throughout the course of navigating across a Single Page Application. when we use controllers, they are instances that get created and destroyed as we navigate across our application.

### Services Versus Controllers
| Controllers |Services |
|----------|----------|
| Presentation logic | Business logic |
| Directly linked to a view | Independent of views |
| Drives the UI | Drives the application |
| One-off, specific | Reusable |
| Responsible for decisions like what data to fetch, what data to show, how Responsible for making server calls, commonto handle user interactions, and styling and display of UI |validation logic, application-level stores, and reusable business logic |

# Dependency Injection in AngularJS
Any service known to AngularJS (internal or our own) can be simply injected into any other service, directive, or controller by stating it as a de‐pendency.AngularJS will automatically figure out what the service is, what it further depends on, and create the entire chain before injecting a fully instantiated service.
Dependency Injection states that instead of creating an instance of a dependent service when we need it, the class or
function should ask for it instead.

### Dependency Injection allows us to:
    * Change the underlying implemention of a dependency without manually changing each dependent function
    * Change the underlying implementation just for the test, to prevent it from making server calls
    * Explicitly state what needs to be included and present before this function or con‐structor can execute

AngularJS guarantees that the function we provide to the service declaration will be executed only once (lazily, the first time something that needs the dependency is load‐ed), and future dependents will get that very same instance. That is, AngularJS services are singletons for the scope of our application. Two controllers or services that ask for ServiceA will get the very same instance, instead of two different instances.
AngularJS prefixes all the services that are provided by the Angu‐larJS library with the $ sign.

#### Using dependency Injection
```javascript
angular.module('notesApp', [])
    .controller('MainCtrl', ['$log', function($log) {
        var self = this;
        self.logStuff = function() {
        $log.log('The button was pressed');
    };
}]);
```
> ### Safe Style of Dependency Injection
>
```javascript
    myModule.controller("MainCtrl", ["$log", function($log) {}]);
```
## Common AngularJS Services
 * __$window__   The $window service in AngularJS is nothing but a wrapper around the global win‐dow object.
 * __$location__  The  \$location service in AngularJS allows us to interact with the URL in the browser bar, and get and manipulate its value.The $location service has the following functions, which allow us to work with the URL:
     * absUrl
     * url
     * path
     * search
 * __$http__   AngularJS service used to make XHR requests to the server from the application.Using the $http service, we can make GET and POST requests, set the headers and caching, and deal with server responses and failures.

## Creating Our Own AngularJS Service
### We should consider creating an AngularJS service if what we are implementing falls into one of the following broad criteria:

* __It needs to be reusable__ More than one controller or service will need to access the particular function that is being implemented.
* __Application-level state__ Controllers get created and destroyed. If we need state stored across our application, it belongs in a service.
* __It is independent of the view__ If what we are implementing is not directly linked to a view, it probably belongs in a service.
* __It integrates with a third-party service__ We need to integrate a third-party service (think SocketIO, BreezeJS, etc.), but we want to be able to mock or replace it in our unit tests. A service makes that easy.
* __Caching/factories__ Do we need an object cache? Or something that creates model objects? Services are our best bet.

### AngularJS guarantees the following:
* The service will be lazily instantiated. The very first time a controller, service, or directive asks for the service, it will be created.
* The service definition function will be called once, and the instance stored. Every caller of this service will get this same, singleton instance handed to them.

### Creating a Simple AngularJS Service


### The Difference Between Factory, Service, and Provider
You should use module.factory() to define your services if:
* You follow a functional style of programming
* You prefer to return functions and objects

__NOTE:-__ When we use a service, AngularJS assumes that the function definition passed in as part of the array of dependencies is actually a JavaScript type/class. So instead of just invoking the function and storing its return value, AngularJS will call new on the function to create an instance of the type/class.

### JavaScript Class Function  
##### It doesn’t return anything.
```javascript
function ItemService() {
    var items = [
    {id: 1, label: 'Item 0'},
    {id: 2, label: 'Item 1'}
    ];
    this.list = function() {
    return items;
    };
    this.add = function(item) {
    items.push(item);
    };
}
```

* Our service defines the public API by defining methods ( add , list ) on its instance (using the this keyword).
* Private state for the service is still defined as local variables inside the function definition.
* AngularJS will perform new ItemService() (with possible dependencies injected in) and then return that instance to all functions that depend on ItemService .

#### Defining services using provider methods
The third and final way of defining services is using the provider function. This is not a very common approach, but can be useful when we need to set up some configuration for our service before our application loads.with the provider, we can have functions that can be called to set up how our service works based on the language, environment, or other things that are applicable to our service.
* providers cannot have de‐pendencies on other services.
* The provider also declares a $get function on its instance, which is what gets called when the service needs to be initialized. At this point, it can use the state that has been set up in the configuration to instantiate the service as needed.


### CONFIG FUNCTION
* The config function executes before the AngularJS app executes. So we can be assured that this executes before our controllers, services, and other functions.
* The config function follows the same Dependency Injection pattern, but gets providers injected in. In this case, we ask for the ItemServiceProvider .
* The config function could also set up URL endpoints, locale information, routing configuration for our application, and so on: things that need to be executed and initialized before our application starts.

# Server Communication Using $http
## Promise interface.
Because XHRs are asynchronous method calls, the response from the server will come back at an unknown future date and time (hopefully almost immediately). The Promise interface guarantees how such responses will be dealt with, and allows consumers of the Promise to use them in a predictable manner.

Promises (just like callbacks) allow us to deal with scalability issues.Both of these concepts keep JavaScript nonblocking and event-driven, which allows us to let the browser continue to do its work while the server request is in flight.

### Then function
The then function takes two arguments, a success handler and an error handler. If the server returns a non-200 response, the error handler is called. Otherwise, the success handler is triggered. Both these handlers get passed in a response object, which has the following keys:
* headers     The headers for the call
* status      The status code for the response
* config      The configuration with which the call was made
* data        The body of the response from the server

> ### callback hell
 Fow of course the more levels of nesting we have, the harder the code is to read, debug, maintain, upgrade, and basically work with. This is generally known as callback hell.

The Promise API proposes the following:
1. Each asynchronous task will return a promise object.
2. Each promise object will have a then function that can take two arguments, a success handler and an error handler.
3. The success or the error handler in the then function will be called only once, after the asynchronous task finishes.
4. The then function will also return a promise , to allow chaining multiple calls.
5. Each handler (success or error) can return a value, which will be passed to the next function in the chain of promises.
6. If a handler returns a promise (makes another asynchronous request), then the next handler (success or error) will be called only after that request is finished.

Because of this, if any error happens in any of the functions in the promise chain, AngularJS will find the next closest error handler and trigger it
### Propagating Success and Error
* If we want to trigger the success handler for the next promise in the chain, we can just return a value from the success or the error handler, and AngularJS will treat it as us successfully resolving any errors.
* If, on the other hand, we want to trigger the error handler for the next promise in the chain, we can leverage the \$q service in AngularJS. Just ask for \$q as a dependency in our controller and service, and return $q.reject(data) from the handler. This will ensure that the next promise in the chain goes into the error condition, and will get the data passed to it as an argument.

### The $q Service
* \$q.defer()    The \$q.defer() is useful in those times because the deferred object has a promise attribute that can be returned from a function.
* deferredObject.resolve    The deferred object created by the previous function can be resolved successfully at any point by calling the resolve() function on it with the argument being the data passed to the success handler in the promise chain.
* deferredObject.reject      The deferred object can also be rejected, thus denoting that the promise was a failure and triggering the failure handler in the promise. Again, the argument passed to it will be passed to the error handler as is.
* $q.reject    


# $http API
$http provides the following convenience methods to make certain types of requests:
• GET
• HEAD
• POST
• DELETE
• PUT
• JSONP

The following is a basic pseudocode template for the configuration object, which details the keys that are acceptable and the type of value that it expects:
```
{
    method: string,
    url: string,
    params: object,
    data: string or object,
    headers: object,
    xsrfHeaderName: string,
    xsrfCookieName: string,
    transformRequest: function transform(data, headersGetter) or
    an array of functions,
    transformResponse: function transform(data, headersGetter) or
    an array of functions,
    cache: boolean or Cache object,
    timeout: number,
    withCredentials: boolean
}
```
### Configuring $http Defaults
we can use the config section of our module, and use the $httpProvider to configure these defaults
```javascript
.config(['$httpProvider', function($httpProvider) {
    // Every POST data becomes jQuery style
    $httpProvider.defaults.transformRequest.push(
        function(data) {
            var requestStr;
            if (data) {
                data = JSON.parse(data);
                for (var key in data) {
                    if (requestStr) {
                        requestStr += '&' + key + '=' + data[key];
                    } else {
                        requestStr = key + '=' + data[key];
                    }
                }
            }
    return requestStr;
    });
    $httpProvider.defaults.headers.post['Content-Type'] ='application/x-www-form-urlencoded';
});
```
>Content-Type text/www-form-urlencoded, which is what jQuery defaults to. AngularJS defaults to application/json, which is recommended for web applications.

The $httpProvider.defaults.headers object allows us to set default headers for common , get , post , and put requests.
The following is the list of keys and values that can have defaults set using \$httpProvider (using $httpProvider.defaults ):
• headers.common
• headers.get
• headers.put
• headers.post
• transformRequest
• transformResponse
• xsrfHeaderName
• xsrfCookieName

### Interceptors
AngularJS interceptors allow us to hook and check each request and response and handle certain events such as logging, authentication check, and handling certain types of responses

>Therefore, do not use $httpProvider.responseInterceptors in your code anymore.

#### Best Practices
* Wrap $http in services
* Use interceptors
* Chain interceptors
* Leverage defaults


# Working with Filters
### What Are AngularJS Filters?
AngularJS filters are used to process data and format values to present to the user. They are applied on expressions in our HTML, or directly on data in our controllers and services. Mostly, they are used as that final level of formatting to convert data from the way it is stored to a user-readable format.Another feature of AngularJS filters, when they are used in the view, is that they give us dynamic, on-the-fly data that doesn’t need to be stored.When we apply filters in the HTML, the filtered values are shown to the user but do not modify the original value on which they are applied.

#### Using AngularJS Filters
1. The general syntax to use filters is to use the Unix syntax of piping the result of one expression to another. That is:
{{expression | filter}}
2. We can also chain multiple filters together by piping one filter after another. The syntax would be:
{{expression | filter1 | filter2}}

#### Common AngularJS Filters
* currency
* number
* lowercase
* uppercase
* json
* date
* limitTo
* orderBy   The simplest form of a predicate expression is a string, which is the name of the field (the key of each object) to order the array by, with an optional + or – sign before the field name to decide whether to sort ascending or descending by the field.
* filter   The beauty of the filter filter is that it is dynamic when used in the HTML directly, so the minute the underlying model changes, the entire list gets filtered automatically.

#### Using Filters in Controllers and Services
There are three main things to note:
• The first argument to the filter is the value it needs to act upon.
• Further arguments are the arguments that the filter needs (optional for some), in the order mentioned in the documentation.
• The return value of the filter is the final output that we need.

#### Creating AngularJS Filters

#### Things to Remember About Filters
1. View filters are executed every digest cycle
2. Filters should be blazingly fast
3. Prefer filters in services and controllers for optimization


# Routing Using ngRoute
implementing routing always involves:
• Creating a state machine
• Adding and removing items from the browser’s history
• Loading and unloading templates and relevant JS as the state changes
• Handling the various idiosyncrasies across different browsers

Any URL fragment after the # sign gets ignored by the browser in the server call. It falls upon the client to then take that part of the URL and deal with it. When the user navigates from http://www.myawesomeapp.com/#/first/ page to http://www.myawesomeapp.com/#/second/page, the browser does not make any additional requests.
When the hash fragment changes, the JavaScript responds and loads only the relevant data and HTML instead of reloading the entire HTML. This makes the application faster and snappier because less data is fetched from the server.
AngularJS leverages hash URLs for routing, so all AngularJS routes that we define will be hash URLs.

### Routing Options
##### the route configuration object
```
{
    template: string,
    templateUrl: string,
    controller: string, function or array,  
    controllerAs: string,
    resolve: object<key, function>
}
```
> __RESOLVE :-__ At a conceptual level, resolves are a way of executing and finishing asynchronous tasks before a particular route is loaded.This is a great way to check if the user is logged in and has authorization and permissions, and even preload some data before a controller and route are loaded into the view.

### Using Resolves for Pre-Route Checks
Each key takes an array, which is the AngularJS Dependency Injection syntax. We define the dependencies for the resolve in the array, and get it injected into the resolve function.
It then returns the promise for that particular server call. AngularJS guarantees the following:
1. If the resolve function returns a value, AngularJS immediately finishes executing and treats it as a successful resolve.
2. If the resolve function returns a promise, AngularJS waits for the promise to return and treats the resolve as successful if the promise is successful. If the promise is rejected, the resolve is treated as a failure.
3. Because of the resolve function, AngularJS ensures that the route does not load until all the resolve functions are finished executing. If there are multiple resolve keys that make asynchronous calls, AngularJS executes all of them in parallel and waits for all of them to finish executing before loading the page.
4. If any of the resolves encounter an error or any of the promises returned are rejected (is a failure), AngularJS doesn’t load the route.

One other interesting thing about resolves is that we can get the value from each of the resolve keys injected into our controller, if we want or need the data. Each key can directly be injected into the controller by adding it as a dependency. This is over and above any AngularJS service dependency we might have. The following items are injected into the controller:
1. The value itself, if the resolve function was returning a value
2. The resolution of a promise, if the resolve function was returning a promise


### Using the $routeParams Service
```javascript
    angular.module('resolveApp', ['ngRoute'])
        .config(['$routeProvider', function($routeProvider) {
            $routeProvider.when('/', {
                template: '<h1>Main Page</h1>'
            }).when('/detail/:detId', {
                template: '<h2>Loaded {{myCtrl.detailId}}' +
                    ' and query String is {{myCtrl.qStr}}</h2>',
                controller: ['$routeParams', function($routeParams) {
                    this.detailId = $routeParams.detId;
                    this.qStr = $routeParams.q;
                }],
                controllerAs: 'myCtrl'
            });
        }]);
```
### Things to Watch Out For
* Empty templates
* Resolve injection into controller
* \$routeParam variable type \$routeParam returns string values by default for all keys A === comparison of something from \$routeParams with something from our database, which is a number, fails. We need to make sure we convert both values into the same format before we do any comparisons with data from $routeParams .
* One ng-view per application


### Additional Configuration
#### HTML5 Mode
* Thus to enable HTML5 mode, three things are needed:
```javascript
    angular.module('myHtml5App', ['ngRoute'])
        .config(['$locationProvider', '$routeProvider',
            function($locationProvider, $routeProvider) {
                $locationProvider.html5Mode(true);
                //Optional
                $locationProvider.hashPrefix('!');
                // Route configuration here as normal
            // Route for /first/page
            // Route for /second/page
        }]);
```
* In index.html, we need to add the <base> tag with an href attribute to the <head> portion.
__ <base href="/app" /> __


#### SEO with AngularJS     Page No 163
> Google and AJAX App Indexing Google has plans to start trying to parse and view JavaScript-based pages as the user might see them by actually executing the Java‐ Script in the page. This is in contrast to the accepted norms up until now, which required just loading the HTML page. The Google Web‐ master Central page has more details on it, along with tools to un‐ derstand how Google views these pages.

http://bit.ly/1CqIVT6


### Analytics with AngularJS
Angularanalytics -->https://github.com/mgonto/angularytics





# Alternatives: ui-router  __(http://github.com/angular-ui/ui-router/wiki)__
* Instead of using hrefs and anchors to navigate in our application, the ui-router provides a directive called ui-sref that allows us to navigate to states. This can be added not only on anchors, but on buttons, images, and any other element on which we need the behavior
* Instead of using the \$routeProvider to define our routes, we use the $stateProvider to define our states.
* We can have multiple named ui-views in our application.
* ui-router is state-oriented, and by default does not modify URLs. We need to specify the URL for each state individually.

### We should consider using ui-router if our project needs or has the following requirements:
* We need different parts of the page to react differently to URL changes or user interactions.
* We have multiple different (nested) sections of the page that are conditionally shown for various actions and events.
* We don’t need the URL to change while the user navigates throughout our application.
* The entire UI layout needs to change completely across different pages.



# Directives
Directives are the AngularJS way of dealing with DOM manipulation and rendering reusable UI widgets.
### What Are Directives?
Directives are of two major types in AngularJS
1. Behavior modifiers:-These types of directives work on existing UI and HTML snippets, and just add or modify the existing behavior of what the UI does. Example _ng-show_ and _ng-model_
2. Reusable components :-These types of directives are the more common variety, in which the directive creates a whole new HTML structure. These directives have some rendering logic (how and what should it display) and some business logic (where should it get the data,what happens when the user interacts with it) attached to it.

### Alternatives to Custom Directives
We have two directives, ng-include and ng-switch , which can help us in extracting HTML into smaller chunks and deciding when to show and hide them in our HTML. In many cases, we can use these two directives instead of writing our custom directives.
### 1. __ng-include__
The ng-include directive takes an AngularJS expression (similar to ng-show and ng- click ) and treats its value as the path to an HTML file. It then fetches that HTML file from the server and includes its content as the child (and the only child, replacing all other existing content) of the element that ng-include is placed on.
<div ng-include="mainCtrl.stockTemplate"></div>
<div ng-include="'views/stock.html'"></div>
#### Limitations of ng-include
1. The stock.html file currently looks for a variable called stock and displays its name, price, and percentage change information.
2. The stock.html file is also currently dependent on being used along with mainCtrl .


### 2. __ng-switch__
The ng-switch is another directive that allows us to add some functionality to the UI for selectively displaying certain snippets of HTML. It gives us a way of conditionally including HTML snippets by behaving like a switch case directly in the HTML.
ng-switch
ng-switch-when
#### There are a few things to note about ng-switch :
• ng-switch loads its content, and then based on the condition, comments out all the ng-switch-when conditions that are not satisfied. So even if we use ng- include inside ng-switch-when , they will not get loaded up front, and will be loaded only when the condition is met.
• ng-switch-when is treated as an attribute, and thus the value passed to it is expected to be direct, and not an AngularJS expression. That is, suppose we have ng-switch="mainCtrl.currentTab" and then ng-switch-when="mainCtrl. possibleValue" . This would expect the value of currentTab in the controller to be equal to the string "mainCtrl.possibleValue" , and not the value of mainCtrl.possibleValue . ng-switch-when does not understand AngularJS expressions.

### Understanding the Basic Options
The main intentions of a directive are:
• To make our intention declarative by specifying in the HTML what something is or tries to do.
• To make something reusable so the same functionality can be achieved easily without copying and pasting the code.
• To achieve abstraction in the sense that the user of the directive doesn’t need to know or understand how something is performed, but only cares about the end result. The corollary of this is that the underlying implementation can be changed without having to change every single usage.

### Creating a Directive
```javascript
    angular.module('stockMarketApp', [])
        .directive('stockWidget', [function() {
            return {
            // Directive definition will go here
        }
    }]);
```
This function sets up our directive using what we call a __directive definition object__ and returns this definition.
> Directive and Attribute Naming One thing to note is the naming of the directive. HTML is case- insensitive by default. To deal with this when translating names of attributes and directives from HTML to JavaScript, AngularJS converts dashes to camelCase. Thus, stock-widget (or STOCK-WIDGET or even Stock-Widget ) in HTML becomes stockWidget in JavaScript.

#### options when creating directives
1. Template/Template URL
> Note that AngularJS will be smart and fetch the HTML template that’s at the templateUrl location only once when the directive is encountered the very first time. After that, it saves the template in its local cache and serves it from there..
2. Restrict
The restrict keyword defines how someone using the directive in their code might use it. As mentioned previously, the default way of using directives is via attributes of existing elements
The possible values for restrict are
A :-The letter A in the value for restrict specifies that the directive can be used as an attribute on existing HTML elements This is the default value.
E :-The letter E in the value for restrict specifies that the directive can be used as a new HTML element
C :-The letter C in the value for restrict specifies that the directive can be used as a class name in existing HTML elements
M :- The letter M in the value for restrict specifies that the directive can be used as HTML comments __(such as <!-- directive: stock-widget -→ )__

> Expressions in Class Directives We have seen four possibilities for the restrict key. We will ex‐ plore in the following sections how to pass values to our directive, but we might wonder how that is possible with the class-based direc‐ tive. That is, how would <div my-widget="someExp"> translate to a class-based directive? Class-based directives translate to <div class="my-widget: some Exp;"> and AngularJS would treat this as similar to passing a value to an attribute in HTML.

#### The link Function
The link keyword in the directive definition object is used to add what we call a “link function” for the directive. The link function does for a directive what a controller does for a view—it defines APIs and functions that are necessary for the directive, in addition to manipulating and working with the DOM.
The link function gets a standard set of arguments passed to it that remain consistent across directives, which looks something like the following:
link: function(\$scope, \$element, $attrs) {}
The link function is also where we can define our own listeners, work directly with the DOM element, and much more.

> ### What’s the Scope?
> You might wonder what scope we are adding these functions to. You’re right to be worried, and it is something we should always keep in mind when we add functions to the scope in the link function. In the example in this section, ng-repeat in the main index.html file creates a scope for each stock in our array, and it is to this scope that we’re adding the functions. Because of this, we’re not affecting the controller’s scope directly, but that is an unintentional side effect of ng-repeat . If we had used our directive outside ng-repeat , we would have ended up modifying the controller’s scope directly, which is bad practice.
 The default scope given in the link function (unless specified other‐ wise) is the scope that the parent has. Adding functions to the parent scope should always be frowned upon, because the parent should ideally not be changed from within a child.


### Scope
By default, each directive inherits its parent’s scope, which is passed to it in the link
function. This can lead to the following problems:
• Adding variables/functions to the scope modifies the parent as well, which suddenly gets access to more variables and functions.
• The directive might unintentionally override an existing function or variable with the same name.
• The directive can implicitly start using variables and functions from the parent. This might cause issues if we start renaming properties in the parent and forget to do it in the directive.

AngularJS gives us the scope key in the directive definition object to have complete control over the scope of the directive element. The scope key can take one of three values:
    * false  :- This is the default value, which basically tells AngularJS that the directive scope is the same as the parent scope, whichever one it is.
    * true  :- This tells AngularJS that the directive scope inherits the parent scope, but creates a child scope of its own.The directive thus gets access to all the variables and functions from the parent scope, but any modifications it makes are not available in the parent.
    * object  :- We can also pass an object with keys and values to the scope. This tells AngularJS to create what we call an isolated scope.This scope does not inherit anything from the parent, and any data that the parent scope needs to share with this directive needs to be passed in through HTML attributes. This is the best option when creating reusable components that should be independent of how and where they are used.

In particular, we can specify three types of values that can be passed in, which AngularJS will directly put on the scope of the directive:
    * = :- The = sign specifies that the value of the attribute in HTML is to be treated as a JSON object, which will be bound to the scope of the directive so that any changes done in the parent scope will be automatically available in the directive.
    * @ :- The @ sign specifies that the value of the attribute in HTML is to be treated as a string, which may or may not have AngularJS binding expressions ({{ }}). The value is to be calculated and the final value is to be assigned to the directive’s scope. Any changes in the value will also be available in the directive
    * & :- The & sign specifies that the value of the attribute in HTML is a function in some controller whose reference needs to be available to the directive. The directive can then trigger the function whenever it needs to.

> Whenever we pass data using object binding to directives, it is done by reference. AngularJS uses this fact to ensure that any changes done to the variable in the controller are reflected inside the directive. But this also means that if the reference to the variable gets reassigned in the directive, then the data-binding breaks in AngularJS.

#### Replace
In some cases, we might not want the original element to remain and instead be completely replaced with new HTML. For such cases, AngularJS offers the replace key as part of the directive definition object.The replace key takes a Boolean, and it defaults to false . If we specify it to true , AngularJS removes the element that the directive is declared on, and replaces it with the HTML template from the directive definition object (or the contents from templateUrl ). Any existing attributes and classes on the HTML element that the directive is on are migrated from the old element to the new one  __replace Is Deprecated__ from 1.3 onwords


# Advanced Directives
## Life Cycles in AngularJS
#### AngularJS Life Cycle
When an AngularJS application is loaded in our browser window, the following events are executed in order:
1. The HTML page is loaded:
  a. The HTML page loads the AngularJS source code (with jQuery optionally loaded before).
  b. The HTML page loads the application’s JavaScript code.
2. The HTML page finishes loading.
3. When the document ready event is fired, AngularJS bootstraps and searches for any and all instances of the ng-app attribute in the HTML: a. If AngularJS is bootstrapped manually, this needs to be triggered by the code we write.
4. Within the context of each (there could be more than one) ng-app , AngularJS starts its magic by running the HTML content inside ng-app through what is known as the compile step:
  a. The compile step goes through each line of HTML and looks for AngularJS directives.
  b. For each directive, it executes the necessary code as defined by that directive’s definition object. In case the directive creates or loads new HTML, it recursively descends into HTML until all directives are recognized by the compiler.
  c. At the end of the compile step, a link function is generated for each directive that has a handle on all the elements and attributes that need to be controlled by AngularJS.
5. AngularJS takes the link function and combines it with scope . The scope has the variables and contents that need to be displayed in the UI. This combination generates the live view that the user sees and interacts with:
  a. AngularJS will take the variables in scope , and display them in the UI if the HTML refers to a scope variable.
  b. Each controller and subcontroller is instantiated with its own scope that will be used to display data in the UI.
  c. AngularJS also adds watchers and listeners for all the directives and bindings to ensure it knows which fields it needs to track and which data field to bind with.
6. At the end of this, we have a live, interactive view with the content filled in for the user.


#### Angular need to update UI
It can change only in response to one of the following events:
— The user makes a modification (types in, checks a checkbox, clicks a button, etc.) in a form or input element.
— A server request is made and its response comes back (XHR or asynchronous request returning).
— A \$timeout or $interval is used to execute something asynchronously.
Outside of these events, the data cannot change randomly on its own. So AngularJS adds watchers for all its bindings and ng-model . And whenever one of the aforementioned events happens, AngularJS checks its watchers and bindings to see if anything has changed. If nothing has changed, AngularJS proceeds without doing anything. But if AngularJS finds that one of the fields it’s controlling has changed underneath, it triggers an update of the UI.

#### The Digest Cycle
The update cycle that we mentioned earlier has a name in AngularJS: the digest cycle. The digest cycle in AngularJS is responsible for keeping the UI up to date in an AngularJS application. The AngularJS UI update cycle happens as follows:
1. In the digest cycle, AngularJS starts from $rootScope and checks each watcher in the scope to see if the current value differs from the value it’s displaying in the UI.
2. If nothing has changed, it recurses to all the parent scopes and so on until all the scopes are verified.
3. If AngularJS finds a watcher at any scope that reports a change in state, AngularJS stops right there, and reruns the digest cycle
4. The digest cycle is rerun because a change in a watcher might have an implication on a watcher that was already evaluated beforehand. To ensure that no data change is missed, the digest cycle is rerun.
5. AngularJS reruns the digest cycle every time it encounters a change until the digest cycle stabilizes. On average, this takes two to three cycles for a normal AngularJS application. a. To prevent AngularJS from getting into an infinite loop where one watcher updates another model and vice versa, AngularJS caps the digest cycle reruns to 10 by default.
6. When the digest stabilizes, AngularJS accumulates all the UI updates and triggers them at once.

>NOTE: Lightweight Watchers Because watchers and listeners can be executed multiple times by AngularJS for a single update, it’s recommended that any watch that we add execute really fast. Thus it is almost always a bad idea to do time and CPU-intensive tasks within watchers. The general rule of thumb is that no individual watcher function take more than 20 microseconds, and no more than 2,000 variables be tracked or watched at any one point in time.

#### Directive Life Cycle
1. When the application loads, the directive definition object is triggered. This happens only once, so anything declared or expressed before returning the directive definition object can be treated as the constructor for the directive, and will be executed once the very first time the application loads.
2. Next, when the directive is encountered in the HTML the very first time, the template for the directive is loaded (either asynchronously from the server or directly as a template from the definition). In either case, the template is cached and reused in further instances of the directive.
3. This template is then compiled and AngularJS handles the other directives present in the HTML. This generates a link function that can be used to link the directive to a scope.
4. The scope for the directive instance is created or acquired. This could be the parent scope, a child of the parent scope, or an isolated scope as the case might be
5. The link function (and the controller) execute for the directive. This is where we add functionality that is specific to each instance of the directive.



#### directive definition keys
1. Transclusions :- AngularJS directives have a concept of transclusions to allow us to create reusable di‐ rectives where each implementation might need to render a certain section of the UI differently.

###### Basic transclusion can be thought of as a two-step process:
1. First, we tell the directive that we are going to use transclusion as part of this di‐ rective. This tells AngularJS that whenever the directive is encountered in the HTML, to make a copy of its content and store it so that it’s not lost when AngularJS replaces it with the directive’s template. This is accomplished by setting the key transclude to true as part of the directive definition object.
2. Second, we need to tell AngularJS where to put the content that was stored in the template. This is accomplished by using the ng-transclude directive, which en‐ sures that the content that was captured is made a child of the element in the di‐ rective template.

When AngularJS encounters the stock-widget directive in index.html, it grabs its inner content ( Recommended Stock : {{s.name}} ) and stores it for later use.
• When it includes the directive’s template in the instance, it looks for the ng- transclude directive, and places the content that it stored in step 1 as a child in the element with ng-transclude .
• Thus, when the UI renders for each stock in the stocks array, we get the Recom‐ mended Stock text followed by each stock’s name.

>When AngularJS encounters transclude , it clones the HTML be‐ fore replacing it with the template or templateUrl contents. Then, when it encounters ng-transclude , it compiles the transcluded con‐ tent, but links it to the parent scope instead of the isolated scope of the directive. Thus, the transcluded content still has access to the parent controller and its content, while the directive HTML has an isolated scope (or a new scope, as the case might be). Thus, the transcluded content and the directive content form a sib‐ ling relationship but do not share the same scope. This is because the transcluded content is not expected to know the inner workings of the directive, but is instead expected to be dependent on the usage and context in which it is used.

###### Advanced Transclusion


###### Directive Controllers and require
The reason we use the link function and not controllers to define our directive-specific behavior is that directive controllers are present for a completely different use case. Directive controllers are used in AngularJS for inter-directive communication, while link functions are fully contained and specific to the directive instance. By inter- directive communication, we mean when one directive on an element wants to com‐ municate with another directive on its parent or on the same element. This encompasses sharing state or variables, or even functions.
Whenever we need to communicate between child and parent directives, or between sibling directives, we should consider using directive controllers.
A directive controller is a function that gets the scope and element injected in. This is similar to the link function that we’ve been using so far, but the difference is that functions we define in the controller on this can be accessed by child or sibling controllers Thus, the controller can define functions that are specific to the directive instance by defining them on $scope as we have been doing so far, and define the API or accessible functions and variables by defining them on this or the controller’s instance.

__require__ :- This tells AngularJS that for the tab directive to work, it requires that one of the parent elements in the HTML be the tabs directive, and we want its controller to be made available to the tab directive.
__require Options__
Now, each individual string can take some prefixes, which define how AngularJS should behave when finding these directives. For example:
require: 'tabs'
implies that AngularJS should locate the directive tabs on the same element, and throw an error if it’s not found:
require: '?tabs'
This implies that AngularJS should try to locate the directive tabs on the same element, but pass null as the fourth argument to the link function if it isn’t found. That is, prefixing ? tells AngularJS to treat the directive as an optional dependency. Furthermore, we can also tell AngularJS to look for a directive not on itself, but on its parent chain. This can be done as:
require: '^tabs'
We can also mix and match these prefixes. For example: require: '?^tabs' implies that a parent element of our directive may or may not have the tabs directive, but if it is present, it should be injected into our directive link function.


#### Input Directives with ng-model

#### Custom Validators
#### Compile
The compile step in a directive is the correct place to do any sort of HTML template manipulation and DOM transformation. We never use the link and compile functions together, because when we use the compile key, we have to return a linking function from within it instead.

#### Pre- and Post-Linking
The link function we generally write (and even return from the compile function) is what is known as a post-link function. When a post-link function executes, all chil‐ dren directives have been compiled and linked at this point. DOM transformations (not adding new AngularJS directives, but creating a chart, for example) are safe at this point as well.
But in case we needed a hook to execute something before the children are linked, we can add what is called pre-link function. At this point, the children directives aren’t linked, and DOM transformations are not safe and can have weird effects.
```javascript
{
    link: {
        pre: function($scope, $element, $attrs) {},
        post: function($scope, $element, $attrs) {}
    }
}
```

#### Priority and Terminal
__priority__ is used to decide the order in which directives are evaluated when there are multiple directives used on the same element.
By default, any directive we create has a priority of 0.The larger the number, the higher the priority, and higher priority directives are compiled and linked before lower priority ones.
The terminal keyword in a directive is used to ensure that no other directives are compiled or linked on an element after the current priority directives are finished. Also, children directives will not be touched or compiled when terminal is set to true . By default, it is set to false . Any directives on an element at the same priority will execute, because the order of execution of directives at the same priority is not defined.

#### Third-Party Integration

## Best Practices
1. Directives with isolated scopes are the most reusable directives.

#### Clean Up and Destroy
1. Listen for the $destroy event on the scope
    ```javascript
        $scope.$on('$destroy', function() {
            // Do clean up here
        });
    ```
2. Listen for the $destroy event on the element :-This is a jQuery event that is fired by AngularJS when the element is about to be removed from the DOM. A sample handler for this would be as follows:
```javascript
    $element.$on('$destroy', function() {
    // Do clean-up here
    });    
```
3. Watchers  :- These basically get triggered by AngularJS whenever the variable under watch changes, and we get access to both the new and the old value in such a case.
There are a few kinds of watches we can add, and it helps to be aware of the implications of each:
$watch
    The most standard watch, which takes:
    • A string, which is the name of a variable on the scope
    • A function, whose return value is evaluated
Deep $watch
The same as the standard watch, but takes a Boolean true as the third argument. This forces AngularJS to recursively check each object and key inside the object or variable and use angular.equals to check for equality for all objects. Obviously, it catches all changes, but also consumes more CPU cycles. So be careful that you don’t abuse deep watches across your application. Instead, it’s preferable to have a Boolean that signals if something internally has changed and watch that.
$watchCollection
Slightly optimized version of the watch aimed at arrays. Similar arguments as the $watch , but expects that the value is an array. The function is triggered any time an item is added, removed, or moved in the array. It does not watch for changes to individual properties in an item in the array.

4. $apply (and $digest)  :- in any case if you are updating any scope variables in response to an external event, make sure you manually trigger the $scope.$apply() or $scope.$digest() .




----------------------------------------------------------------------------------------
# Guidelines and Best Practices
## Testing
The first and foremost rule of web application development is that testing happens before the application development starts, while you are developing the application, and af‐ ter you’ve finished development.
#### Test-Driven Development
1. Variety of Tests
    1. __Unit tests__ :- A unit test is concise and focused on testing only one piece (a controller, service, filter, etc.) and in that, one function.
    2. __Integration tests__ :- Integration tests (again, written using Karma and Jasmine in AngularJS) are the ones that test whether the different parts of your application are correctly configured and hooked up. These might check whether the controller communicates with the service, and whether the service has the right side effects and returns the correct values.
    3. __Scenario tests__ :- The final kinds of tests we should consider having for our application are end-to- end, scenario tests. A good set of scenario tests will be small, stable, deterministic, and try to catch most of the integration points. The aim of scenario tests is to ensure that, at the very least, your most basic features and flows in your application are correctly hooked up and working.
2. When to Run Tests
    1. On every save
    2. Before pushing
    3. Continuous integration

### Project Structure
    1. Have one controller, service, directive, or filter per file. Don’t club them into large single files.
    2. Don’t have one giant module. Break up your applications into smaller modules. Let your main application be composed of multiple smaller, reusable modules.
    3. Prefer to create modules and directories by functionality (authorization, admin‐ services, search, etc.), over type (controllers, services, directives). This makes your code more reusable.
    4. Use the recommended syntax of using the module functions ( angular.module('so meModule').controller... , etc.) over any other syntax you might see online or any‐ where else.
    5. Use namespaces. Namespace your modules, controllers, services, directives, and filters.
    6. Most of all, be consistent. This is not AngularJS-specific, but don’t change the way you name files, folders, directives, or controllers from one place in your application to another.

### Directory Structure
    app             The app folder houses all the JavaScript code that you develop. We’ll talk about this in more detail next.
    tests           Houses all your unit tests and possibly the end-to-end scenario tests as well.
    data            Any data that is common but not dynamic in your application can be stored here.
    scripts         Build scripts and other common utility scripts can be stored in this folder.
    Other files     The package.json, bower.json, and other files that don’t really need a directory can be in the main folder.

1. __Group by functionality or component__ :- you group your code into folders based on components or functionality.
2. __Components and app sections__ :- The components directory contains folders that contain:
    • Related services, directives, and related files.
    • Dependent data like CSS, images, and others can be located here, or separately, depending on the need.
    • Each folder could be further divided into subfolders if the component is complex.

    The second is app sections, which reflect your application structure and can be routes, pages (like search, listing, admin, view, etc.), or even small subsections of the page (like a dashboard). The sections folders generally:
    • Reflect views that are shown to the user.
    • Contain only the template HTML (and CSS), and the controller that works with it.
3. __Tests should mirror the app__ :- The tests folder would mostly contain the protractor-based, end-to-end, scenario tests. The structure inside the tests folder would mimic your application and its views. In that sense, each view or section of the page under test in the live running application could have a subfolder with its own tests. The folder structure here should reflect how your application is structured and flows through it, rather than try to fit inside the existing directory structure.
The unit tests, on the other hand, are colocated with your application code, in each subfolder inside the app folder. Thus each controller, service, directive, or filter has its unit test right alongside it in the same folder. This makes it easier to find and manage, while giving the build tools the responsibility to ensure that tests don’t get included in your application bundle.
4. __Naming conventions__ :-
    1. When it comes to filenames, the filename should be descriptive enough to be able to figure out which section or component it’s in and what type of AngularJS object it is.
    2. Ideally, a controller file should end with -controller.js, a service with - service.js, a directive with -directive.js, and a filter with -filter.js.
    3. Test files should have the filename, followed by test.js.
    4. Prefer to use lowercase when naming your files, instead of camelCase or each word starting with uppercase.

## Starting Point
1. Yeoman :- Yeoman is a workflow management tool that automates a lot of the routine, chore- like tasks that are necessary in any project.
2. Angular seed projects :-
3. Mean.io :- http://www.mean.io

## Build
1. Grunt :-
2. Minification :- Don’t forget that you should be using the safe style of Dependency Injection before you run your minification step.
3. ng-templates

## Best Practices
The following are some high-level things to keep in mind when writing your AngularJS application:
1. Prefer small files to large files. They are much easier to maintain, debug, and un‐ derstand than large files. An arbitrary rule of thumb for a large file is over 100 lines.
2. Use the AngularJS version of setTimeout , which is the $timeout service, and the AngularJS version of setInterval , which is the $interval service.
3. Any controller or directive, if it adds $timeout or $interval , should remember to clean it up or cancel it when it is destroyed, to prevent it from unnecessarily exe‐ cuting in the background.
4. If you are adding listeners outside of AngularJS, ensure that it is cleaned up correctly. AngularJS manages its own scopes and listeners, but anything you manually add might need to be managed and cleaned up. You can do this when the scope is destroyed by adding a $scope.$on('$destroy', function() {}) listener.
5. Try to avoid doing deep watches ( $scope.$watch , with the third argument as true). It is expensive, and overusing it can cause performance issues in your application. Instead, prefer to have a simple Boolean that reflects when an object has changed, and watch that instead.
6. Try to follow the AngularJS paradigm of model-driven programming.
7. Use HttpInterceptors when you have common tasks you need to do every time the server returns an authorization error, or a “404 not found.” Let the services and controllers take care of specific error handling only.

#### Services
Here are some good tips to get the most out of your services:
1. Services are singletons for your application. Leverage this—use it as a service API, as a data store, as a cache. Services are great for it.
2. If you need to share state across the application, think of a service.
3. There is no performance difference between using service, factory, or provider.
4. Services are the only place where adding event listeners on the $rootScope is ac‐ ceptable.This is because services don’t have their own scope.
5. Multilayered, composite services are great. Instead of having one giant service that does everything, split it into smaller services. Then have one larger service that uses each of the individual ones.
6. XHR calls should be done in services using $http .
7. Integrations with third-party service libraries (think third-party non-UI libraries, like SocketIO) should be done as a service. This allows you to integrate and replace it seamlessly at any given point, as well as mock it out for unit testing.

#### Controllers
Here are some guidelines to keep in mind when creating your controllers:
1. Prefer to use the newer syntax when working with controllers, or defining variables and functions on the controller’s this . That is, use the controllerAs syntax and avoid using the $scope syntax whenever possible. The newer syntax is more concise and easier to understand.
2. Watch out for using the this variable. Prefer to assign it to a local variable (like self ), and then use it.
3. Controllers should not reference the DOM or reach out into the DOM. Do not use jQuery directly in your controllers.
4. Controllers should ideally just have the presentation logic of what data to fetch, how to show it, and how to handle user interactions. And most of these should pass through to a service when possible.
5. Only put the variables and functions that need to be accessed from the HTML on the controller’s this . Anything that the HTML does not need can and should be local variables inside the controller. The exception, of course, are functions that you want to unit test.
6. If a controller is for a specific route that needs to be accessible via a URL, then ensure that the controller loads all the data it needs when it is instantiated.
7. If a controller needs to store some state for the entire application, it should be stored in a service, and not $rootScope . Never $rootScope .
8. Controllers can $broadcast or $emit events on their own scope, or inject the $rootScope and fire events on $rootScope . But a controller should never add a listener on $rootScope . This is because a controller and its scope can get destroyed, but the $rootScope remains across the application, along with its listeners, which will keep triggering even if the controller is not present.


#### Directives
Here’s how you can ensure you get the most out of your directives:
1. If you’re bringing in a third-party UI component, bring it in as an AngularJS directive.
2. Try to isolate the scope if you want reusable components, because this ensures that you don’t modify the parent scope, or depend on anything from any parent scope.
3. Don’t forget to $scope.$apply() in case you’re responding to an event or callback that is external to AngularJS and updating the AngularJS model. Otherwise, your UI won’t get updated at the correct moment.
4. If you add any event listeners on elements external to the directive, or any polling functions, ensure that you clean it up when the directive gets destroyed.
5. You should do your cleanup on the $scope $destroy event if you are creating a new child scope or an isolated scope. But in case you’re inheriting the parent scope, prefer to do your cleanup on the $element $destroy event.
6. If a controller needs to share state with a directive, it should:
    * Pass in the state using HTML attributes (and the isolated scope) if the component is not specific to your project and can be reusable.
    * Pass in the state using a service if it is a specific component.
7. Pass in an object using the = binding on the scope, and add a $scope.$watch on it if you need to perform an operation whenever the object changes.
8. Never reassign the reference of any object passed in through the scope. That is, if the scope.firstObject is passed in via = binding, you should never set or overwrite the value of scope.firstObject in your directive. Updating a key on firstOb ject is fine, but the firstObject itself should never be reassigned.

#### Filters
Filters are great for the last step formatting that we need to do, or data manipulation. Here are two things to keep in mind:
1. Every filter used in the HTML gets evaluated in every digest cycle.
2. Filters should be fast, because each filter is expected to execute multiple times during the life of an application.

---------------------------------------------------------------------------------------------
# Tools and Libraries
1. __Batarang__ :-
2. __WebStorm__ :-

# Optional Modules
1. ngCookies
2. ngSanitize
3. ngResource
4. ngTouch
5. ngAnimate
---------------------------------------------------------------------------------------------
#### AngularJS Convenience Functions
AngularJS exposes a bunch of global functions that we can use when writing our application to perform common tasks that are not part of the basic JavaScript API. These include the following functions:
* angular.forEach Iterator over objects and arrays, to help you write code in a functional manner.
* angular.fromJson and angular.toJson Convenience methods to convert from a string to a JSON object and back from a JSON object to a string.
* angular.copy Performs a deep copy of a given object and returns the newly created copy.
* angular.equals Determines if two objects, regular expressions, arrays, or values are equal. Does deep comparison in the case of objects or arrays.
*  angular.isObject , angular.isArray , and angular.isFunction Convenience methods to quickly check if a given variable is an object, array, or function.
* angular.isString , angular.isNumber , and angular.isDate Convenience methods to check if a given variable is a string, number, or date object.




-------------------------------------------------------------------------------------------------------
# Important points to be remembered
1. Do not use any variables that start with $$ in your application.An‐ gularJS uses them to denote private variables that it uses for its own purposes, and does not guarantee their presence or continued work‐ ing across different versions of AngularJS
2. The ng-submit directive has a few advantages over having an ng-click on a button when it comes to forms. A form submit event can be triggered in multiple ways: clicking the Submit button, or hitting Enter on a text field. The ng-submit gets triggered on all those events, whereas the ng-click will only be triggered when the user clicks the button.
3. When you use ng-model ,AngularJS automatically creates the objects and keys necessary in the chain to in‐stantiate a data-binding connection
4. ng-switch ng-switch acts like a switch statement in the HTML.It takes a variable (using the on attribute, which in this case is MainCtrl ’s tab), and then, depending on the state, hides and shows elements (using the ng-switch-when attribute, used as children of the ng-switch ). The ng- switch-when takes the value that the variable should take.
5. This is important because in a Single Page Application where controllers can get created and destroyed, the service can act as an application-level store.
6. The third and final way of defining services is using the provider function. This is not a very common approach, but can be useful when we need to set up some configuration for our service before our application loads.with the provider, we can have functions that can be called to set up how our service works based on the language, environment, or other things that are applicable to our service.
7. four cornerstones of AngularJS applications: controllers and services,filters.

--------------------------------------------------------------------------------
# AngularJS working
1. The HTML is loaded. This triggers requests for all the scripts that are a part of it.
2. After the entire document has been loaded, AngularJS kicks in and looks for the
ng-app directive.
3. When it finds the ng-app directive, it looks for and loads the module that is specified
and attaches it to the element.
4. AngularJS then traverses the children DOM elements of the root element with the
ng-app and starts looking for directives and bind statements.
5. Each time it hits an ng-controller or an ng-repeat directive, it creates what we
call a scope in AngularJS. A scope is the context for that element. The scope dictates
what each DOM element has access to in terms of functions, variables, and the like.
6. AngularJS then adds watchers and listeners on the variables that the HTML ac‐
cesses, and keeps track of the current value of each of them. When that value
changes, AngularJS updates the UI immediately.
7. Instead of polling or some other mechanism to check if the data has changed, An‐
gularJS optimizes and checks for updates to the UI only on certain events, which
can cause a change in the data or the model underneath. Examples of such events
include XHR or server calls, page loads, and user interactions like click or type.
--------------------------------------------------------------------------------
# TODO
1. A Full AngularJS Routing Example
2. Alternatives: ui-router  __(http://github.com/angular-ui/ui-router/wiki)__
3. Reducing Code with ngResource Page Nor-88 (Angular JS Up and running);
4. official AngularJS docs for ngResource .    
5. Routing
6. Directives
7. Unit Testing Directives
8. AngularJS Convenience Functions  (https://docs.angularjs.org/api/ng/function)
