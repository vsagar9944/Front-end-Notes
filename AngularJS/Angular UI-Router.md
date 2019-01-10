

[TOC]

 onEnter and onExit callbacks

```javascript
$stateProvider.state("contacts", {
  template: '<h1>{{title}}</h1>',
  resolve: { 
     title: function () { 
       return 'My Contacts' 
     } 
  },
  controller: function($scope, title){
    $scope.title = title;
  },
  onEnter: function(title){
    if(title){ ... do something ... }
  },
  onExit: function(title){
    if(title){ ... do something ... }
  }
})
```

# State Change Events

1. **$stateChangeStart** - fired when the transition begins.	

   ```javascript
   $rootScope.$on('$stateChangeStart', 
   function(event, toState, toParams, fromState, fromParams, options){ 
     ...
   })
   ```

   **Note:** Use event.preventDefault() to prevent the transition from happening.

   ```javascript
   $rootScope.$on('$stateChangeStart', 
   function(event, toState, toParams, fromState, fromParams, options){ 
       event.preventDefault(); 
       // transitionTo() promise will be rejected with 
       // a 'transition prevented' error
   })
   ```

2. **$stateNotFound** :- fired when a requested state **cannot be found** using the provided state name during transition.  A special `unfoundState` Use `event.preventDefault()` to abort the transition (transitionTo() promise will be rejected with a 'transition aborted' error)

   ```javascript
   // somewhere, assume lazy.state has not been defined
   $state.go("lazy.state", {a:1, b:2}, {inherit:false});

   // somewhere else
   $rootScope.$on('$stateNotFound', 
   function(event, unfoundState, fromState, fromParams){ 
       console.log(unfoundState.to); // "lazy.state"
       console.log(unfoundState.toParams); // {a:1, b:2}
       console.log(unfoundState.options); // {inherit:false} + default options
   })
   ```

3. **$stateChangeSuccess** :- fired once the state transition is **complete**.

   ```javascript
   $rootScope.$on('$stateChangeSuccess', 
   function(event, toState, toParams, fromState, fromParams){ ... })
   ```

4. **$stateChangeError** - fired when an **error occurs** during transition. if you have any errors in your `resolve` functions (javascript errors, non-existent services, etc) they will not throw traditionally. You must listen for this $stateChangeError event to catch ALL errors. Use `event.preventDefault()` to prevent the $UrlRouter from reverting the URL to the previous valid location (in case of a URL navigation).

   ```javascript
   $rootScope.$on('$stateChangeError', 
   function(event, toState, toParams, fromState, fromParams, error){ ... })
   ```

   ​

# View Load Events

1. **$viewContentLoading** - fired once the view **begins** loading, **before** the DOM is rendered. The '$rootScope' broadcasts the event.

   ```javascript
   $rootScope.$on('$viewContentLoading', 
   function(event, viewConfig){ 
       // Access to all the view config properties.
       // and one special property 'targetView'
       // viewConfig.targetView 
   });
   ```

   ​

2. **$viewContentLoaded** - fired once the view is **loaded**, **after** the DOM is rendered. The '$scope' of the view emits the event.

   ```javascript
   $scope.$on('$viewContentLoaded', 
   function(event){ ... });
   ```

   ​

# Nested States and Nested Views

### Methods for Nesting States

States can be nested within each other. There are several ways of nesting states:

