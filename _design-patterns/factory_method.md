---
title: Design Pattern
layout: post
date: 2020-06-07 15:12:15
categories: DesignPattern
---

## Factory Method Design Pattern

This patterns is one of the _Creational Pattern_

### Intent

Define an interface for creating a standard for an object. However, let the subclass of that of object be able to define and instantiate the _new_ object.

Even though the creation of the object is defined in the superclass, the subclass can change the returned object that will be created.

### Problem

The need of standard return models for one application.
For instance:

- Depending on the goal of the this class. The way how the objects are instantiated can cause or not some trouble. So, I just need the objects, but the implementation of the objects initialisation can prevent or cause problems.

```swift
class CoreDataStore {

    static var persistentStoreCoordinator: NSPersistentStoreCoordinator? {
        ...
    }

    static var managedObjectModel: NSManagedObjectModel? {
       ...
    }

    static var managedObjectContext: NSManagedObjectContext? {
        ...
    }

}
```

### Struct

1. Product declares the single interface for all objects that can be produced by the creator and its subclasses.

2. _Concrete Products_ are the different implementations of the Product interface.
   _Concrete Creators_ will create and return instances of these classes.

3. _Creator_ declares a factory method that returns the Product type. This method can either be abstract or have some default implementation. In the first case, all Concrete Creators **must** implement their factory methods.
   Despite the name, in the real world, the product creation is not the main responsibility of a Creator class. Usually, it has some core business logic that works with Products.
   Here is the analogy: a large software development company can have a training department for programmers. But the primary function of the company is still writing code.

4. _Concrete Creators_ implement or override the base factory method, by creating and returning one of the Concrete Products.
   Note that a factory method does not have to **create** new instances all the time. It can also return existing objects from some cache, etc.

### Discussion

Factory Method makes subclasses responsible for choosing concrete classes of products that will be created.

In the above code:

```swift
class CoreDataStore {

    static var persistentStoreCoordinator: NSPersistentStoreCoordinator? {
        ...
    }

    static var managedObjectModel: NSManagedObjectModel? {
       ...
    }

    static var managedObjectContext: NSManagedObjectContext? {
        ...
    }

}
```

We could use a `Protocol` to make sure we will get some stuff from `CoreDataStore`

```swift
class CoreDataStoreProtocol {

    var persistentStoreCoordinator: NSPersistentStoreCoordinator? { get }

    var managedObjectModel: NSManagedObjectModel? { get }

    var managedObjectContext: NSManagedObjectContext? { get }

}
```

Then on test I could do something like:

```swift
extension NSPersistentStoreCoordinator {
    // Do stuff for new Product instantiation for test
    init(_ customInit: SomeParameter) { ... }
}
```

And my test creator:

```swift
class CoreDataStoreTest: CoreDataStoreProtocol {

    static var persistentStoreCoordinator: NSPersistentStoreCoordinator? {
        return NSPersistentStoreCoordinator(SomeParameter)
    }

    static var managedObjectModel: NSManagedObjectModel? {
       ...
    }

    static var managedObjectContext: NSManagedObjectContext? {
        ...
    }

}
```

### Applicability

- When you do not know what type of particularities your initialisation code will need. For instance if you need some default SDK context for initialise your object on one kind of environment, that might broke your code on other environment.
- When you want to provide flexibility, such as when you, at the same, want to provide a standard but that can be altered/transformed by its subclasses. Such as bottoms, they might have default methods and ways to behaviour, but they can have tiny different characteristics such as color or size.

### How to implement

### Pros and Cons

- Pros

  - Follows the Open/Closed Principle.
    - Avoids tight coupling between concrete products and code that uses them.
    - Simplifies code due to moving all creational code to one place.
    - Simplifies adding new products to the program.

- Cons
  - Requires extra subclasses.

## Source

- [Factory Method Design Pattern](https://sourcemaking.com/design_patterns/factory_method)
- [Factory Method](https://refactoring.guru/design-patterns/factory-method)
