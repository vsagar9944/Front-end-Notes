# [Promise in Javascript](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)

### What is a Promise?

A promise is an object that may produce a single value some time in the future: either a resolved value, or a reason that it’s not resolved.A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

### How Promises Work

A promise is an object which can be returned synchronously from an asynchronous function. It will be in one of 3 possible states:

- **Fulfilled:** `onFulfilled()` will be called (e.g., `resolve()` was called)
- **Rejected:** `onRejected()` will be called (e.g., `reject()` was called)
- **Pending:** not yet fulfilled or rejected

Once settled, a promise can not be resettled. Calling `resolve()` or `reject()`again will have no effect. The immutability of a settled promise is an important feature.



The ES6 promise constructor takes a function. That function takes two parameters, `resolve()`, and `reject()`. In the example above, we’re only using `resolve()`, so I left `reject()` off the parameter list. Then we call `setTimeout()` to create the delay, and call `resolve()` when it’s finished.

You can optionally `resolve()` or `reject()` with values, which will be passed to the callback functions attached with `.then()`

When I `reject()` with a value, I always pass an `Error` object. Generally I want two possible resolution states: the normal happy path, or an exception — anything that stops the normal happy path from happening. Passing an `Error` object makes that explicit.

### Important Promise Rules

Promises following the spec must follow a specific set of rules:

- A promise or “thenable” is an object that supplies a standard-compliant `.then()` method.
- A pending promise may transition into a fulfilled or rejected state.
- A fulfilled or rejected promise is settled, and must not transition into any other state.
- Once a promise is settled, it must have a value (which may be `undefined`). That value must not change.

The `.then()` method must comply with these rules:

- Both `onFulfilled()` and `onRejected()` are optional.
- If the arguments supplied are not functions, they must be ignored.
- `onFulfilled()` will be called after the promise is fulfilled, with the promise’s value as the first argument.
- `onRejected()` will be called after the promise is rejected, with the reason for rejection as the first argument. The reason may be any valid JavaScript value, but because rejections are essentially synonymous with exceptions, I recommend using Error objects.
- Neither `onFulfilled()` nor `onRejected()` may be called more than once.
- `.then()` may be called many times on the same promise. In other words, a promise can be used to aggregate callbacks.
- `.then()` must return a new promise, `promise2`.
- If `onFulfilled()` or `onRejected()` return a value `x`, and `x` is a promise, `promise2` will lock in with (assume the same state and value as) `x`. Otherwise, `promise2` will be fulfilled with the value of `x`.
- If either `onFulfilled` or `onRejected` throws an exception `e`, `promise2`must be rejected with `e` as the reason.
- If `onFulfilled` is not a function and `promise1` is fulfilled, `promise2` must be fulfilled with the same value as `promise1`.
- If `onRejected` is not a function and `promise1` is rejected, `promise2` must be rejected with the same reason as `promise1`.



### Promise Chaining

Because `.then()` always returns a new promise, it’s possible to chain promises with precise control over how and where errors are handled. Promises allow you to mimic normal synchronous code’s `try`/`catch`behavior.

Like synchronous code, chaining will result in a sequence that runs in serial. In other words, you can do:

```
fetch(url)
  .then(process)
  .then(save)
  .catch(handleErrors)
;
```

Assuming each of the functions, `fetch()`, `process()`, and `save()` return promises, `process()` will wait for `fetch()` to complete before starting, and `save()` will wait for `process()` to complete before starting. `handleErrors()`will only run if any of the previous promises reject.



### Error Handling

Note that promises have both a success and an error handler, and it’s very common to see code that does this:

```
save().then(
  handleSuccess,
  handleError
);
```



### How Do I Cancel a Promise?

One of the first things new promise users often wonder about is how to cancel a promise. Here’s an idea: Just reject the promise with “Cancelled” as the reason. If you need to deal with it differently than a “normal” error, do your branching in your error handler.

Here are some common mistakes people make when they roll their own promise cancellation:

#### Adding .cancel() to the promise

Adding `.cancel()` makes the promise non-standard, but it also violates another rule of promises: Only the function that creates the promise should be able to resolve, reject, or cancel the promise. Exposing it breaks that encapsulation, and encourages people to write code that manipulates the promise in places that shouldn't know about it. Avoid spaghetti and broken promises.

#### Forgetting to clean up

Some clever people have figured out that there’s a way to use `Promise.race()`as a cancellation mechanism. The problem with that is that cancellation control is taken from the function that creates the promise, which is the only place that you can conduct proper cleanup activities, such as clearing timeouts or freeing up memory by clearing references to data, etc...

#### Forgetting to handle a rejected cancel promise

Did you know that Chrome throws warning messages all over the console when you forget to handle a promise rejection? Oops!