1. Using 'dot notation'. For example `.state('contacts.list', {})`.
2. Use the [ui-router.stateHelper](https://github.com/marklagendijk/ui-router.stateHelper) to build states from a nested state tree.
3. Using the `parent` property with the parent name as **string**. For example: `parent: 'contacts'`
4. Using the `parent` property with the parent **object**. For example `parent: contacts` (where 'contacts' is a stateObject)


1. **Dot Notation** :- 

   ```javascript
   $stateProvider
     .state('contacts', {})
     .state('contacts.list', {});
   ```

2. stateHelper Module

   ```javascript
   angular.module('myApp', ['ui.router', 'ui.router.stateHelper'])
     .config(function(stateHelperProvider){
       stateHelperProvider.state({
         name: 'root',
         templateUrl: 'root.html',
         children: [
           {
             name: 'contacts',
             templateUrl: 'contacts.html',
             children: [
               {
                 name: 'list',
                 templateUrl: 'contacts.list.html'
               }
             ]
           },
           {
             name: 'products',
             templateUrl: 'products.html',
             children: [
               {
                 name: 'list',
                 templateUrl: 'products.list.html'
               }
             ]
           }
         ]
       });
     });
   ```

3. Parent Property using State Name String

   ```javascript
   $stateProvider
     .state('contacts', {})
     .state('list', {
       parent: 'contacts'
     });
   ```

4. Object Based States

   ```javascript
   var contacts = { 
       name: 'contacts',
       templateUrl: 'contacts.html'
   }
   var contactsList = { 
       name: 'list',
       parent: contacts,
       templateUrl: 'contacts.list.html'
   }

   $stateProvider
     .state(contacts)
     .state(contactsList)
   ```



**Registering State Order**

You can register states in any order and across modules. You can register children before the parent state exists. It will queue them up and once the parent state is registered then the child will be registered.

**Parent Must Exist** :- If you register only a single state, like `contacts.list`, you MUST define a state called `contacts`at some point, or else no states will be registered. The state `contacts.list` will get queued until `contacts` is defined. You will not see any errors if you do this, so be careful that you define the parent in order for the child to get properly registered.

**Namming your states** :- No two states can have the same name. When using dot notation the `parent` is inferred, but this doesn't change the name of the state. When explicitly providing a parent using the `parent`property, state names still must be unique. For example, you can't have two different states named "edit" even if they have different parents.

## Nestes States and Views

When the application is in a particular state—when a state is "active"—all of its ancestor states are implicitly active as well. Below, when the "contacts.list" state is active, the "contacts" state is implicitly active as well, because it's the parent state to "contacts.list".

Child states will load their templates into their parent's `ui-view`.

## What Do chiild States Inherit from parent states

1. [Resolved dependencies via `resolve`](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#wiki-inherited-resolved-dependencies)

   Child states will inherit resolved dependencies from parent state(s), which they can overwrite. You can then inject resolved dependencies into the controllers and resolve functions of child states.

   ```javascript
   $stateProvider.state('parent', {
         resolve:{
            resA:  function(){
               return {'value': 'A'};
            }
         },
         controller: function($scope, resA){
             $scope.resA = resA.value;
         }
      })
      .state('parent.child', {
         resolve:{
            resB: function(resA){
               return {'value': resA.value + 'B'};
            }
         },
         controller: function($scope, resA, resB){
             $scope.resA2 = resA.value;
             $scope.resB = resB.value;
         }
   ```

   **NOTE**:

   - The `resolve` keyword MUST be relative to `state` not `views` (in case you use multiple views).
   - If you want a child resolve to wait for a parent resolve, you should inject the parent resolve keys into the child. (This behavior is [different in ui-router 1.0](https://ui-router.github.io/guide/ng1/migrate-to-1_0#lazy-resolves)).

2. [Custom `data` properties](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#wiki-inherited-custom-data)

   Child states will inherit data properties from parent state(s), which they can overwrite.

   ```javascript
   $stateProvider.state('parent', {
         data:{
            customData1:  "Hello",
            customData2:  "World!"
         }
      })
      .state('parent.child', {
         data:{
            // customData1 inherited from 'parent'
            // but we'll overwrite customData2
            customData2:  "UI-Router!"
         }
      });

   $rootScope.$on('$stateChangeStart', function(event, toState){ 
       var greeting = toState.data.customData1 + " " + toState.data.customData2;
       console.log(greeting);

       // Would print "Hello World!" when 'parent' is activated
       // Would print "Hello UI-Router!" when 'parent.child' is activated
   })
   ```

   ​

3. children of [abstract states](https://github.com/angular-ui/ui-router/wiki/Nested-States-and-Nested-Views#abstract-states) do inherit the [`url` property of their parent as a prefix of their own `url`](https://github.com/angular-ui/ui-router/wiki/Nested-States-and-Nested-Views#abstract-state-usage-examples).

4. Scope Inheritance by View Hierarchy Only

5. View Inherited Resolved Dependencies

   Views **may** inherit resolved dependencies from the state that they belong to, but **may not** inherit those of their sibling views.

   ```javascript
   $stateProvider.state('myState', {
     resolve:{
        resMyState:  function(){
           return { value: 'mystate' };
        }
     },
     views: {
       'foo@myState': {
         templateUrl: 'mystate-foo.html',
         controller: function($scope, resMyState, resFoo){ 
           /* has access to resMyState and resFoo,
              but *not* resBar */ 
         },
         resolve: {
           resFoo: function() {
             return { value: 'foo' };
           }
         },
       },
       'bar@myState': {
         templateUrl: 'mystate-bar.html',
         controller: function($scope, resMyState, resBar){ 
           /* has access to resMyState and resBar,
              but *not* resFoo */ 
         },
         resolve: {
           resBar: function() {
             return { value: 'bar' };
           },
         },
       },
     },
   });
   ```

   ​

## Abstract States

An abstract state can have child states but can not get activated itself. An 'abstract' state is simply a state that can't be transitioned to. It is activated implicitly when one of its descendants are activated.

Some examples of how you might use an abstract state are

- To prepend a [`url`](https://github.com/angular-ui/ui-router/wiki/URL-Routing#url-routing-for-nested-states) to all child state urls.

  ```javascript
  $stateProvider
      .state('contacts', {
          abstract: true,
          url: '/contacts',

          // Note: abstract still needs a ui-view for its children to populate.
          // You can simply add it inline here.
          template: '<ui-view/>'
      })
      .state('contacts.list', {
          // url will become '/contacts/list'
          url: '/list'
          //...more
      })
      .state('contacts.detail', {
          // url will become '/contacts/detail'
          url: '/detail',
          //...more
      })
  ```


- To insert a `template` with its own ui-view(s)  that its child states will populate.
  - Optionally assign a `controller` to the template. The controller **must** pair to a template.
  - Additionally, inherit $scope objects down to children, just understand that this happens via the [view hierarchy](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#scope-inheritance-by-view-hierarchy-only), not the state hierarchy.
- To [provide resolved dependencies](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#inherited-resolved-dependencies) via `resolve` for use by child states.
- To [provide inherited custom data](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#inherited-custom-data) via `data` for use by child states or an event listener.
- To run an `onEnter` or `onExit` function that may modify the application in someway.
- Any combination of the above.

**Remember:** Abstract states still need their own `<ui-view/>` for their children to plug into. So if you are using an abstract state just to prepend a url, set resolves/data, or run an onEnter/Exit function, then you'll additionally need to set `template: "<ui-view/>"`

## Multiple Named Views

When setting multiple views, you need to use the `views` property on the state object. `views` is an object.

1. **Views override state's template properties** :- If you define a `views` object, your state's `templateUrl`, `template` and `templateProvider` will be ignored.

   **Name Matching**

   ```html
   <body>		
     <div ui-view="filters"></div>		
     <div ui-view="tabledata"></div>		
     <div ui-view="graph"></div>		
   </body>	
   ```

   ```javascript
   $stateProvider		
     .state('report',{		
       views: {		
         'filters': {		
           templateUrl: 'report-filters.html',		
           controller: function($scope){ ... controller stuff just for filters view ... }
         },		
         'tabledata': {		
           templateUrl: 'report-table.html',		
           controller: function($scope){ ... controller stuff just for tabledata view ... 			}		
         },		
         'graph': {		
           templateUrl: 'report-graph.html',		
           controller: function($scope){ ... controller stuff just for graph view ... }
         }		
       }		
     })
   ```

2. View Names -Relative vs Absolute Names :- Behind the scenes, every view gets assigned an absolute name that follows a scheme of `viewname@statename`

   Absolute naming lets us do some powerful view targeting. [Remember! With power comes responsibility](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#scope-inheritance-by-view-hierarchy-only). Let's assume we had several templates set up like this (this example is not realistic, it's just to illustrate view targeting):

   ```html
   <!-- index.html (root unnamed template) -->		
   <body ng-app>		
   <div ui-view></div> <!-- contacts.html plugs in here -->		
   <div ui-view="status"></div>		
   </body>	

   <!-- contacts.html -->		
   <h1>My Contacts</h1>		
   <div ui-view></div>		
   <div ui-view="detail"></div>	

   <!-- contacts.detail.html -->		
   <h1>Contacts Details</h1>		
   <div ui-view="info"></div>
   ```

   ```javascript
   $stateProvider		
     .state('contacts', {		
       // This will get automatically plugged into the unnamed ui-view 		
       // of the parent state template. Since this is a top level state, 		
       // its parent state template is index.html.		
       templateUrl: 'contacts.html'   		
     })		
     .state('contacts.detail', {		
       views: {		
           ////////////////////////////////////		
           // Relative Targeting             //		
           // Targets parent state ui-view's //		
           ////////////////////////////////////		
   		
           // Relatively targets the 'detail' view in this state's parent state, 'contacts'.		
           // <div ui-view='detail'/> within contacts.html		
           "detail" : { },            		
   		
           // Relatively targets the unnamed view in this state's parent state, 'contacts'.		
           // <div ui-view/> within contacts.html		
           "" : { }, 		
   		
           ///////////////////////////////////////////////////////		
           // Absolute Targeting using '@'                      //		
           // Targets any view within this state or an ancestor //		
           ///////////////////////////////////////////////////////		
   		
           // Absolutely targets the 'info' view in this state, 'contacts.detail'.		
           // <div ui-view='info'/> within contacts.detail.html		
           "info@contacts.detail" : { }		
   		
           // Absolutely targets the 'detail' view in the 'contacts' state.		
           // <div ui-view='detail'/> within contacts.html		
           "detail@contacts" : { }		
   		
           // Absolutely targets the unnamed view in parent 'contacts' state.		
           // <div ui-view/> within contacts.html		
           "@contacts" : { }		
   		
           // absolutely targets the 'status' view in root unnamed state.		
           // <div ui-view='status'/> within index.html		
           "status@" : { }		
   		
           // absolutely targets the unnamed view in root unnamed state.		
           // <div ui-view/> within index.html		
           "@" : { } 		
     });		
   ```



# URL Routing

```javascript
$stateProvider
    .state('contacts', {
        url: "/contacts",
        templateUrl: 'contacts.html'
    })
```

## URL Parameters

1. Basic Parameters

   ```javascript
   url: "/contacts/:contactId",
   // identical to previous example
   url: "/contacts/{contactId}" 
   ```

2. Using Parameters in links

   To create a link that passes parameters, use the state name like a function and pass it an object with parameter names as keys. The proper `href` will be generated.

   ```html
   <a ui-sref="contacts.detail({contactId: id})">View Contact</a>
   ```

3. Regular expressions

   ```javascript
   url: "/contacts/{contactId:[0-9]{1,8}}"
   ```

4. Query Parameters

   ```javascript
   url: "/contacts?myParam"

   url: "/contacts?myParam1&myParam2"
   ```

5. Using Parameters without Specifying then in State URLs

   ```javascript
   .state('contacts', {
           url: "/contacts",
           params: {
               param1: null
           },
           templateUrl: 'contacts.html'
       })
       '<a ui-sref="contacts({param1: value1})">View Contacts</a>'
       
       $state.go('contacts', {param1: value1})
   ```

   ​

## URL Routing for Nested States

1. **Appended Routes(Default)**

   When using url routing together with nested states the default behavior is for child states to append their url to the urls of each of its parent states.

   ```javascript
   $stateProvider
     .state('contacts', {
        url: '/contacts',
        ...
     })
     .state('contacts.list', {
        url: '/list',
        ...
     });
   ```

   So the routes would become:

   - **'contacts' state** matches `"/contacts"`
   - **'contacts.list' state** matches `"/contacts/list"`. The urls were combined.

2. Absolute Routes (^)

   If you want to have absolute url matching, then you need to prefix your url string with a special symbol '^'.

   ```javascript
   $stateProvider
     .state('contacts', {
        url: '/contacts',
        ...
     })
     .state('contacts.list', {
        url: '^/list',
        ...
     });
   ```

   So the routes would become:

   - **'contacts' state** matches `"/contacts"`
   - **'contacts.list' state** matches `"/list"`. The urls were **not** combined because `^` was used.

## $stateParams Service

In state controllers, the `$stateParams` object will only contain the params that were registered with that state. So you will not see params registered on other states, including ancestors.

```javascript
$stateProvider.state('contacts.detail', {
   url: '/contacts/:contactId',   
   controller: function($stateParams){
      $stateParams.contactId  //*** Exists! ***//
   }
}).state('contacts.detail.subitem', {
   url: '/item/:itemId', 
   controller: function($stateParams){
      $stateParams.contactId //*** Watch Out! DOESN'T EXIST!! ***//
      $stateParams.itemId //*** Exists! ***//  
   }
})
```

## $urlRouterProvider

There are several methods on $urlRouterProvider that make it useful to use directly in your module config.

1. when() for redirection

   ```javascript
   $urlRouterProvider.when(what, handler);

   1. handler as String
   app.config(function($urlRouterProvider){
       // when there is an empty route, redirect to /index   
       $urlRouterProvider.when('', '/index');

       // You can also use regex for the match parameter
       $urlRouterProvider.when(/aspx/i, '/index');
   })
   2. handler as Function :- If the handler is a function, it is injectable. It gets invoked if $location matches. You have the option of inject the match object as $match
   The handler can return:
   falsy to indicate that the rule didn't match after all, then $urlRouter will continue trying to find another one that matches.
   a String, which is treated as a redirect and passed to $location.url()
   nothing or any truthy value tells $urlRouter that the url was handled
   $urlRouterProvider.when(state.url, ['$match', '$stateParams', function ($match, $stateParams) {
       if ($state.$current.navigable != state || !equalForKeys($match, $stateParams)) {
           $state.transitionTo(state, $match, false);
       }
   }]);
   ```

2. otherwise() for invalid routes

   ```javascript
   app.config(function($urlRouterProvider){
       // if the path doesn't match any of the urls you configured
       // otherwise will take care of routing the user to the specified url
       $urlRouterProvider.otherwise('/index');

       // Example of using function rule as param
       $urlRouterProvider.otherwise(function($injector, $location){
           ... some advanced code...
       });
   })	
   ```

3. rule() for custom url handling

   ```javascript
   app.config(function ($urlRouterProvider) {
      // Here's an example of how you might allow case insensitive urls
      // Note that this is an example, and you may also use 
      // $urlMatcherFactory.caseInsensitive(true); for a similar result.
      $urlRouterProvider.rule(function ($injector, $location) {
          //what this function returns will be set as the $location.url
           var path = $location.path(), normalized = path.toLowerCase();
           if (path != normalized) {
               //instead of returning a new url string, I'll just change the $location.path directly so I don't have to worry about constructing a new url string and so a new state change is not triggered
               $location.replace().path(normalized);
           }
           // because we've returned nothing, no state change occurs
       });
   }) 
   ```



## $urlMatcherFactory and UrlMatchers

Defines the syntax for url patterns and parameter placeholders. This factory service is used behind the scenes by $urlRouterProvider to cache compiled UrlMatcher objects, 

instead of having to re-parse url patterns on every location change. Most users will not need to use $urlMatcherFactory directly, however it could be useful to craft a UrlMatcher object and pass it as the url to the state config.

```javascript
var urlMatcher = $urlMatcherFactory.compile("/home/:id?param1");
$stateProvider.state('myState', {
    url: urlMatcher 
});
```



## The Components

- **$state** / **$stateProvider**: Manages state definitions, the current state, and state transitions. This includes triggering transition-related events and callbacks, asynchronously resolving any dependencies of the target state, and updating $location to reflect the current state. For states that have URLs, a rule is automatically registered with $urlRouterProvider that performs a transition to that state.
- **ui-sref** directive: Equivalent to href or ng-href in `<a />` elements except the target value is a state name. Adds an appropriate href to the element according to the state associated URL.
- **ui-view** directive: Renders views defined in the current state. Essentially ui-view directives are (optionally named) placeholders that gets filled with views defined in the current state.
- **$urlRouter** / **$urlRouterProvider**: Manages a list of *rules* that are matched against $location whenever it changes. At the lowest level, a rule can be an arbitrary function that inspects $location and returns true if it was handled. Support is provided on top of this for RegExp rules and URL patterns that are compiled into UrlMatcher objects via $urlMatcherFactory.
- **$urlMatcherFactory**: Compiles URL patterns with placeholders into UrlMatcher objects. In addition to the placeholder syntax supported by $routeProvider, it also supports an extended syntax that allows a regexp to be specified for the placeholder, and has the ability to extract named parameters from the query part of the URL.
- **$templateFactory**: Loads templates via $http / $templateCache. Used by $state.