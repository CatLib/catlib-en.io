---
title: Facade
---

# Facade

The facade mode, also known as the skin mode, is used to provide a consistent interface to a set of interfaces in a subsystem.

The facade mode defines a high-level interface that makes the subsystem easier to use: The user only needs to interact directly with the facade, and the complex relationship between the user and the subsystem is implemented by the facade, thereby reducing the coupling degree of the system.

## Implemention facade

You only need to inherit from `CatLib.Facade` to implement the facade.

```csharp
namespace Facades
{
    public class Foo : Facade<IFoo>
    {
    }
}
```

## Use facade

Using the facade is very simple, you only need to access the `That` under the facade to access the corresponding implementation.

```csharp
using Facades;

Foo.That.HelloWorld();
```

> The code generation technique can be used to simplify the facade call to `Foo.HelloWorld()`

## How the facade works

A facade is a class that provides access to objects in a container. This is implemented by the `CatLib.Facade` class.

The facade class only needs to provide the interface type of the service. The underlying Facade will request the implementation of the service from the container through this service interface and cache it to achieve performance consistent with the source code.

> When the service implementation changes, the facade automatically updates the service implementation.

## Reasonable use of the facade

The facade has many advantages, it provides a simple, easy to remember grammar, so that we can use the features provided by the service without having to remember the long class name.

However, there are also places to be aware of when using the facade. One of the most important dangers is class-wide creep. Since the facade is so easy to use and does not require injection, using too many facades in a single class can make the class easy to get bigger and bigger.

Using dependency injection will alleviate this type of problem, because a huge constructor makes it easy to tell if the class is getting bigger. Therefore, when using the facade, pay special attention to the size of the class in order to control its limited responsibilities.

## Facade and static functions

The facade provides a `static` interface for the binding classes in the service container.

CatLib's facade acts as a **static proxy** for the underlying classes in the service container, providing maintenance that is easier to test, flexible, and concise than traditional static methods.