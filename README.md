# Frontend 101

### [Core Languages -> Javascript, Kotlin, HTML/ CSS/ DOM JS](#Core-Languages)

### Styling -> CSS, Flexbox, Android Layouts

### Compile -> Javascript runtime (V8), Web assembly, Hermes

### Frameworks -> React, Redux, Android, React Native & Bridging

### Bundling -> Webpack, Gradle

### Navigation -> React Router, React Navigation, App Navigation 

### Local Storage -> Room DB, Cookies vs Local Storage

### [Code Architecture -> MVC, MVVM, MVP, Redux Architecture](#Code-Architecture)

### [Design patterns -> Solid Principles](#Design-Patterns)

### [Website loading on browsers](#Website-loading-browsers)

### App Lifecycle

### Performance -> Pixel rendering on browsers/ apps, performance measuring

### Offline experience -> Service Workers/ PWA

### Security

### Networking internals

### [Asynchronous -> Generators, Promises, Coroutines, Multithreading, Event Loop](#Async Frontend)

### Infinite Lists -> Recycler View, FlatList

### Push Notifications

### Garbage Collection -> ARC, Mark & Sweep

### Image Optimisations

# Core Languages

## Javascript

- [33 JS Concepts](https://github.com/leonardomso/33-js-concepts)
- [Tricky JS Questions](https://github.com/lydiahallie/javascript-questions)
- [JS Fundamentals](https://javascript.info/)

# Code Architecture

## MVC vs MVP vs MVVM

### MVC

- Many to many relationship between View and Controllers
- Views not aware of the controller or the model (good thing).
- Input is passed on the controller which then executes the businness logic and modifies the view and the model.
- No clear distinction as to who will do the model formatting for presenting it to the view. Everything dumped to controllers make it bulky.
- Controllers are not reusable as they are coupled tightly with the View and the Model

### MVC in Android
![MVC Android](https://user-images.githubusercontent.com/12800313/113200098-8b482980-9285-11eb-88ec-53ccf8d51ea0.png)

### MVP

- One to one relationship between View and Presenter
- View holds reference of Presenter and notifies it on input change
- Presenter is fully aware of the View using IView interface and calls its methods based on different actions
- The interface/ contracts b/w View and Presenter becomes difficult to manage and is not reusable

### MVP in Android
![MVP Android](https://user-images.githubusercontent.com/12800313/113200111-913e0a80-9285-11eb-9914-cbd8103f68ed.png)

### MVVM

- Views can have many View Models
- View Models are not aware of the View
- Views handle their own updation by observing on the ViewModel data
- Little perfomance issues with so many data bindings

### MVVM in Android
![MVVM Android](https://user-images.githubusercontent.com/12800313/113200175-9f8c2680-9285-11eb-97fc-2737335b032a.png)

### Summary

![Comparison](https://user-images.githubusercontent.com/12800313/113200184-a31fad80-9285-11eb-89d3-0c72d46c534a.jpeg)

# Design-Patterns

### Solid in JS

- [Solid Principles in JS](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)

## Clean Code

- [Clean Code JS](https://github.com/ryanmcdermott/clean-code-javascript)

# Website loading browsers

- [How Browsers Works](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

# Async Frontend

## JS Promises 

```js
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```
The function passed to new Promise is called the executor. When new Promise is created, the executor runs automatically

When the executor obtains the result, be it soon or late, doesn’t matter, it should call one of these callbacks:

- resolve(value) — if the job is finished successfully, with result value.
- reject(error) — if an error has occurred, error is the error object.

### Consumers: then, catch, finally

```js
promise
.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error, same as using it in catch */ }
)
.catch()
.finally()
```

### Promise Chaining

```js
promise.then(f1).catch(f2);
Versus:
promise.then(f1, f2);
```
The difference b/w the two is that if an error happens in f1, then it is handled by .catch in first but not in second.
In other words, .then passes results/errors to the next .then/catch. So in the first example, there’s a catch below, and in the second one there isn’t, so the error is unhandled.

### Unhandled rejections

What happens when an error is not handled? For instance, we forgot to append .catch to the end of the chain, like here:

```js
new Promise(function() {
  noSuchFunction(); // Error here (no such function)
})
  .then(() => {
    // successful promise handlers, one or more
  }); // without .catch at the end!
```
The JavaScript engine tracks such rejections and generates a global error in that case.
In the browser we can catch such errors using the event `unhandledrejection`.

### Promise.allSettled

Promise.all rejects as a whole if any promise rejects. That’s good for “all or nothing” cases, when we need all results successful to proceed.

Promise.allSettled just waits for all promises to settle, regardless of the result. The resulting array has:

```
[
  {status:"fulfilled", value:result} for successful responses,
  {status:"rejected", reason:error} for errors.
```
Pollyfill:
```js
if (!Promise.allSettled) {
  const rejectHandler = reason => ({ status: 'rejected', reason });

  const resolveHandler = value => ({ status: 'fulfilled', value });

  Promise.allSettled = function (promises) {
    const convertedPromises = promises.map(p => Promise.resolve(p).then(resolveHandler, rejectHandler));
    return Promise.all(convertedPromises);
  };
}
```

### Promisification

Suppose we want to promisify a function which used to take callback:

```js
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}

// usage:
// loadScript('path/script.js', (err, script) => {...})
```

Here's how we can make a promisify any function which takes a callback argument:

```js
function promisify(f) {
  return function (...args) { // return a wrapper-function (*)
    return new Promise((resolve, reject) => {
      function callback(err, result) { // our custom callback for f (**)
        if (err) {
          reject(err);
        } else {
          resolve(result);
        }
      }

      args.push(callback); // append our custom callback to the end of f arguments

      f.call(this, ...args); // call the original function
    });
  };
}

// usage:
let loadScriptPromise = promisify(loadScript);
loadScriptPromise(...).then(...);
```

Promise handling is always asynchronous, as all promise actions pass through the internal “promise jobs” queue, also called “microtask queue” (V8 term).

## Event Loops 

When a promise is ready, its .then/catch/finally handlers are put into the microtask queue; they are not executed yet. When the JavaScript engine becomes free from the current code, it takes a task from the queue and executes it.

The event loop concept is very simple. There’s an endless loop, where the JavaScript engine waits for tasks, executes them and then sleeps, waiting for more tasks.

The general algorithm of the engine:

While there are tasks:
- execute them, starting with the oldest task.
- Sleep until a task appears, then go to 1.

Examples of tasks:

- When an external script \<script src="..."\> loads, the task is to execute it.
- When a user moves their mouse, the task is to dispatch mousemove event and execute handlers.
- When the time is due for a scheduled setTimeout, the task is to run its callback.

### Macrotasks and Microtasks

Microtasks come solely from our code. They are usually created by promises: an execution of .then/catch/finally handler becomes a microtask. Microtasks are used “under the cover” of await as well, as it’s another form of promise handling.

```
Immediately after every macrotask, the engine executes all tasks from microtask queue, prior to running any other macrotasks or rendering or anything else.
```

- Dequeue and run the oldest task from the macrotask queue (e.g. “script”).
- Execute all microtasks:
  - While the microtask queue is not empty:
  - Dequeue and run the oldest microtask.
- Render changes if any.
- If the macrotask queue is empty, wait till a macrotask appears.
- Go to step 1.

To schedule a new macrotask:

- Use zero delayed setTimeout(f).
- That may be used to split a big calculation-heavy task into pieces, for the browser to be able to react to user events and show progress between them.

- Also, used in event handlers to schedule an action after the event is fully handled (bubbling done).

- To schedule a new microtask
  - Use queueMicrotask(f).

- Also promise handlers go through the microtask queue.
- There’s no UI or network event handling between microtasks: they run immediately one after another.

- So one may want to queueMicrotask to execute a function asynchronously, but within the environment state.
## JS Promises vs Async Await

## Generators 

## Android Multi-threading

## Kotlin Coroutines

# Reference Links 

- [SessionStack How JS Works](https://blog.sessionstack.com/how-javascript-works/home)
- [Frontend Interview handbook](https://github.com/yangshun/front-end-interview-handbook)
- [ToDo MVC Examples](https://github.com/tastejs/todomvc/tree/master/examples)
- [Sanchit's Frontend notes](https://docs.google.com/document/d/1ikSTpvptGrS8GXQmVE-XLV4_WvCDcPGIoyySUicYv3I/edit)
- [React Interview Questions](https://github.com/sudheerj/reactjs-interview-questions)

 
