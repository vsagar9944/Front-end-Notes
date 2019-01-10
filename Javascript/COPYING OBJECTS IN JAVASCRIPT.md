### [COPYING OBJECTS IN JAVASCRIPT](https://smalldata.tech/blog/2018/11/01/copying-objects-in-javascript)

Crafted on 1 November, 2018 | By Victor Parmar
Updated on 13 November, 2018 and 14 November, 2018

In this article we will look at the various ways an object can be copied in Javascript. We will take a look at both shallow and deep copying.

Before we begin, it is worth mentioning a few basics: objects in Javascript are simply references to a location in memory. These references are mutable, i.e. they can be reassigned. Thus, simply making a copy of a reference only results in 2 references pointing to the same location in memory:

```
var foo = {
    a : "abc"
}
console.log(foo.a); // abc

var bar = foo;
console.log(bar.a); // abc

foo.a = "yo foo";
console.log(foo.a); // yo foo
console.log(bar.a); // yo foo

bar.a = "whatup bar?";
console.log(foo.a); // whatup bar?
console.log(bar.a); // whatup bar?    
```

As you see in the above example, both foo and bar are reflecting the change done in either object. Thus, making a copy of an object in Javascript requires some care depending upon your use case.

##### SHALLOW COPY

If your object only has properties which are value types, you can use the spread syntax or `Object.assign(...)`

```
var obj = { foo: "foo", bar: "bar" };

var copy = { ...obj }; // Object { foo: "foo", bar: "bar" }
var obj = { foo: "foo", bar: "bar" };

var copy = Object.assign({}, obj); // Object { foo: "foo", bar: "bar" }
```

Note that both of the above methods can be used to copy property values from *multiple source objects* to a target object:

```
var obj1 = { foo: "foo" };
var obj2 = { bar: "bar" };

var copySpread = { ...obj1, ...obj2 }; // Object { foo: "foo", bar: "bar" }
var copyAssign = Object.assign({}, obj1, obj2); // Object { foo: "foo", bar: "bar" }
```

The problem with the above methods lies in the fact that for objects with properties which are themselves objects, only the references are copied over, i.e. it is the equivalent of doing `var bar = foo;` as in the first code example:

```
var foo = { a: 0 , b: { c: 0 } };
var copy = { ...foo };

copy.a = 1;
copy.b.c = 2;

console.dir(foo); // { a: 0, b: { c: 2 } }
console.dir(copy); // { a: 1, b: { c: 2 } }
```

##### DEEP COPY (WITH CAVEATS)

In order to deep copy objects, a potential solution can be to serialize the object to a string and then deserialize it back:

```
var obj = { a: 0, b: { c: 0 } };
var copy = JSON.parse(JSON.stringify(obj));
```

Unfortunately, this method only works when the source object contains serializable value types and does not have any circular references. An example of a non-serializable value type is the `Date` object - even though it is printed in ISO format on stringification, `JSON.parse` only interprets it as a string and not as a`Date` object.

##### DEEP COPY (WITH FEWER CAVEATS)

For more complex cases, one could make use of a newer HTML5 cloning algorithm called ["structured clone"](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm). Unfortunately, at the time of writing it is still limited to certain built-in types but it supports many more types than what `JSON.parse`does: Date, RegExp, Map, Set, Blob, FileList, ImageData, sparse and typed Array. It also preserves references within the cloned data, allowing it to support cyclical and recursive structures that don't work with the above mentioned serialization method.

Currently, there is no direct way of calling the structured clone algorithm but there are some newer browser features that use this algorithm under the hood. Thus, there are a couple of workarounds that could potentially be used to deep copy objects.

Via MessageChannels: the idea behind this is to leverage the serialization algorithm used by a communication feature. Since this feature is event based, the resultant clone is also an asynchronous operation.

