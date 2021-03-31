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

### Asynchronous -> Generators, Promises, Coroutines 

### Multi-threading -> Web workers, Loopers

### Infinite Lists -> Recycler View, FlatList

### Push Notifications

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

# Design Patterns

## Solid Principles

### Single Responsibility Principle
```
A class should have one and only one reason to change, meaning that a class should have only one job.
```

```
class Employee {
  fun getTax() {
    val tax = this.salary * 10/100;
    FileReader f = new FileReader();
    f.out(tax)
    print(tax)
  }
}
```
Employee class is handling responsibilities of calculating tax, outputing to file and printing to console.

We should rather make a separate class as `TaxCalculator`, `Logger`, `FileOutputter` etc.

### Open-Closed Principle
```
Objects or entities should be open for extension but closed for modification.
```
```
class Bird {
  fun fly() {
    if (this.type == 'flamingo') {
      // do something
    } else if (this.type == 'egret') {
      // do something else
    }
  }
}
```
Here, function fly and in effect class Bird will be modified on every addition of a type of bird. We should rather have separate bird classed inheriting Bird class.

```
class Flamingo extends Bird {
  fun fly() {}
}

class Egret extends Bird {
  fun fly() {}
}
```

Also, for class `TaxCalculator` we can different types of Employees implement different TaxCalculator like `TaxCalculatorIntern`, `TaxCalculatorFullTime` or `TaxClaculatorCEO`.
**Ques**: But how do we instantiate different TaxCalculator classes for different Employess ?
**Ans**: We use Factory Pattern. 

```
class Employee {
  fun calculateTax() {
    val taxCalculator = TaxCalculator.create(this.type)
    return taxCalculator.calculate()
  }
}
```

### Liskov Substitution Principle
```
Every subclass or derived class should be substitutable for their base or parent class.
```
```
class Penguin extends Bird {
  fun fly() {
    // what to do ??
  }
}
```
Not all birds can fly. It doesn't make sense for Bird class to contain fly method if not all birds are flyable. Perhaps make an interface `Flyable` and we can only call bird.fly() on bird instances that implement `Flyable` interface.

```
class Stack extends ArrayList<Integer> {
  fun push() {}
  fun pop() {}
}
```
Doesn't make sense Stack class to extend ArrayList as it will have to implement all methods of ArrayList and throw error. One can access any element of the stack which is not the desired behavior. We should favor composition here.

### Interface Seggregation Principle
```
A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.
```
This simply means Single Responsible Principle for interfaces. `Flayable` interface shouldn't have a `flapWings()` method but rather make a new interface `Flappable`.

### Dependency Inversion Principle
```
Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.
```
```
class ProductSQLRepo {
  fun save() {}
  fun get() {}
}

class Payment {
  fun pay(productId: Int) {
    val sqlRepo = new ProductSQLRepo(productId)
    val product = sqlRepo.get()
    payAmount(product.price)
  }
}
```
Here high-level class `Payment` is dependent on a low-level module `ProductSQLRepo` which is not correct for two reasons- If later, product fetching is changed to ProductMongoRepo, a lot of changes will be required + Payment should not be concerned with initialisation of the repo. 

It is solved by following dependency injection and leaving reposibility of instantiating of repo on the caller.

```
abstract class ProductRepo {
  fun save() {}
  fun get() {}
}

class Payment {
  fun pay(productId: Int, productRepo: ProductRepo) {
    val product = productRepo.get()
    payAmount(product.price)
  }
}
```


IOC (Inversion of Control) frameworks like springboot help with managing dependencies.

### Compostion and Inheritance

Both are generalisations that have a `has a` relation rather than `is a` relation in class inheritance. 

Compostions are a `belong to` type of relationship. Class `Car` is composed of class `Engine`.
Aggregations are a `has a` type of relationship. Class `Doctor` may or may not contain class `Patient`.

### Solid in JS

- [Solid Principles in JS](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)

## Clean Code

- [Clean Code JS](https://github.com/ryanmcdermott/clean-code-javascript)

# Website loading browsers

- [How Browsers Works](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)

# Reference Links 

- [SessionStack How JS Works](https://blog.sessionstack.com/how-javascript-works/home)
- [Frontend Interview handbook](https://github.com/yangshun/front-end-interview-handbook)
- [ToDo MVC Examples](https://github.com/tastejs/todomvc/tree/master/examples)
- [Sanchit's Frontend notes](https://docs.google.com/document/d/1ikSTpvptGrS8GXQmVE-XLV4_WvCDcPGIoyySUicYv3I/edit)
- [React Interview Questions](https://github.com/sudheerj/reactjs-interview-questions)

 
