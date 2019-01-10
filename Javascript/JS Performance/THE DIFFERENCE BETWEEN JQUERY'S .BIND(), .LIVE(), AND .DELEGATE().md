# THE DIFFERENCE BETWEEN JQUERY'S .BIND(), .LIVE(), AND .DELEGATE()

![jquery-tags-bind-live-delegate](https://www.alfajango.com/blog/wp-content/uploads/2011/01/jquery-tags-bind-live-delegate.jpg)

The difference between [.bind()](http://api.jquery.com/bind/), [.live()](http://api.jquery.com/live/), and [.delegate()](http://api.jquery.com/delegate/) is not always apparent. Having a clear understanding of all the differences, though, will help us write more concise code and prevent bugs from popping up in our interactive applications.

The jQuery team [have announced](http://blog.jquery.com/2011/09/28/jquery-1-7-beta-1-released/) in v1.7 a new method for binding events called `on`. This method combines the functionality of `live`, `bind`, and `delegate` described below, allowing you to specify the binding method based the arguments passed rather than by using different function names.

## The basics

### The DOM tree

First, it helps to visualize the DOM tree of an HTML document. A simple HTML page would look like this:

![HTML DOM Structure](http://chart.apis.google.com/chart?cht=gv&chl=graph{window--document--h1;document--p--span;p--a;document--h2;document--form--input;form--submit;}&chs=550x300)

### Event bubbling (aka event propagation)

When we click a link, it fires the `click` event on the link element, which triggers any functions we bound to that element's click event.

```
$('a').bind('click', function() { alert("That tickles!") });

```

So a click will trigger the alert.

![HTML DOM Structure](http://chart.apis.google.com/chart?cht=gv&chl=digraph{a[color=red];%22That%20tickles%21%22[shape=rectangle];a-%3E%22That%20tickles%21%22[color=red][label=click][fontcolor=red]})

That `click` event then propagates up the tree, broadcasting to the parent element and then to each ancestor element that the `click` event was triggered on one of the descendent elements.

![HTML DOM Structure](http://chart.apis.google.com/chart?cht=gv&chl=digraph{a[color=red];window-%3Edocument[label=%22document%20%3E%20p%20%3E%20a%2Eclick%22][dir=back][color=red][fontcolor=red];document-%3Eh1[dir=none];document-%3Ep[label=%22p%20%3E%20a%2Eclick%22][dir=back][color=red][fontcolor=red];p-%3Espan[dir=none];document-%3Eh2[dir=none];document-%3Eform[dir=none];form-%3Einput[dir=none];form-%3Esubmit[dir=none];p-%3Ea[label=%22a%2Eclick%22][dir=back][color=red][fontcolor=red];}&chs=550x350)

**In the context of manipulating the DOM, `document` is the root node.

Now we can more easily illustrate the difference between `.bind()`, `.live()`, and `.delegate()`.

## .bind()

```
$('a').bind('click', function() { alert("That tickles!") });

```

This is the most straight forward binding method. jQuery scans the document for all `$('a')` elements and binds the alert function to each of their `click` events.

## .live()

```
$('a').live('click', function() { alert("That tickles!") });

```

jQuery binds the alert function to the `$(document)` element, along with `'click'` and `'a'` as parameters. Any time an event bubbles up to the document node, it checks to see if the event was a click and if the target element of that event matches the `'a'` CSS selector. If both are true, the function executes.

The live method can also be bound to a specific element (or "context") other than `document`, like this:

```
$('a', $('#container')[0]).live(...);

```

## .delegate()

```
$('#container').delegate('a', 'click', function() { alert("That tickles!") });

```

jQuery scans the document for `$('#container')`, and binds the alert function along with the `click` event and `'a'` CSS selector as parameters. Any time an event bubbles up to `$('#container')`, it checks to see if the event was a click and if the target element of that event matches the CSS selector. If both checks are true, it executes the function.

Notice this is similar to `.live()`, except that it binds the handler to the specified element instead of the document root. The astute JS'er might conclude that `$('a').live() == $(document).delegate('a')`, right? Well, no, not exactly.

## Why .delegate() is better than .live()

jQuery's delegate method is generally preferred to the live method for a few reasons. Consider the following examples:

```
$('a').live('click', function() { blah() });
// or
$(document).delegate('a', 'click', function() { blah() });

```

### Speed

The latter is actually faster than the former, because the former first scans the entire document for all `$('a')`elements, storing them as jQuery objects. Even though the live function only needs to pass 'a' through as string argument to be evaluated later, the `$()` function doesn't "know" that the chained method is going to be `.live()`.

The delegate method on the other hand, only needs to find and store the `$(document)` element.

**One hack to get around this is to call the live binding outside of the `$(document).ready()` state, so that it runs immediately. That way it will run before the DOM gets populated, and thus won't find the elements or create the jQuery objects.

### Flexibility and chain-ability

The live function is also convoluted. Think about it; it's chained to the `$('a')` object set, but it's actually acting on the `$(document)` object. For this reason, it can get hairy trying to chain methods to it. In fact, I'd argue the live method would make more sense as a global jQuery method in the form of `$.live('a',...)`.

### CSS selector only

And finally, the live method has a very large shortcoming, and that is that it can only operate on a direct CSS selector string. This makes it very inflexible.

**For more on the CSS selector shortcoming, see [Exploring jQuery .live() and .die()](https://www.alfajango.com/blog/exploring-jquery-live-and-die/).

**Update: Thanks to [pedalpete on Hacker News](http://news.ycombinator.com/item?id=2180896) and [Ellsass below in the comments](https://www.alfajango.com/blog/the-difference-between-jquerys-bind-live-and-delegate/comment-page-1/#comment-3637) for reminding me to add this next section.

## Why .live() or .delegate() instead of .bind()

After all, `bind` seems so much clearer and more direct, doesn't it? Well, there are 2 reasons we prefer `delegate` or `live` to `bind`:

- To attach handlers to DOM elements that may not yet exist in the DOM. Because `bind` directly binds handlers to the individual elements, it cannot bind them to elements that aren't on the page yet. If you were to run `$('a').bind(...)`, and then new links were added to the page via AJAX, your bind handler would not work for these. `live` and `delegate` on the other hand are bound to another ancestor node, so it will work for any element exists now or in the future within that ancestor element.
- Or to attach a handler to a single element or small group of elements, listening for events on descendent elements, instead of looping through and attaching the same function to 100 individual elements in the DOM. This would be the performance benefit of attaching a handler to one (or a small group of) ancestor element(s) instead of directly attaching handlers to all elements on the page.

## Stopping propagation

I'd like to mention one last note concerning event propagation. Typically, we can stop other handler functions from running by using event methods like:

```
$('a').bind('click', function(e) {
  e.preventDefault();
  // or
  e.stopPropagation();
});

```

However, when we use the live or delegate methods, the handler function won't actually run until the event bubbles to the element to which the handler is actually bound. By this time, our other handler functions from `.bind()` have already run.