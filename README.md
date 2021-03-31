# Frontend 101

### [Core Languages -> Javascript, Kotlin, HTML/ CSS/ DOM JS](#Core-Languages)

### Styling -> CSS, Flexbox, Android Layouts

### Compile -> Javascript runtime (V8), Web assembly, Hermes

### Frameworks -> React, Redux, Android, React Native & Bridging

### Bundling -> Webpack, Gradle

### Navigation -> React Router, React Navigation, App Navigation 

### Local Storage -> Room DB, Cookies vs Local Storage

### Code Architecture -> MVC, MVVM, MVP, Redux Architecture

### [Design patterns -> Solid Principles](#Design-Patterns)

### Website page load

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

### Clean Code

- [Clean Code JS](https://github.com/ryanmcdermott/clean-code-javascript)

# Reference Links 

- [How Browsers Works](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)
- [How JS Works](https://blog.sessionstack.com/how-javascript-works/home)
- [Frontend Interview handbook](https://github.com/yangshun/front-end-interview-handbook)
- [ToDo MVC Examples](https://github.com/tastejs/todomvc/tree/master/examples)
- [Sanchit's Frontend notes](https://docs.google.com/document/d/1ikSTpvptGrS8GXQmVE-XLV4_WvCDcPGIoyySUicYv3I/edit)
- [Solid Principles in JS Summary]()
- [JS Interview Questions](https://www.toptal.com/javascript/interview-questions)
- [React Interview Questions](https://github.com/sudheerj/reactjs-interview-questions)
- [Web Fundamentals](https://javascript.info/)

 
