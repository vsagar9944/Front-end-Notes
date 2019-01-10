# [How does data binding work in AngularJS?](https://stackoverflow.com/questions/9682092/how-does-data-binding-work-in-angularjs) 

# Data Binding

Data-binding in AngularJS apps is the automatic synchronization of data between the model and view components. The way that AngularJS implements data-binding lets you treat the model as the single-source-of-truth in your application. The view is a projection of the model at all times. When the model changes, the view reflects the change, and vice versa.

## Data Binding in Classical Template Systems

![img](https://docs.angularjs.org/img/One_Way_Data_Binding.png)
Most templating systems bind data in only one direction: they merge template and model components together into a view. After the merge occurs, changes to the model or related sections of the view are NOT automatically reflected in the view. Worse, any changes that the user makes to the view are not reflected in the model. This means that the developer has to write code that constantly syncs the view with the model and the model with the view.

## Data Binding in AngularJS Templates

![img](https://docs.angularjs.org/img/Two_Way_Data_Binding.png)
AngularJS templates work differently. First the template (which is the uncompiled HTML along with any additional markup or directives) is compiled on the browser. The compilation step produces a live view. Any changes to the view are immediately reflected in the model, and any changes in the model are propagated to the view. The model is the single-source-of-truth for the application state, greatly simplifying the programming model for the developer. You can think of the view as simply an instant projection of your model.

Because the view is just a projection of the model, the controller is completely separated from the view and unaware of it. This makes testing a snap because it is easy to test your controller in isolation without the view and the related DOM/browser dependency.

## Types of data-binding

1. **Two way data-binding** :- By simply throwing **ng-model** on an input tag, the view is able to dynamically update it's underlying data model. After that developer dont worry about event listners, callback and service calls to keep views in sync with the data they represent. **View -> Controller** & **Controller -> View**
2.  **One way data-binding** :- By using **ng-bind** or **{{}}** you create one way data binding. In one way data binding model can only be changed from javascript code not from user input. But this type of binding creates watchers whenever value of mdel change it will automatically update view. **Controller -> View**
3. **One time data-binding** :-  When we dont want to add watcher on any model as the value of the model not get updated at that time we use one time data-binding. We can achive one time data binding using **::** before model name Ex. **ng-bind="::myvar" or {{::myvar}}** **Controller -> View**

# Working of Data-bonding

## By dirty checking the $scope object 

Angular maintains a simple array of watchers in the `$scope`  objects. If you inspect any `$scope` you will find that it contains an array called $$watchers.

## $watch()

This function is used to observe changes in a variable on the $scope. It accepts three parameters: expression, listener and equality object, where listener and equality object are optional parameters.

There are two ways of declaring a `$scope` variable as being watched.

1. By using it in your template via the expression `<span>{{myVar}}</span>`
2. By adding it manually via the `$watch` service

Each watcher is an object that contains among other things

1. An expression which the watcher is monitoring. This might just be an attribute name, or something more complicated.
2. A last known value of the expression. This can be checked against the current computed value of the expression. If the values differ the watcher will trigger the function and mark the $scope as dirty.
3. A function which will be executed if the watcher is dirty.

### Angular adds a watcher to the $watchers for each of these

> 1. {{expression}} — In your templates (and anywhere else where there’s an expression) or when we define ng-model. 
> 2. $scope.$watch(‘expression/function’) — In your JavaScript we can just attach a scope object for angular to watch.

**$watch** function takes in three parameters:

> 1. First one is a watcher function which just returns the object or we can just add an expression. 
>
> 2. Second one is a listener function which will be called when there is a change in the object. All the things like DOM changes will be implemented in this function.
>
> 3. The third being an optional parameter which takes in a boolean . If its true , angular deep watches the object & if its false Angular just does a reference watching on the object. Rough Implementation of $watch looks like this
>
>    ```javascript
>    Scope.prototype.$watch = function(watchFn, listenerFn) {
>       var watcher = {
>           watchFn: watchFn,
>           listenerFn: listenerFn || function() { },
>           last: initWatchVal  // initWatchVal is typically undefined
>       };
>       this.$$watchers.push(watcher); // pushing the Watcher Object to Watchers  
>    };
>    ```

The `$scope.watch()` function creates a watch of some variable. When you register a watch you pass two functions as parameters to the `$watch()` function:

- A value function
- A listener function

Here is an example:

```
$scope.$watch(function() {},
              function() {}
             );
```

The first function is the value function and the second function is the listener function.

The value function should return the value which is being watched. AngularJS can then check the value returned against the value the watch function returned the last time. That way AngularJS can determine if the value has changed. Here is an example:

```
$scope.$watch(function(scope) { return scope.data.myVar },
              function() {}
             );
```

This example valule function returns the `$scope` variable `scope.data.myVar`. If the value of this variable changes, a different value will be returned, and AngularJS will call the listener function.

Notice how the value function takes the `scope` as parameter (without the `$` in the name). Via this parameter the value function can access the `$scope` and its variables. The value function can also watch global variables instead if you need that, but most often you will watch a `$scope` variable.

The listener function should do whatever it needs to do if the value has changed. Perhaps you need to change the content of another variable, or set the content of an HTML element or something. Here is an example:

```
$scope.$watch(function(scope) { return scope.data.myVar },
              function(newValue, oldValue) {
                  document.getElementById("").innerHTML =
                      "" + newValue + "";
              }
             );
```

This example sets the inner HTML of an HTML element to the new value of the variable, embedded in the`b` element which makes the value bold. Of course you could have done this using the code **{{ data.myVar }**, but this is just an example of what you can do inside the listener function.

### How watchers are defined

There are many different ways of defining a watcher in AngularJS.

- Any time you reference a scope variable in an ng-* or use the double brackets {{ }} you create another watcher for the scope.

- You can explicitly `$watch` an attribute on $scope.

  ```
  $scope.$watch('person.username', validateUnique);
  ```

- You can place a {{}} interpolation in your template (a watcher will be created for you on the current $scope).

  ```
  <p>username: {{person.username}}</p>
  ```

- You can ask a directive such as ng-model to define the watcher for you.

  ```
  <input ng-model="person.username" />
  ```

### Destroying A Watch

The `$watch` function in Angular actually has a return value. It is a function that, when invoked, destroys the watch that was registered. 



## Digest Cycle 

This cycle can be considered as a loop, during which AngularJS checks if there are any changes to all the variables **watched** by all the `$scopes`

### $digest()

**$digest()** - This function iterates through all the watches in the `$scope` object, and its child `$scope` objects (if it has any). When `$digest()` iterates over the watches, it checks if the value of the expression has changed. If the value has changed, AngularJS calls the listener with the new value and the old value.  The `$digest()` function is called whenever AngularJS thinks it is necessary. For example, after a button click, or after an AJAX call. You may have some cases where AngularJS does not call the $digest() function for you. In that case you have to call it yourself. The digest cycle is what makes two-way data binding possible in Angular 1. It's also what causes the majority of Angular 1's performance issues. It should be noted that the digest cycle runs every time the app's state changes.These manually trigger the digest cycle, however Angular based DOM manipulations, AJAX calls, and callbacks automatically fire the cycle as well.

The `$scope.$digest()` function iterates through all the watches in the `$scope` object, and its child `$scope`objects (if it has any). When `$digest()` iterates over the watches, it calls the value function for each watch. If the value returned by the value function is different than the value it returned the last time it was called, the listener function for that watch is called.

The `$digest()` function is called whenever AngularJS thinks it is necessary. For instance, after a button click handler has been executed, or after an AJAX call returns (after the `done()` / `fail()` callback function has been executed).

You may encounter some corner cases where AngularJS does not call the `$digest()` function for you. You will usually detect that by noticing that the data bindings do not upate the displayed values. In that case, call `$scope.$digest()` and it should work. Or, you can perhaps use `$scope.$apply()` instead which I will explain in the next section.

There is an interesting thing in Angular called Digest Cycle. The `$digest` cycle starts as a result of a call to $scope.$digest(). Assume that you change a `$scope` model in a handler function through the ng-click directive. In that case AngularJS automatically triggers a `$digest` cycle by calling `$digest()`.In addition to ng-click, there are several other built-in directives/services that let you change models (e.g. ng-model, `$timeout`, etc) and automatically trigger a `$digest` cycle. The rough implementation of $digest looks like this.

```
Scope.prototype.$digest = function() {
      var dirty;
      do {
          dirty = this.$$digestOnce();
      } while (dirty);
}
Scope.prototype.$$digestOnce = function() {
   var self = this;
   var newValue, oldValue, dirty;
   _.forEach(this.$$watchers, function(watcher) {
          newValue = watcher.watchFn(self);
          oldValue = watcher.last;   // It just remembers the last value for dirty checking
          if (newValue !== oldValue) { //Dirty checking of References 
   // For Deep checking the object , code of Value     
   // based checking of Object should be implemented here
             watcher.last = newValue;
             watcher.listenerFn(newValue,
                  (oldValue === initWatchVal ? newValue : oldValue),
                   self);
          dirty = true;
          }
     });
   return dirty;
 };
```

If we use JavaScript’s **setTimeout()** function to update a scope model, Angular has no way of knowing what you might change. In this case it’s our responsibility to call `$apply()` manually, which triggers a `$digest` cycle. Similarly, if you have a directive that sets up a DOM event listener and changes some models inside the handler function, you need to call `$apply()` to ensure the changes take effect. The big idea of `$apply` is that we can execute some code that isn't aware of Angular, that code may still change things on the scope. If we wrap that code in `$apply` , it will take care of calling `$digest()`. Rough implementation of $apply().

```
Scope.prototype.$apply = function(expr) {
       try {
         return this.$eval(expr); //Evaluating code in the context of Scope
       } finally {
         this.$digest();
       }
};
```

The digest cycle will make sure that the UI and the JavaScript code stays synchronised, by evaluating every watcher attached to the all `$scope`s as long as nothing changes. If no more changes happen in the digest loop, then it's considered to be finished.

## The $digest cycle checks all watchers against their last value

When we interact with angular through the normal channels (ng-model, ng-repeat, etc) a digest cycle will be triggered by the directive.

A digest cycle is a **depth first traversal of $scope and all its children**. For each `$scope` object, we iterate over its $$watchers array and evaluate all the expressions. If the new expression value is different from the last known value, the watcher's function is called. This function might recompile part of the DOM, recompute a value on $scope, trigger an AJAX request, anything you need it to do.

Every scope is traversed and every watch expression evaluated and checked against the last value.

## If a watcher is triggered, the $scope is dirty

If a watcher is triggered, the app knows something has changed, and the $scope is marked as dirty.

Watcher functions can change other attributes on `$scope` or on a parent `$scope`. If one $watcher function has been triggered, we can't guarantee that our other $scopes are still clean, and so we execute the entire digest cycle again.

This is because Angular 1 has two way binding, so data can be passed back up the $scope tree. We may change a value on a higher $scope that has already been digested. Perhaps we change a value on the $rootScope.

## If the $digest is dirty, we execute the entire $digest cycle again

We continually loop through the $digest cycle until either the digest cycle comes up clean (all $watch expressions have the same value as they had in the previous cycle), or we reach the digest limit. By default, this limit is set at 10.

If we reach the digest limit Angular will raise an error in the console:

```
10 $digest() iterations reached. Aborting!
```

## The digest is hard on the machine but easy on the developer

As you can see, every time something changes in an Angular app, Angular will check every single watcher in the $scope hierarchy to see how to respond. For a developer this is a massive productivity boon, as you now need to write almost no wiring code, Angular will just notice if a value has changed, and make the resto of the app consistent with the change.

From the perspective of the machine though this is wildly inefficient and will slow our app down if we create too many watchers. Misko has quoted a figure of about 4000 watchers before your app will feel slow on older browsers.

This limit is easy to reach if you ng-repeat over a large JSON array for example. You can mitigate against this using features like one-time binding to compile a template without creating watchers.

## How to avoid creating too many watchers

Each time your user interacts with your app, every single watcher in your app will be evaluated at least once. A big part of optimising an Angular app is reducing the number of watchers in your $scope tree. One easy way to do this is with *one time binding*.

If you have data which will rarely change, you can bind it only once using the :: syntax, like so:

```
<p>{{::person.username}}</p>
```

or

```
<p ng-bind="::person.username"></p>
```

The binding will only be triggered when the containing template is rendered and the data loaded into $scope.

This is especially important when you have an `ng-repeat` with many items.

```
<div ng-repeat="person in people track by username">
  {{::person.username}}
</div>
```

## $apply()

The `$scope.$apply()` function takes a function as parameter which is executed, and after that`$scope.$digest()` is called internally. That makes it easier for you to make sure that all watches are checked, and thus all data bindings refreshed. Here is an `$apply()` example:

```
$scope.$apply(function() {
    $scope.data.myVar = "Another value";
});

```

The function passed to the `$apply()` function as parameter will change the value of `$scope.data.myVar`. When the function exits AngularJS will call the `$scope.$digest()` function so all watches are checked for changes in the watched values.

It lets you to start the digestion cycle explicitly. However, you should only use this to migrate some data to AngularJS (integration with other frameworks), but never use this method combined with regular AngularJS code, as AngularJS will throw an error then.

You can think of the **$apply function as of an integration mechanism**. You see, each time you change some **watched variable attached to the $scope** object directly, AngularJS will know that the change has happened. This is because AngularJS already knew to monitor those changes. So if it happens in code managed by the framework, the digest cycle will carry on.

-----

# Some Important Information to $digest

*$digest()* is the third scope function related to data binding, and it’s the most important one. With high probability, you will never actually call *$digest()* directly, since *$apply()* does that for you. However, this function is at the core of all data binding magic, and its internals warrant some careful inspection if you’re going to be an AngularJS pro.

At a high level, *$digest()* runs through every watcher in the scope, evaluates the expression, and checks if the value of the expression has changed. If the value has changed, AngularJS calls the change callback with the new value and the old value. Simple, right? Well, not quite, there are a few subtleties.

1. The first subtlety is with the change callback: the change callback itself can change the scope, like we did with the name example in the *$watch()* section. If we had a watcher on *firstName*, this watcher wouldn’t fire! This is why *$digest()* is executed in a loop: *$digest()* will repeatedly execute all watchers until it goes through all watchers once without any of the watched expressions changing. In AngularJS internals, a dirty flag is set on each iteration of the *$digest()* loop when a change callback needs to be fired. If the dirty flag is not set after an iteration, *$digest()* terminates.

   Of course, the *$digest()* loop described above can run forever, which is very bad. Internally, AngularJS uses a questionably-named field TTL (presumably “times to loop”) field to determine the maximum number of times a *$digest()* loop will run before giving up. By default, TTL is 10, and you will usually not run into this limit unless you have an infinite loop. If you for some reason need to tweak the TTL, the [AngularJS root scope](http://docs.angularjs.org/api/ng.$rootScope) has a poorly-documented *digestTtl()* function which you can use to change the TTL on a per-page basis. You can read more about this function [here](https://groups.google.com/forum/#!topic/angular/Fn8ujAx2OP4).

2. The second subtlety is another interesting corner case with the change callback: what if the change callback calls *$apply()* or *$digest()*? Internally, AngularJS uses a system of phases to make sure this doesn’t happen: an error gets thrown if you try to enter the *$digest* phase while you’re already in the *$digest* phase.

3. Remember when we said that AngularJS scopes are organized in a tree structure and a scope can access its ancestor’s variables? Well, this means that *$digest()* needs to happen on every child scope in every iteration! Internally, this code is a bit messy in AngularJS, but each iteration of the *$digest()* loop does a depth-first search and performs the watcher check on every child scope. If any child scope is dirty, the loop has to run again!

4. Since Javascript is a single-threaded event-driven language, the *$digest()* loop cannot be interrupted. That means that the UI is blocked while *$digest()* is running, which means two very bad things happen when your *$digest()* is slow: your UI does not get updated until *$digest()* is done running, and your UI will not respond to user input, e.g. typing in an input field, until *$digest()* is done. To avoid a bad case of client side in-*$digest*-ion, make sure your event handlers lightweight and fast.

5. The final and often most overlooked subtlety is the question of what we mean when we say “checks if the value of the expression has changed”. Thankfully, AngularJS is a bit smarter than just using Javascript’s === operator: if AngularJS just used ===, data binding against an array would be very frustrating. Two arrays with the same elements could be considered different! Internally, AngularJS uses [its own equals function](http://docs.angularjs.org/api/angular.equals). This function considers two objects to be equal if === says they’re equal, if angular.equals returns true for all their properties, if isNaN is true for both objects, or if the objects are both regular expressions and their string representations are equal. Long story short, this does the right thing in most situations:

# Value-Based Dirty-Checking

For now we've been comparing the old value to the new with the strict equality operator `===`. This is fine in most cases, as it detects changes to all primitives (numbers, strings, etc.) and also detects when an object or an array changes to a new one. But there is also another way Angular can detect changes, and that's detecting when something *inside* an object or an array changes. That is, you can watch for changes in *value*, not just in reference.

This kind of dirty-checking is activated by providing a third, optional boolean flag to the `$watch` function. When the flag is `true`, value-based checking is used. Let's redefine `$watch` to take this flag and store it in the watcher:

[?](http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html#)

All we do is add the flag to the watcher, coercing it to a true boolean by negating it twice. When a user calls `$watch` without a third argument, `valueEq` will be `undefined`, which becomes `false` in the watcher object.

Value-based dirty-checking implies that we have to iterate through everything contained in the old and new values if they are objects or arrays. If there's any difference in the two values, the watcher is dirty. If the value has other objects or arrays nested within, those will also be recursively compared by value.

Angular ships with [its own equal checking function](https://github.com/angular/angular.js/blob/8d4e3fdd31eabadd87db38aa0590253e14791956/src/Angular.js#L812), but we're going to use [the one provided by Lo-Dash](http://lodash.com/docs#isEqual) instead. Let's define a new function that takes two values and the boolean flag, and compares the values accordingly:

[?](http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html#)

In order to notice changes in value, we also need to change the way we store the old value for each watcher. It isn't enough to just store a reference to the current value, because any changes made within that value will also be applied to the reference we're holding. We would never notice any changes since essentially `$$areEqual` would always get two references to the same value. For this reason we need to make a deep copy of the value and store that instead.

Just like with the equality check, Angular ships with [its own deep copying function](https://github.com/angular/angular.js/blob/8d4e3fdd31eabadd87db38aa0590253e14791956/src/Angular.js#L725), but we'll be using [the one that comes with Lo-Dash](http://lodash.com/docs#cloneDeep). Let's update `$digestOnce` so that it uses the new `$$areEqual` function, and also copies the `last` reference if needed:

[?](http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html#)

Now we can see the difference between the two kinds of dirty-checking:

Checking by value is obviously a more involved operation than just checking a reference. Sometimes a lot more involved. Walking a nested data structure takes time, and holding a deep copy of it also takes up memory. That's why Angular does not do value-based dirty checking by default. You need to explicitly set the flag to enable it.

There's also a third dirty-checking mechanism Angular provides: Collection watching. Just like value-based checking, it notices changes within objects and arrays. Unlike value-based checking, it does a shallow check and does not recurse onto deeper levels. This makes it more performant than value-based checking. Collection watching is available via the `$watchCollection` function. We'll take a look at how it's implemented later in the series.

Before we're done with value comparison, there's one more JavaScript quirk we need to handle.

## NaNs

In JavaScript, `NaN` (Not-a-Number) is not equal to itself. This may sound strange, and that's because it is. If we don't explicitly handle `NaN` in our dirty-checking function, a watch that has `NaN` as a value will always be dirty.

For value-based dirty-checking this case is already handled for us by the Lo-Dash `isEqual`function. For reference-based checking we need to handle it ourselves. Let's do that by tweaking the `$$areEqual` function:

[?](http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html#)

Now the watch behaves as expected with `NaN`s as well:

With the value-checking implementation in place, let's now switch our focus to the ways in which you can interact with a scope from application code.

----

http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html

http://tahsinscreation.com/mysite/resumate-updated/single-blog.html
http://ahmedbeheiry.tk/works/tf/marqa/
http://www.demo.codiux.com/pulsev1/?section=4
http://www.demo.codiux.com/pulsev2/?section=7
http://www.webdingo.net/precision/
http://preview.themeforest.net/item/flexyvcard-responsive-vcard-template-/full_screen_preview/7158750?ref=freshdesignweb&clickthrough_id=1086429386&redirect_back=true

http://tutlane.com/



declarative code is better than imperative Code

How do search engines deal with AngularJS applications?
https://stackoverflow.com/questions/13499040/how-do-search-engines-deal-with-angularjs-applications/23245379#23245379

What are the nuances of scope prototypal / prototypical inheritance in AngularJS?
https://stackoverflow.com/questions/14049480/what-are-the-nuances-of-scope-prototypal-prototypical-inheritance-in-angularjs


carousel
https://www.npmjs.com/package/angular-jk-carousel
https://embed.plnkr.co/ovBExhpO40yzWPJ47QFE/

https://cipchk.github.io/ng-alain/#/logics/acl
https://github.com/cipchk/ng-alain


http://angularscript.com/angularjs-2-youtube-player/

http://ngmodules.org/

http://www.quartz-scheduler.org/

laxmi.shukla0707@gmail.com

Your Mi Account ID: 1736678435
Email account: laxmi.shukla0707@gmail.com

http://blog.mgechev.com/2017/04/23/angular-tooling-codelyzer-angular-cli-ngrev/

https://github.com/teropa/build-your-own-angularjs

https://jioapps.ril.com/Citrix/jioapps2Web/