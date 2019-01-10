# DOM and BOM

## DOM

The **Document Object Model** (**DOM**) is an application programming interface ([API](https://www.computerhope.com/jargon/a/api.htm)) that gives programmers and developers the ability to create and modify [HTML](https://www.computerhope.com/jargon/h/html.htm) and [XML](https://www.computerhope.com/jargon/x/xml.htm) documents as programming [objects](https://www.computerhope.com/jargon/o/object.htm). The structure of the DOM for any document will resemble the actual structure of the markup of the document.

The DOM for this document includes all of the elements and any text [nodes](https://www.computerhope.com/jargon/n/node.htm) within those elements. 



## BOM

he Browser Object Model (BOM) is really the core of using JavaScript on the Web. The BOM provides objects that expose browser functionality independent of any web page content.

- THE WINDOW OBJECT At the core of the BOM is the window object, which represents an instance of the browser. The window object serves a dual purpose in browsers, acting as the JavaScript interface to the browser window and the ECMAScript Global object. This means that every object, variable, and function defi ned in a web page uses window as its Global object and has access to  methods like parseInt()

- Since the window object doubles as the ECMAScript Global object, all variables and functions declared globally become properties and methods of the window object.

- Despite global variables becoming properties of the window object, there is a slight difference between defi ning a global variable and defi ning a property directly on window: global variables cannot be removed using the delete operator, while properties defi ned directly on window can

- Properties of window that were added via var statements have their [[Configurable]] attribute set to false and so may not be removed via the delete operator. Internet Explorer 8 and earlier enforced this by throwing an error when the delete operator is used on window properties regardless of how they were originally created.

- Another thing to keep in mind: attempting to access an undeclared variable throws an error, but it is possible to check for the existence of a potentially undeclared variable by looking on the windowobject

  ​

### Window Relationships and Frames

If a page contains frames, each frame has its own window object and is stored in the framescollection. Within the frames collection, the window objects are indexed both by number (starting at 0, going from left to right, and then row by row) and by the name of the frame. Each windowobject has a name property containing the name of the frame.

This code creates a frameset with one frame across the top and two frames underneath. Here, the top frame can be referenced by window.frames[0] or window.frames[“topFrame”]; however, you would probably use the top object instead of window to refer to these frames (making it top.frames[0], for instance).The top object always points to the very top (outermost) frame, which is the browser window itself. This ensures that you are pointing to the correct frame from which to access the others. Any code written within a frame that references the window object is pointing to that frame’s unique instance rather than the topmost one. Figure 8-1 indicates the various ways that the frames in the previous example may be accessed from code that exists in the topmost window

Whenever frames are used, multiple Global objects exist in the browser. Global variables defi ned in each frame are defi ned to be properties of that frame’s window object. Since each window object contains the native type constructors, each frame has its own version of the constructors, which are not equal. For example, top.Object is not equal to top.frames[0].Object, which affects the use of instanceof when objects are passed across frames

### Window object

1. **top**
2. **parent** :- Another window object is called parent. The parent object always points to the current frame’s immediate parent frame.
3. **self** :- There is one final window object, called self, which always points to window.



### Window Position

All browsers provide **screenLeft**  and  **screenTop** properties that indicate the window’s location in relation to the left and top of the screen.

 Firefox provides this functionality through the screenX and screenY properties, which are also supported in Safari and Chrome

The end result is that you cannot accurately determine the left and top coordinates of a browser window across all browsers. It is possible, however, to accurately move the window to a new position using the moveTo() and moveBy() methods. Each method accepts two arguments. **moveTo()** expects the x and y coordinates to move to, and **moveBy()** expects the number of pixels to move in each direction.

### Window Size

Internet Explorer 9+, Firefox, Safari, Opera, and Chrome all provide four properties: innerWidth, innerHeight, outerWidth, and outerHeight

The document.documentElement.clientWidth and document.documentElement.clientHeightproperties provide the width and height of the page viewport in Internet Explorer, Firefox, Safari, Opera, and Chrome. 

### Navigating and Opening Windows

The window.open() method can be used both to navigate to a particular URL and to open a new browser window. This method accepts four arguments: the URL to load, the window target, a string of features, and a Boolean value indicating that the new page should take the place of the currently loaded page in the browser history. Typically only the fi rst three arguments are used; the last argument applies only when not opening a new window.

### Popping Up Windows

When the second argument doesn’t identify an existing window or frame, a new window or tab is created based on a string passed in as the third argument. If the third argument is missing, a new browser window (or tab, based on browser settings) is opened with all of the default browser window settings. (Toolbars, the location bar, and the status bar are all set based on the browser’s default settings.) The third argument is ignored when not opening a new window.

```
window.open("http://www.wrox.com/","wroxWindow","height=400,width=400,top=10,left=10,resizable=yes");
```

It’s possible to close the newly opened window by calling the close() method as follows.

When one tab opens another, the window objects need to be able to communicate with one another, so the tabs cannot run in separate processes. Chrome allows you to indicate that the newly created tab should be run in a separate process by setting the openerproperty to null, as in the following example:var wroxWin =window.open(“http://www.wrox.com/”,”wroxWindow”,                         “height=400,width=400,top=10,left=10,resizable=yes”);wroxWin.opener = null;Setting opener to null indicates to the browser that the newly created tab doesn’t need to communicate with the tab that opened it, so it may be run in a separate process. Once this connection has been severed, there is no way to recover it



### Pop-up Blockers

Most browsers have pop-up–blocking software built in, and for those that don’t, utilities such as the Yahoo! Toolbar have built-in pop-up blockers. The result is that most unexpected pop-ups are blocked. When a pop-up is blocked, one of two things happens. If the browser’s built-in pop-up blocker stopped the pop-up, then window.open() will most likely return null. In that case, you can tell if a pop-up was blocked by checking the return value, as shown in the following example:

```
var wroxWin = window.open(“http://www.wrox.com”, “_blank”);
if (wroxWin == null){    
	alert(”The popup was blocked!”);
}
```



## Intervals and Timeouts

JavaScript execution in a browser is single-threaded, but does allow for the scheduling of code to run at specifi c points in time through the use of timeouts and intervals. Timeouts execute some code after a specifi ed amount of time, whereas intervals execute code repeatedly, waiting a specifi c amount of time in between each execution.

JavaScript is single-threaded and, as such, can execute only one piece of code at a time. To manage execution, there is a queue of JavaScript tasks to execute. The tasks are executed in the order in which they were added to the queue. The second argument of setTimeout() tells the JavaScript engine to add this task onto the queue after a set number of milliseconds. If the queue is empty, then that code is executed immediately; if the queue is not empty, the code must wait its turn. 

When setTimeout() is called, it returns a numeric ID for the timeout. The timeout ID is a unique identifi er for the scheduled code that can be used to cancel the timeout. To cancel a pending timeout, use the clearTimeout() method and pass in the timeout ID, as in the following example:

```
//set the timeout
var timeoutId = setTimeout(function() {   
    alert(“Hello world!”);
}, 1000);
//nevermind - cancel itclearTimeout(timeoutId);
```

NOTE:- All code executed by a timeout runs in the global scope, so the value of thisinside the function will always point to window when running in nonstrict mode and undefined when running in strict mode.

**Intervals**

Intervals work in the same way as timeouts except that they execute the code repeatedly at specifi c time intervals until the interval is canceled or the page is unloaded. The setInterval() method lets you set up intervals, and it accepts the same arguments as setTimeout(): the code to execute (string or function) and the milliseconds to wait between executions. 

The setInterval() method also returns an interval ID that can be used to cancel the interval at some point in the future. The clearInterval() method can be used with this ID to cancel all pending intervals.

## THE LOCATION OBJECT

One of the most useful BOM objects is location, which provides information about the document that is currently loaded in the window, as well as general navigation functionality. The locationobject is unique in that it is a property of both window and document; both window.location and document.location point to the same object. Not only does location know about the currently loaded document, but it also parses the URL into discrete segments that can be accessed via a series of properties

Query String Arguments

Most of the information in location is easily accessible from these properties. The one part of the URL that isn’t provided is an easy-to-use query string. Though location.search returns everything from the question mark until the end of the URL, there is no immediate access to query-string arguments on a one-by-one basis.



THE NAVIGATOR OBJECT