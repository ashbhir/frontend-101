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

### [Asynchronous -> Generators, Promises, Coroutines](#Async Frontend)

### Multi-threading -> Web workers, Loopers

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

```
let promise = new Promise(function(resolve, reject) {
  // executor (the producing code, "singer")
});
```


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

 
