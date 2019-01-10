# Javascript Performance Optimization

## 1. Avoid interaction with host objects

![Repeated interaction with host objects will kill your performance.](http://desalasworks.com/wp-content/uploads/javascript-host-object-agents.jpg)

Watch out for these guys. Repeated interaction with them will kill your performance.

**THE PROBLEM:**

Native JavaScript is compiled into machine code by most scripting engines offering incredible performance boost, however interaction with host (browser) objects outside the javascript native environment raises unpredictability and considerable performance lag, particularly when dealing with screen-rendered DOM objects or objects which cause Disk I/O (such as WebSQL).

**THE SOLUTION:**

You can’t really get away from them, but keep your interaction with host objects to an absolute minimum.

**THE TECHNIQUES:**

1. #### Use CSS classes instead of JavaScript for DOM animation.

   Its a good habit to try an implement *any animation* (or DOM interaction) with CSS if you can get away with it. [CSS3 Transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) have been around for a while now, so there are few excuses *not* to use them. You can even [use a polyfill](http://stackoverflow.com/questions/5344745/polyfill-shim-for-css-transitions-and-animations) if you are worried about older browsers. Think also of hover menus using the `:hover` pseudo-class, or styling and display of elements using [`@keyframes`, `:before` and `:after`](http://tympanus.net/codrops/2013/05/22/examples-of-pseudo-elements-animations-and-transitions/), this is because unlike JavaScript, CSS solutions are heavily optimized by the browser, often down to the level of [using the GPU for extra processing power](https://www.urbaninsight.com/2013/01/04/improving-html5-app-performance-gpu-accelerated-css-transitions).

   I realize this might sound like irony (if you want to optimize JavaScript – avoid using it for animation), but the reality is that this technique is executed from within your JavaScript code, it just involves putting more effort on the CSS classes.

2. #### Use fast DOM traversal with `document.getElementById()`.

   Given the availability of jQuery, it is now easier than ever to produce highly specific selectors based on a combination of tag names, classes and CSS3. You need to be aware that this approach involves several iterations while jQuery loops thorough each subset of DOM elements and tries to find a match. You can improve DOM traversal speeds by picking nodes by ID.

   ```
   // jQuery will need to iterate many times until it finds the right element
   var button = jQuery('body div.dialog > div.close-button:nth-child(2)')[0];

   // A far more optimized way is to skip jQuery altogether.
   var button = document.getElementById('dialog-close-button');

   // But if you need to use jQuery you can do it this way.
   var button = jQuery('#dialog-close-button')[0];

   ```

3. #### Store pointer references to in-browser objects.

   Use this technique to reduce DOM traversal trips by storing references to browser objects during instantiation for later usage. For example, if you are not expecting your DOM to change you should store a reference to DOM or jQuery objects you are going to use when your page is created; if you are building a DOM structure such as a dialog window, make sure you store a few handy reference to DOM objects inside it during instantiation, so you dont need to find the same DOM object over an over again when a user clicks on something or drags the dialog window.

   If you haven’t stored a reference to a DOM object, and you need to iterate inside a function, you can create a local variable containing a reference to that DOM object, this will considerably speed up the iteration as the local variable is stored in the most accessible part of the stack.

4. #### Keep your HTML super-lean (get rid of all those useless DIV and SPAN tags)

   This is extremely important, the time needed to query and modify DOM is directly proportional the the amount and complexity of HTML that needs to be rendered.

   Using half the amount of HTML will roughly double the DOM speed, and since DOM creates the greatest performance drag on any complex JavaScript app, this can produce a considerable improvement. See [‘Reduce Number of DOM Elements’ guidance](http://developer.yahoo.com/performance/rules.html#min_dom) in Yahoo YSlow.

5. #### Batch your DOM changes, especially when updating styles.

   When making calls to modify DOM make sure you batch them up so as to avoid repeated screen rendering, for example when applying styling changes. The ideal approach here is to make many styling changes in one go by adding or removing a class, rather than apply each individual style separately. This is because every DOM change prompts the browser to re-render the whole UI using the boxing model. If you need to move an item across the page using X+Y coordinates, make sure that these two are applied at the same time rather than separately. See these examples in jQuery:

   ​

   ```
   // This will incurr 5 screen refreshes
   jQuery('#dialog-window').width(600).height(400).css('position': 'absolute')
                          .css('top', '200px').css('left', '200px');
   ```

   ```
   // Let jQuery handle the batching
   jQuery('#dialog-window').css({
        width: '600px',
        height: '400px',
        position: 'absolute',
        top: '200px',
        left: '200px'
   );
   ```

   ```
   // Or even better use a CSS class.
   jQuery('#dialog-window').addClass('mask-aligned-window');
   ```

6. #### Build DOM separately before adding it to the page.

   As per the last item, every DOM update requires the whole screen to be refreshed, you can minimize the impact here by building DOM for your widget ‘off-line’ and then appending your DOM structure in one go.

7. #### Use buffered DOM inside scrollable DIVs.

   This is an extension of the fourth point above (Keep HTML super-lean), you can use this technique to remove items from DOM that are not being visually rendered on screen, such as the area outside the viewport of a scrollable DIV, and append the nodes again when they are needed. This will reduce memory usage and DOM traversal speeds. Using this technique the guys at ExtJS have managed to produce an [infinitely scrollable grid](http://www.sencha.com/blog/infinite-grid-scrolling-in-ext-js-4) that doesn’t grind the browser down to a halt.

## 2. Manage and Actively reduce your Dependencies

Poorly managed JavaScript dependencies degrade user experience.

**THE PROBLEM:**

On-screen visual rendering and user experience is usually delayed while waiting for script dependencies load onto the browser. This is particularly bad for mobile users who have limited bandwidth capacity.

**THE SOLUTION:**

Actively manage and reduce dependency payload in your code.

**THE TECHNIQUES: **

1. #### Write code that reduces library dependencies to an absolute minimum.

   Use this approach to reduce the number of libraries your code requires to a minimum, ideally to none, thus creating an incredible boost to the loading times required for your page.

   You can reduce dependency on external libraries by making use of as much in-browser technology as you can, for example you can use `document.getElementById('nodeId')` instead of `jQuery('#nodeId')`, or `document.getElementsByTagName('INPUT')` instead of `jQuery('INPUT')` which will allow you to get rid of jQuery library dependency.

   If you need complex CSS selectors use [Sizzle.js ](http://sizzlejs.com/)instead of jQuery, which is far more lightweight (4kb instead of 80kb+).

   Also, before adding any new library to the codebase, evaluate whether or you really need it. Perhaps you are just after 1 single feature in the whole library? If that’s the case then take the code apart and add the feature separately (but don’t forget to check the license and acknowledge author if necessary).

2. #### Minimize and combine your code into modules.

   You can bundle distinct components of your application into combined *.js files and pass them through a javascript minimizer tool such as [Google Closures](http://code.google.com/closure/) or [JsMin ](http://www.crockford.com/javascript/jsmin.html)that gets rid of comments and whitespacing.

   The logic here is that a single minimized request for a 10Kb .js file [completes faster ](http://developer.yahoo.com/performance/rules.html#num_http)than 10 requests for files that are 1-2kb each due to lower bandwidth usage and network latency.

3. #### Use a post-load dependency manager for your libraries and modules.

   Much of your functionality will not need to be implemented until after the page loads. By using a dependency manager (such as [RequireJS](http://requirejs.org/) or [Webpack](http://webpack.github.io/)) to load your scripts after the page has completed rendering you are giving the user a few extra seconds to familiarise themselves with the layout and options before them.

   Make sure that your dependency manager can ‘remember’ which dependencies have been loaded so you dont end up loading the same libraries twice for each module. See guidance for [Pre-Loading and Post-loading](http://developer.yahoo.com/performance/rules.html#postload) in Yahoo YSLow, and be mindful about loading only what is necessary at each stage of the user journey.

4. #### Maximise use of caching (eTags, .js files, etc).

   Cache is your best friend when it comes to loading pages faster. Try to maximise the use of cache by [applying ETags liberally](http://developer.yahoo.com/blogs/ydn/posts/2007/07/high_performanc_11/) and putting all your javascript into files ending in *.js found in static URI locations (avoid dynamic Java/C# bundle generations ending with *.jsp and *.ashx) . This will tell the browser to use the locally cached copy of your scripts for any pages loaded after the initial one.

5. #### Move scripts to the end of the page (not recommended).

   This is the lazy way of handling post-load dependencies, ideally you should implement a post-load dependency manager, but if you only have one or two scripts to load into the page you can add them at the very end of the HTML document where the browser will start loading them after the page is rendered, giving the user a few extra seconds of interaction.

## 3. Be disciplined with event binding

![Be a ninja when using event handling.](http://desalasworks.com/wp-content/uploads/javascript-event-handling.png)

Be a ninja when using event handling.

**THE PROBLEM:**

Browser and custom event handlers are an incredible tool for improving user experience and reducing the depth of the call stack (so you avoid having a function calling a function which calls another function etc), but since they are hard to track due to their ‘hidden’ execution they can fire many times repeatedly and quickly get out of hand, causing performance degradation.

**THE SOLUTION:**

Be mindful and disciplined when creating event handlers. Get to know your weapons too, if you are using a framework then find out what’s going on underneath the hood.

**THE TECHNIQUES:**

1. #### Use event binding but do it carefully.

   Event binding is great for creating responsive applications. However, it is important that you walk through the execution and various user journeys *to make sure they are not firing multiple times* or using up unnecessary resources behind the scenes. Comment your code well so they next guy (which may be you a few months down the line) can follow what’s going on and avoid this issue as well.

   If you are using AngularJS make sure you are getting [rid of unnecessary ‘watchers’](http://www.alexkras.com/11-tips-to-improve-angularjs-performance/#watchers). These are background events that involve heavy processing and will slow down your app, particularly on mobile devices.

2. #### Pay special attention event handlers that fire in quick succession (ie, ‘mousemove’).

   Browser events such as ‘mousemove’ and ‘resize’ are executed in quick succession up to *several hundred times each second*, this means that you need to ensure that an event handler bound to either of these events is coded optimally and can complete in less than 2-3 milliseconds.

   Any overhead greater than that will create a patchy user experience, specially in browsers such as IE that have poor rendering capabilities.

3. #### Remember to unbind events when they are not needed.

   Unbinding events is almost as important as binding them. When you add a new event handler to your code make sure that you provide for it to stop firing when it is no longer needed, ideally using once-off execution constructs like *jQuery.one()* or coding in the unbind behaviour at that point. This will avoid you having the same handler bound multiple times degrading performance, or events firing when they are no longer needed, this often points to memory leaks in your app as pointed out by the [React developers in this blog post](https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html).

   If you are using jQuery to bind and unbind events, make sure your selector points to a unique node, as a loose selector can create or remove more handlers than you intend to.

4. #### Learn about event bubbling.

   If you are going to use event handlers, it is important that you understand how event bubbling propagates an event up the DOM tree to every ancestor node. You can use this knowledge to limit your dependency on event bubbling with approaches such as jQuery.live() and jQuery.delegate() that require full DOM traversal upon handling each event, or to  stop event bubbling for improved performance. See this [great post](http://www.alfajango.com/blog/the-difference-between-jquerys-bind-live-and-delegate/) on the subject.

5. #### Use ‘mouseup’ instead of ‘click’.

   Remember that user interaction via the mouse or keyboard fires several events in a specific order. It is useful to remember the order in which these events fire so you can squeeze in your functionality* before anything else gets handled, *including native browser event handlers.

   A good example of this is to bind your functionality to the ‘mouseup’ event which fires *before* the ‘click’ event, this can produce a surprising performance boost in older browsers such as IE, making the difference between handling every interaction or missing some of the action if the user triggers clicks many times in succession.

## 4. Maximise the efficiency of your iterations

![String concatenation performance becomes critical during long iterations.](http://desalasworks.com/wp-content/uploads/javascript-string-performance.png)

Performance becomes critical during long iterations.

**THE PROBLEM:**

Due to the processing time used, iterations are usually the first places where you can address performance flaws in an application.

**THE SOLUTION:**

Get rid of unnecessary loops and calls made inside loops.

**THE TECHNIQUES:**

1. #### Harness the indexing power of JavaScript objects.

   Native JavaScript objects `{}` can be used as powerful [Hashtable data structures](http://en.wikipedia.org/wiki/Hash_table)with quick-lookup indexes to store references to other objects, acting similarly to the way database indexes work for speeding up search operations by preventing needless looping. 

   So why bother iterating to find something? You can simply use a plain object as an index (think of a phone-book) to get to your desired item quickly and efficiently. Here is an example as follows:

   ```
   var data = {
     index: {
               "joeb": {name: "joe", surname: "bloggs", age: 29 },
               "marys": {name: "mary", surname: "smith", age: 25 }
               // another 1000 records
            },
     get: function(username) {
               return this.index[username];
            }
   }
   ```

2. #### Harness the power of array structures with push() and pop() and shift().

   Array `push()` `pop()` and `shift()` instructions have [minimal processing overhead](http://stackoverflow.com/questions/8423493/what-is-the-performance-of-objects-arrays-in-javascript-specifically-for-googl/8584173#8584173)(20x that of object manipulation) due to being language constructs closely related to their low-level assembly language counterparts. In addition, using [queue](http://en.wikipedia.org/wiki/Queue_(abstract_data_type)) and [stack](http://en.wikipedia.org/wiki/Stack_(data_structure))data structures can help simplify your code logic and get rid of unnecessarily loops. See more on the topic in [this article](http://jclaes.blogspot.com/2011/07/stacks-and-queues-in-javascript.html).

3. #### Take advantage of reference types.

   JavaScript, much like other C-based languages, has both [primitive and reference value types](http://docstore.mik.ua/orelly/webprog/jscript/ch11_02.htm). Primitive types such as strings, booleans and integers are copied whenever they are passed into a new function, however reference types such as arrays, objects and dates are passed only as a light-weight reference.You can use this to get the most performance out of recursive functions, such as by passing a DOM node reference recursively to minimise DOM traversal, or by passing a reference parameter into a function that executes within an iteration. Also, remember that comparing object references is far more efficient than comparing strings.

4. #### Use `Array.prototype.join()` for string concatenation.

   **\*PLEASE NOTE:** This article was first written in 2012 when string concatenation was a hazard to be aware of. However, these days most JavaScript engines have compilation tricks that have made this issue obsolete. The wording below is only really relevant for historical purposes.*

   Joining strings using the plus sign (ie `var ab = 'a' + 'b';`) creates performance issues in IE when used within an iteration. This is because, like Java and C#, [JavaScript uses unmutable strings](http://stackoverflow.com/questions/51185/are-javascript-strings-immutable-do-i-need-a-string-builder-in-js).

   Basically, when you concatenate two strings, a third string is constructed for gathering the results with its own object instantiation logic and memory allocation. While other browsers have various compilation tricks around this, IE is particularly bad at it.A far better approach is to use an array for carrying out the donkey work, creating an array outside the loop, using `push()` to add items into to the array and then a `join()` to output the results. See [this link](http://www.sitepen.com/blog/2008/05/09/string-performance-an-analysis/) for a more in-depth article on the subject.

## 5. Become friends with the JavaScript lexicon

![Become a friend of the ECMA Standard and it make your code faster.](http://desalasworks.com/wp-content/uploads/javascript-lexical-performance.png)

Become a friend of the ECMA Standard and it make your code faster.

**THE PROBLEM:**

Due to its loosely-typed and free-for-all nature, JavaScript can be written using a very limited subset of lexical constructs with no discipline or controls applied to its use. Using simple function patterns repetitively often leads to poorly thought-out ‘spaghetti’ code that is inefficient in terms of resource use.

**THE SOLUTION:**

Learn when and how to apply the constructs of the [ECMAScript language standard](http://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-262,%203rd%20edition,%20December%201999.pdf) to maximise performance.

**THE TECHNIQUES:**

1. #### Shorten the scope chain

   In JavaScript, whenever a function is executed, a set of first order variables are instantiated as part of that function. These include the immediate scope of a function (the `this` variable) with its own scope chain, the `arguments` of the function and all locally-declared variables.

   If you try and access a globally-declared variable or a closure further up the scope chain, it will take extra effort to traverse up the chain every level util the compiler can wire up the variable you are after. You can thus improve execution by reducing the depth of the call stack, and by only using the local scope (`this`), the `arguments`of the function, as well as locally declared variables. [This article](http://homepage.mac.com/rue/JS_Optimization_Techniques/) explains the matter further.

2. #### Make use of ‘this’, by passing correct scope using ‘call’ and ‘apply’.

   This is particularly useful for writing asynchronous code using callbacks, however it also improves performance because you are not relying on global or [closure variables](http://www.javascriptkit.com/javatutors/closures.shtml) held further up the scope chain. You can get the most out of the scope variable (`this`) by rewiring it using the [special call() and apply() methods](http://odetocode.com/blogs/scott/archive/2007/07/05/function-apply-and-function-call-in-javascript.aspx) that are built into each function. See the example below:

   ```
   var Person = Object.create({
     init: function(name) {
        this.name = name;
     },
     do: function(callback) {
        callback.apply(this);
     }
   });
   var john = new Person('john');
   john.do(function() {
       alert(this.name); // 'john' gets alerted because we rewired 'this'.
   });
   ```

3. #### Learn and use native functions and constructs.

   ECMAScript provides a whole host of native constructs that save you having to write your own algorithms or rely on host objects. Some examples include `Math.floor()`, `Math.round()`, `(new Date()).getTime()` for timestamps, `String.prototype.match()` and `String.prototype.replace()` for regexes, `parseInt(n, radix)` for changing numeral systems, `===` instead of `==` for faster type-based comparsion, `instanceof` for checking type up the hierarchy, `&` and `|` for [bitwise comparisons](https://developer.mozilla.org/en/JavaScript/Reference/Operators/Bitwise_Operators). And the list goes on and on.

   Make sure you use all these instead of trying to work out your own algorithms as you will not only be reinventing the wheel but affecting performance.

4. #### Use ‘switch’ instead of lengthy ‘if-then-else’ statements.

   This is because  ‘switch’ statements can be [optimized more easily during compilation](http://en.wikipedia.org/wiki/Switch_statement#Compilation). There is an interesting article in O’Reily about using [this approach with JavaScript](http://oreilly.com/server-administration/excerpts/even-faster-websites/writing-efficient-javascript.html#the_switch_statement).