```
class StructuredCloner {
  constructor() {
    this.pendingClones_ = new Map();
    this.nextKey_ = 0;

    const channel = new MessageChannel();
    this.inPort_ = channel.port1;
    this.outPort_ = channel.port2;

    this.outPort_.onmessage = ({data: {key, value}}) => {
      const resolve = this.pendingClones_.get(key);
      resolve(value);
      this.pendingClones_.delete(key);
    };
    this.outPort_.start();
  }

  cloneAsync(value) {
    return new Promise(resolve => {
      const key = this.nextKey_++;
      this.pendingClones_.set(key, resolve);
      this.inPort_.postMessage({key, value});
    });
  }
}

const structuredCloneAsync = window.structuredCloneAsync =
    StructuredCloner.prototype.cloneAsync.bind(new StructuredCloner);


const main = async () => {
  const original = { date: new Date(), number: Math.random() };
  original.self = original;

  const clone = await structuredCloneAsync(original);

  // different objects:
  console.assert(original !== clone);
  console.assert(original.date !== clone.date);

  // cyclical:
  console.assert(original.self === original);
  console.assert(clone.self === clone);

  // equivalent values:
  console.assert(original.number === clone.number);
  console.assert(Number(original.date) === Number(clone.date));

  console.log("Assertions complete.");
};

main();
```

Via the history API: both `history.pushState()` and `history.replaceState()`create a structured clone of their first argument! Note that while this method is synchronous, manipulating browser history is not a fast operation and calling this method repeatedly can lead to browser unresponsiveness.

```
const structuredClone = obj => {
  const oldState = history.state;
  history.replaceState(obj, null);
  const clonedObj = history.state;
  history.replaceState(oldState, null);
  return clonedObj;
};
```

Via the [notification API](https://developer.mozilla.org/en-US/docs/Web/API/Notification/Notification): when creating a new notification, the constructor creates a structured clone of its associated data. Note that it also attempts to display a browser notification to the user, but this will silently fail unless the application has requested permissions to display notifications. In the case that permission was granted, the notification is immediately closed.

```
const structuredClone = obj => {
  const n = new Notification("", {data: obj, silent: true});
  n.onshow = n.close.bind(n);
  return n.data;
};
```

##### DEEP COPY IN NODE.JS

As of version 8.0.0, Node.js provides a [serialization api](https://nodejs.org/api/v8.html#v8_serialization_api) which is compatible with structured clone. Note that this API is marked as experimental at the time of writing:

```
const v8 = require('v8');
const buf = v8.serialize({a: 'foo', b: new Date()});
const cloned = v8.deserialize(buf);
cloned.b.getMonth();
```

For versions below 8.0.0 or for a more stable implementation, one can use lodash's`cloneDeep` method, which is also loosely based on the structured clone algorithm.

##### CONCLUSION

To sum up, the best algorithm for copying objects in Javascript is heavily dependent on the context and type of objects that you are looking to copy. While lodash is the safest bet for a generic deep copy function, you might get a more efficient implementation if you roll your own, the following is an example of a simple deep clone that works for dates as well:

```
function deepClone(obj) {
  var copy;

  // Handle the 3 simple types, and null or undefined
  if (null == obj || "object" != typeof obj) return obj;

  // Handle Date
  if (obj instanceof Date) {
    copy = new Date();
    copy.setTime(obj.getTime());
    return copy;
  }

  // Handle Array
  if (obj instanceof Array) {
    copy = [];
    for (var i = 0, len = obj.length; i < len; i++) {
        copy[i] = deepClone(obj[i]);
    }
    return copy;
  }

  // Handle Function
  if (obj instanceof Function) {
    copy = function() {
      return obj.apply(this, arguments);
    }
    return copy;
  }

  // Handle Object
  if (obj instanceof Object) {
      copy = {};
      for (var attr in obj) {
          if (obj.hasOwnProperty(attr)) copy[attr] = deepClone(obj[attr]);
      }
      return copy;
  }

  throw new Error("Unable to copy obj as type isn't supported " + obj.constructor.name);
}
```

Personally, I'm looking forward to be able to use structured clone everywhere and finally put this issue to rest, happy cloning :)