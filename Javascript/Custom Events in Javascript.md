# Custom Events in Javascript

Events can be created with the [`Event`](https://developer.mozilla.org/en-US/docs/Web/API/Event) constructor as follows:

```
var event = new Event('build');

// Listen for the event.
elem.addEventListener('build', function (e) { //... }, false);

// Dispatch the event.
elem.dispatchEvent(event);
```

To add more data to the event object, the [CustomEvent](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent) interface exists and the **detail** property can be used to pass custom data.

```
var event = new CustomEvent('build', { detail: elem.dataset.time });
function eventHandler(e) {
  console.log('The time is: ' + e.detail);
}
```

Old Fashined way

```
// Create the event.
var event = document.createEvent('Event');

// Define that the event name is 'build'.
event.initEvent('build', true, true);

// Listen for the event.
elem.addEventListener('build', function (e) {
  // e.target matches elem
}, false);

// target can be any Element or other EventTarget.
elem.dispatchEvent(event);
```



## Triggering built-in events

```
function simulateClick() {
  var event = new MouseEvent('click', {
    view: window,
    bubbles: true,
    cancelable: true
  });
  var cb = document.getElementById('checkbox'); 
  var cancelled = !cb.dispatchEvent(event);
  if (cancelled) {
    // A handler called preventDefault.
    alert("cancelled");
  } else {
    // None of the handlers called preventDefault.
    alert("not cancelled");
  }
}
```







