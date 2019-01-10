

# Javascript Fundamentals

https://channel9.msdn.com/Series/Javascript-Fundamentals-Development-for-Absolute-Beginners



https://medium.freecodecamp.org/a-complete-beginners-guide-to-react-4d490abc349c

### Executing Javascript code as Worker

```js
var fs = (function () { 
	/* code for the worker */ 
}).toString(); 
var blob = new Blob(
   [fs.substr(13, fs.length - 14)],
   { type: 'text/javascript' }
);
var url = window.URL.createObjectURL(blob);
var worker = new Worker(url);
worker.onmessage = function(event) {
		console.log(event);
	};
var args = { start : 100, end : 10000 };
worker.postMessage(JSON.stringify(args));
```



# ES 6

https://hacks.mozilla.org/category/es6-in-depth/

http://jamesknelson.com/es6-the-bits-youll-actually-use/

# React

https://hackernoon.com/you-are-not-a-react-native-noob-c4309ceccd91

1. [A Brief Introduction to JSX](https://alligator.io/react/jsx-introduction/)
2. [Introduction to The Beginner's Guide to ReactJS](https://egghead.io/lessons/react-introduction-to-the-beginner-s-guide-to-reactjs)
3. [A Roadmap to Becoming a Better React Native Developer in 2018 & Beyond](https://alligator.io/react/roadmap-react-native-developer/)
4. https://reactarmory.com/guides/learn-react-by-itself/react-basics
   1. Events in element https://reactjs.org/docs/events.html#clipboard-events
   2. [React Event Cheatsheets](https://reactarmory.com/guides/react-events-cheatsheet)
   3. https://reactarmory.com/guides/learn-react-by-itself/brief-introduction-to-jsx
5. [Routing](https://reactarmory.com/guides/context-free-react-router)
   1. https://github.com/ReactTraining/react-router
6. [DEV Tool](https://www.npmjs.com/package/react-devtools)
7. React Modular Apps
   1. https://medium.com/@alexmngn/how-to-better-organize-your-react-applications-2fd3ea1920f1
   2. https://medium.com/@alexmngn/why-react-developers-should-modularize-their-applications-d26d381854c1
8. https://medium.com/@alexmngn/from-reactjs-to-react-native-what-are-the-main-differences-between-both-d6e8e88ebf24
9. https://dzone.com/articles/what-is-better-for-web-development-react-or-angula
10. https://medium.com/@hamzamahmood/advantages-of-developing-modern-web-apps-with-react-js-8504c571db71
11. https://formidable.com/blog/2015/12/04/using-react-is-a-business-decision-not-a-technology-choice/
12. https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176



# React Native Web

https://github.com/necolas/react-native-web

https://medium.com/@ron.arts/web-support-for-create-react-native-app-80b16f930326

# Redux

- [Getting Started with Redux](https://egghead.io/courses/getting-started-with-redux)
- [Building Applications with Idiomatic Redux](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)
- https://medium.com/javascript-scene/10-tips-for-better-redux-architecture-69250425af44

# companies-who-trust-node-js

https://medium.com/@_ericelliott/companies-who-trust-node-js-4bfb8385ab84

https://medium.com/@danwang74/the-economics-between-testing-and-types-4a3f8c8a86eb

http://events.inf.ed.ac.uk/Milner2012/J_Harrison-html5-mp4.html

-------------

# Market Share

https://trends.builtwith.com/javascript/React/Market-Share

-------------------







**The First Rule Of React: If it is practical to pass the data you need through props, it must be passed through props**

**A component can set its child element’s props. Child elements cannot directly change the values of their props.**

**your component only re-renders when this.props or this.state change.**



### So how do I define class components?

You just wrap a function component in a class! There are only three rules you’ll need to follow:

- The class must extend from `React.Component`
- Your method that returns a React Element must be called `render()`
- Props are accessed through `this.props` instead of through function arguments



When you call `this.setState(state)` from within your class, the following will happen:

1. The properties of the passed in `state` object will be copied onto `this.state` (but `this.state` won’t change immediately – more on this later)
2. React will then re-render your component and its nested components

Of course, you’re probably more interested in what you *can*accomplish with `setState`.

### `setState` makes self-contained components possible