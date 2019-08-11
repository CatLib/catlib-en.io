---
title: Container
---

# Container

The Container service container is a powerful tool for managing **class's dependencies** and **execute dependency injections**. The essence is to inject the **constructor** or the **property** marked as `[Inject]` by reflection.

## Introduction

Almost all service bindings are done in the service provider. If a service is not based on any interface then there is no need to bind it to the container.

The container does not need to be told how to build the object, because it will automatically parse the concrete object instance using reflection technology.

In the service provider we can use the `Bind` or `Singleton` method to register a service binding.

[Application] (application.html) inherits from the service container in the CatLib core library.

## Make Service

You can build the service with the `Make` method. If a container is not used to generate a service that is not bound, the container will attempt to resolve the service that needs to be generated according to the policy. If the attempt fails, an `UnresolvableException` exception will be thrown.

```csharp
var foo = app.Make<IFoo>();
```

#### User custom parameters

CatLib supports custom parameters when building a service, all you need to do is:

```csharp
var foo = app.Make<IFoo>("params-1", "params-2");
```

## Bind Service

To use the power of the service container you first need to bind the service to the service container. If you don't bind, then you will not have any services available. These bindings are usually in the [service provider](service-provider.html).

The binding service is divided into the following two types：

- **Bind**: A new instance is generated for each call.
- **Singleton**: Each call always returns the same instance.

#### Bind

The `Bind` method allows the service to be bound as an binding.

``` csharp
app.Bind<IFoo, Foo>();
```

We bind the `IFoo` interface to the `Foo` implementation via the above code. This way you can use the interface to build the service:

``` csharp
var foo1 = App.Make<IFoo>();
var foo2 = App.Make<IFoo>();
// foo1 != foo2
```

#### Singleton

The service container uses the `Singleton` method to bind the service to a singleton. Once the singleton binding is built, each call always returns the same instance.

``` csharp
app.Singleton<IFoo, Foo>();
```

```csharp
var foo1 = app.Make<IFoo>();
var foo2 = app.Make<IFoo>();
// foo1 == foo2
```

> Through the combination of [facade](facade.html) and service containers, the traditional singleton implementation is subverted, and the traditional singleton mode is implemented in an updated and easier to maintain way.

## Try bind service

You can use `BindIf` or `SingletonIf` to conditionally determine the binding. If the bound service does not exist, then the binding is performed, otherwise no action is taken.

```csharp
app.BindIf<IFoo, Foo>();
```

```csharp
app.SingletonIf<IFoo, Foo>();
```

## Indexer

The service container supports binding of some simple values through an indexer, and the binding by the indexer is always done in the [Bind Binding](#bind) mode.

#### Bind with indexer

```csharp
container["foo"] = "bar";
```

#### Get instance with indexer

```csharp
var bar = container["foo"];
```

## Constructor injection

In your simple `Make` operation, the service container has performed a series of complex operations on the bound service (service relationship transformation, service construction, service speculation, context processing, dependency injection, service modification, singletonation...), all this is completely transparent and non-perceived to the developer.

The service container automatically recognizes the parameters in your constructor and injects the appropriate service instance for it. For the injection scheme, please refer to: [Dependency Injection Rule](#dependency-injection-rule)

``` csharp
public class CustomizeService
{
    public Service(IFoo foo)
    {
        // foo will be automatically injected.
        // throw UnresolvableException if service not found.
    }
}
```

In short, you only need to bind the services you develop to the service container, and the service container will automatically handle your service dependencies when you generate the service.

> Note that only the constructor is `public` can be injected

## Inject Attribute

The service container uses the `InjectAttribute` to identify which **property** are allowed to be injected.

Any property that can be injected must satisfy the following rules:

- The property must mark the `[Inject]` attribute
- Protpery `set` field access level is `public`。

``` csharp
public class CustomizeService
{
    [Inject]
    public IFoo Foo { get; set; }
}
```

> You can't access an instance of the `property` that is marked as injected in the constructor, because the property inject will be done after the constructor inject, if you access it in the constructor, this will result in a `NullReferenceException` exception.

#### Optional inject

Property inject can be used the `Required` flag indicates that this property is optional.

```csharp
public class CustomizeService
{
    [Inject(Required = false)]
    public IFoo Foo { get; set; } = new Foo();
}
```

## Service alias

The `alias` is a very important feature of the service container, which provides the ability to bind an interface to a specified implementation.

``` csharp
app.Singleton<IFoo, FooBar>();
```

Or point multiple interfaces to the same instance.

``` csharp
app.Singleton<IFoo, FooBar>().Alias<IBar>();
```

## Release singleton instance

The container allows you to release the **singleton bind instance** that has been generated using the `Release` method. [Release event](#release-event) is triggered after release

```csharp
app.Release<IFoo>();
```

#### IDisposable supported

For the release of the generated singleton service, it checks if the service implementation implements the `IDisposable` interface. If the interface is implemented, it will be called automatically.

> If the service fails to find it in the container, the `Release` function will return `false`.

## Unbind

You can unbind the already bound service with `Unbind`. If the bound class is singletoned and has been built, then the unbind will automatically release and trigger the [Release event] (#release-event).

``` csharp
var binder = app.Singleton<IFoo, Foo>();
binder.UnBind();
```

## Resolving event

The service container trigger the service resolving event each time a new instance is created, which can be listened to using the `OnResolving` method.

This event can be used to decorate or parameterize the constructed service and allows you to set additional properties for the service object before it is passed to the developer.

The resolving event is divided into 2 types. The **local resolving event** will be called in preference than the **global resolving event**. The characteristics of the resolving event are as follows:

- A local resolving event is only triggered when the specified service is built.
- A global resolving event triggers whenever any service is built.
- The resolving event for a singleton service will only trigger once on the first resolving(unless release).
- Setting the service instance with `Instance` will trigger a service resolving event.

#### local resolving event

``` csharp
app.Singleton<IFoo, Foo>().OnResolving<Foo>((foo) =>
{
});
```

#### global resolving event

- Decorate for all services

``` csharp
app.OnResolving((instance) =>
{
});
```

- Decorate for specified services

```csharp
app.OnResolving<IFoo>((foo) =>
{
    // decorate for all services that implement the IFoo interface
});
```

## After resolving event

The service container trigger the **after resolving event** after the **service resolving event**. You can listen for the event using the `OnAfterResolving` method.

It is useful for programs that need to be verified after the event is resolved.

#### local after resolving event

``` csharp
app.Singleton<IFoo, Foo>().OnAfterResolving<Foo>((foo) =>
{
});
```

#### global after resolving event

- Decorate for all services

``` csharp
app.OnAfterResolving((instance) =>
{
});
```

- Decorate for specified services

```csharp
app.OnAfterResolving<IFoo>((foo) =>
{
    // decorate for all services that implement the IFoo interface
});
```

## Release event

When the container uses the `Release` method or other release methods (Unbind, Instance, Flush) to release the generated singleton service, a service release event is triggered.

In the release event, you can perform the final processing of the service (such as: resource release, close the associated service).

The release event is divided into 2 types, the **local release event** will be called in preference than the **global release event**, and you can listen to the event using the `OnRelease` method. The following behaviors may trigger a release event:

- A new instance via `Instance`, and the old instance will be triggered to release the event.
- Use `Release` to release exists service.
- Unbind the service by `Unbind`.
- Use `Flush` to cleanup the container.

#### local release event

``` csharp
app.Singleton<IFoo, Foo>().OnRelease<Foo>((foo) =>
{
});
```

#### global release event.

- Decorate for all services

``` csharp
app.OnRelease((instance) =>
{
});
```

- Decorate for specified services

```csharp
app.OnRelease<IFoo>((foo) =>
{
    // decorate for all services that implement the IFoo interface
});
```

> This event is not triggered when the service via [Bind](#bind)'s service been released.

## Rebound event

When the service implementation changes, a rebound event is triggered. By focusing on this event, the service change can be obtained. The rebound event is valid for both the singleton binding and the instance binding service. The following behaviors will trigger the redefinition event:

- When rebinding a service that has been bound and built.
- Use `Instance` to change the singleton service implementation that has been built.

The following behavior will not cause a rebound event:

- Rebound events is not trigged when rebinding a service that has never been built
- When the service instance is released.

You can listen to the service change through the `OnRebound` or `Watch` methods(The Watch method is the OnRebound extension alias method).

``` csharp
app.Watch<IFoo>((newInstance)=>
{
});
```

> - By rebound events, your program can always be implemented with the latest services.
> - The service implementation obtained through [Facade](facade.html) is always up to date.

## Instance(Singleton)

The service container can instantiate your own generated instance via `Instance`, and then fetching the instance through the container always returns the managed instance.

``` csharp
app.Instance("foo" , new Foo());
```

Note that if you use `Bind` for the specified service, then if you try to instantiate this service instance, the service container will throw a `LogicException` exception:

``` csharp
app.Bind("foo", (container, userParams)=> new Foo());
app.Instance("foo" , new Foo()); // throw LogicException
```

## Tag/Tagged

You can group multiple services, which allows you to generate multiple services at once, and service grouping is grouped using `Tag`.

This is very useful in some scenarios such as:

- The modules required in the game battlefield can be uniformly loaded and unified release through service grouping.
- Logs will be logged to the file log service and web log service

```csharp
public class Tags
{
    public const BattleManagerTags = "BattleManagerTags";
}
```

- Tagged service

``` csharp
app.Singleton<IBattleMonster, BattleMonsterManager>()
   .Tag(Tags.BattleManagerTags);
app.Singleton<IBattleMonster, BattleCameraManager>()
   .Tag(Tags.BattleManagerTags);
```

- Make with tag

```csharp
app.Tagged(Tags.BattleManagerTags); 
// { BattleMonsterManager, BattleCameraManager }
```

#### Released

You can use the `Release` method to release the tagged objects, which may be very convenient in some scenarios, such as unifying the singleton object of the game battlefield when exiting the game battlefield.

```csharp
var services = app.Tagged(Tags.BattleManagerTags);
```

```csharp
app.Release(ref services);
```

> If any of the services fail to be found from the container, `false` will be returned, and a service implementation that fails to be found in the container will appear in the `ref service` variable.

## Context binding

Sometimes we may have two different instance using the same interface, but we want to inject different implementations into each service, we can solve this problem through context binding.

#### Describe the context by binding the type name

```csharp
app.Singleton<ScreenshotUpload>()
    .Needs<IDisk>()
    .Given(()=> FileSystem.Disk("aliyun-oss"))
```

When the screenshot upload service requires an abstract disk, the closure is used to provide a disk under the file system manager.

#### Describe the context by binding the variable name

The bound variable name must start with `$` and be sensitive to the case of the variable name.

```csharp
app.Singleton<ScreenshotUpload>() 
    .Needs("$disk")
    .Given(()=> FileSystem.Disk("aliyun-oss"))
```

When building the screenshot upload service, the framework looks up the `disk` variable in the screenshot upload service constructor and gives it a given implementation.

## Extended service

The service container allows you to modify a singleton service that has already been built with `Extend` or to create additional grooming for services that have not yet been built.

If `Extend` is a service that has already been built, then the extension code will take effect immediately, but will not affect subsequent services (the specified build service is released and then rebuilt).

If the `Extend` service is a service that has not yet been built, then the extension will continue to take effect for the specified service built in the future.

If `Extend` does not specify a service name, it will take effect globally, as long as the interface, type that meets the specified conditions is triggered.

```csharp
app.Extend<IFoo>((foo) =>
{
    return foo;
});
```

## Bind method

In addition to the service container binding, the service container can also perform method(function) binding. You can bind the function through the `BindMethod` method. The service container will automatically analyze the parameters required by the binding function and provide appropriate parameters. inject parameters.

``` csharp
app.BindMethod("foo" , ()=>
{
});
```

If you need to do [context binding](#context-binding) for the bound function arguments, you can do this:

``` csharp
var binder = app.BindMethod("foo" , (IFileSystem fileSystem) => { });
binder.Needs<IFileSystem>.Given(()=> new FileSystem());
```

## Invoke bind method

A bound function can be called with the `Invoke` method. You can pass custom parameters to the called function. These parameters will be injected into the function parameters according to the [dependency injection rule](#dependency-injection-rule) of the service container. If the method parameters cannot be solved, then an `UnresolvableException` exception will be thrown.

``` csharp
app.Invoke("foo", /* Your params*/);
```

## Unbind bind method

A binding function can be removed by the `UnbindMethod` method, which supports the following lifting methods:

- If the argument passed in is a string (`string`), the function name is removed as a method name.
- If the passed argument is a method bound object (`IMethodBind`) then the corresponding function is released.
- If the passed argument is another object (`object`) then the call target, remove all functions based on this call target

``` csharp
app.UnbindMethod("foo");
```

## Call method with dependency injection

The `Call` method can be used to initiate a dependency injection call to any `method` or `Lambda`, and the container will automatically resolve the parameters required by the function according to the [dependency injection rule](#dependency-injection-rule).

``` csharp
app.Call((IFoo foo) =>
{
});
```

## OnFindType

When the container attempts to speculate on a service that has not been bound, it will query the `Type` of the service by traversing the query function registered by `OnFindType`.

In general, this usage scenario is mostly used for Type lookups across assemblies.

``` csharp
app.OnFindType((finder)=> 
{ 
    return Type.GetType(finder); 
});
```

> When the generated service relies on `OnFindType` to infer the service, the service build will use the reflection service of c#, which is a serious loss of performance. So you should use singleton binding instead of relying on speculation to generate it as much as possible.

## Variant

Classes marked with the `VariantAttribute` attribute will be considered by the container to be mutable, and the mutable type allows the container to convert the underlying type parameters passed in by the developer to a mutable type.

The default base type parameter is: `Boolean`,`Byte`,`SByte`,`Int16`,`Int32`,`UInt32`,`Int64`,`IntPtr`,`UIntPtr`,`Char`,`Double`,`Single`,`String`.

```csharp
[Variant]
public class ItemModel
{
    public string itemName;
    public ItemModel(int itemId)
    {
        this.itemName = (itemId == 10) ? "dress" : "headwear";
    }
}
```

```csharp
public class ItemUI
{
    public ItemUI(string name, ItemModel model)
    {
        // name : foo
        // model.itemName : dress
    }
}
```

```csharp
app.Make<ItemUI>("foo", 10);
```

> With variable types, you can hide cumbersome conversions and the framework will automatically convert between types.

## IConvertible

The service container supports conversions for implementing the `IConvertible` interface object.

```csharp
public class Foo
{
    public Foo(string bar)
    {
        // bar : 100
    }
}
```

```csharp
app.Make<Foo>(100);
```

> The framework will prioritize the conversion of the variable type, ie if the variable type conversion is possible, the variable type conversion will be used first.

## Flush the container

You can use `Flush` to reset the service container, clear all the data in the service container, and restore to the initial state.

``` csharp
app.Flush();
```

> In the process of Flush resetting the container, if it is a singleton service, a [service release event] (#release-event) will be trigged.

## Parameter table/parameter matcher

The service container allows the developer to pass in the parameter list to match the parameters of the dependency injection. If you need to use the parameter table, you need to provide the data structure that implements the `IParams` interface in the `Make` service.

The framework already provides the default parameter table structure: `Params`, the default parameter table is matched according to `variable name` and is case sensitive.

- The parameter table trigger order is the order of the parameter tables passed in.
- The matching of the parameter table is the highest priority.
- The parameter type provided by the parameter table will try to perform parameter type conversion if it is inconsistent with the injection parameter type. If the conversion fails, it will use [dependency injection rule] (#dependency-injection-rule).

```csharp
public class Foo : IFoo
{
    public Foo(IBar bar)
    { 
    }
}
```

```csharp
app.Make<Foo>(new Params()
{
    { "bar" , new Bar() }
});
```

## Tightening parameter injection

The service container allows all parameters passed in by the user to be accepted at once by the special type `object` or `object[]`.

```csharp
public class Foo
{
    public Foo(object[] args)
    { 
        // args[0] == 1
        // args[1] == "foo"
    }
}
```

```csharp
App.Make<Foo>(1, "foo");
```

If the receive type is `object` and the number of arguments passed is 1, then the argument will be automatically deconstructed, otherwise it will be an array of `object[]`.

```csharp
public class Foo
{
    public Foo(object arg)
    { 
        // arg == "str"
    }
}
```

```csharp
App.Make<Foo>("str");
```

## Dependency injection rule

- **Execution constructor injection**
- If the implementation is built through a closure, the implementation is immediately built through the closure and jumps to: `Execution of the instance's feature injection` to continue execution.
- Traverse the parameter list:
-- **Get the parameters that can be used for injection by parameter matcher**
----- The default matcher matches a parameter name with an incoming match table to return a valid result.
-- **Check if the austerity injection parameter (`object` or `object[]`), and can do austerity injection:**
----- Yes: Inject the parameters passed in by the user tightly, and clear the parameters passed in by the user.
-- **Pick and pop the appropriate parameters from the parameters passed in by the remaining users.**
-- Use a dependency injection container for analysis:
----- **If it is: class, interface injection:**
--------- Build with context closures
------------- Pass the required interface name (class name), and build it if there is a closure.
------------- Pass in the parameter variable name and build it if there is a closure.
--------- Build by context service name
------------- The incoming request interface name (class name), if there is a context service name, is built using the context service name.
------------- Incoming parameter variable name, if there is a context service name, it is built using the context service name.
--------- Build by the required interface name (class name).
--------- Check if the default value can be used, if it can be used, return it with the default value.
--------- If both fail: throw an `UnresolvableException` exception
----- **If it is a basic type injection:**
--------- Build with context closures
------------- Pass the required interface name (class name), and build it if there is a closure.
------------- Pass in the parameter variable name and build it if there is a closure.
--------- Build by context service name
------------- The incoming request interface name (class name), if there is a context service name, is built using the context service name.
------------- Incoming parameter variable name, if there is a context service name, it is built using the context service name.
--------- Build by the required interface name (class name).
--------- Check if the default value can be used, if it can be used, return it with the default value.
--------- If both fail: throw an `UnresolvableException` exception
- **Feature injection of the execution instance**
- Traverse the parameter list:
-- **Check readable and writable status, skip it when not**
-- Use a dependency injection container for analysis:
----- **If it is: class, interface injection:**
--------- Build with context closures
------------- Pass the required interface name (class name), and build it if there is a closure.
------------- Pass in the property name and build it if there is a closure.
--------- Build by context service name
------------- The incoming request interface name (class name), if there is a context service name, is built using the context service name.
------------- Pass in the property name, if there is a context service name, build with the context service name.
--------- Build by the required interface name (class name).
--------- If both fail: throw an `UnresolvableException` exception
----- **If it is a basic type injection:**
--------- Build with context closures
------------- Pass the required interface name (class name), and build it if there is a closure.
------------- Pass in the property name and build it if there is a closure.
--------- Build by context service name
------------- The incoming request interface name (class name), if there is a context service name, is built using the context service name.
------------- Pass in the property name, if there is a context service name, build with the context service name.
--------- Build by the required interface name (class name).
--------- If both fail: throw an `UnresolvableException` exception
- Injection completed

## Other Method

- **Type2Service(type)** convert type to service name
- **GetBind(service)** Get the binding data of the service, or null if the binding does not exist
- **HasBind(service)** Is the service already bound?
- **HasInstance(service)** : Is there a single instance?
- **IsResolved(service)** : Has the service been resolved?
- **CanMake(service)** Can I generate a service?
- **IsStatic(service)** Whether the service is singletoned or false if the service does not exist
- **IsAlias(service)** Is the service name an alias?
- **BindIf(service, concrete)** Bind when the service does not exist (instance binding)
- **SingletonIf(service, concrete)** Bind when the service does not exist (singleton binding)
- **Wrap(lambda,params)** wraps a method that can be called in a dependency injection form
- **Factory(service)** wraps a method that triggers the ability to get the specified service

## Container behavior customization

When developers need to deeply customize container behavior, you can refactor the virtual methods provided below to adjust the container default behavior. Container behavior customization can only be done if you have a good understanding of the behavior of virtual functions.

- **Build**: Build a service instance
- **IsBasicType**: Is it the default base type (by default: primitive type + string)
- **IsUnableType**: Is it a type that cannot be built?
- **WrapperTypeBuilder**: wraps a type that can be used to generate a service
- **GetDependenciesFromUserParams**: Get the appropriate parameters from the parameters passed in by the user for injection.
- **ChangeType**: Convert type to the specified type.
- **GetPropertyNeedsService**: Pass in a property selector object (`PropertyInfo`) to get the service name of the corresponding object.
- **GetParamNeedsService**: Pass in a parameter object (`ParameterInfo`) to get the service name of the corresponding parameter requirement
- **ResolveAttrPrimitive**: When resolving the underlying type of the property selector, the return value is the service instance of the requirement.
- **ResloveAttrClass**: When the property selector class or interface type is resolved, the return value is the service instance of the requirement.
- **ResolvePrimitive**: When solving the underlying type of the constructor, the return value is the service instance of the requirement.
- **ResloveClass**: When solving the class or interface type of the constructor, the return value is the service instance of the requirement.
- **SpeculationServiceByParamName**: Infer the service based on a parameter information. The return value is the service instance of the requirement.
- **GetBuildStackDebugMessage**: Get debug information for the compiled stack.
- **MakeBuildFaildException**: Build an exception that failed to compile.
- **MakeUnresolvablePrimitiveException**: Build an exception that failed to resolve the underlying parameters.
- **MakeCircularDependencyException**: Build a circular dependency exception.
- **FormatService**: Format (standardized) service name.
- **CanInject**: Check if the instance can be injected.
- **GuardUserParamsCount**: Limits the number of parameters passed in by the user.
- **GuardResolveInstance**: Limits the valid conditions for resolving instances.
- **SpeculatedServiceType**: Predict the specified service type based on the service name.
- **AttributeInject**: Attribute injection for the specified instance.
- **CheckCompactInjectUserParams**: Checks if the parameters passed in by the user can be condensed.
- **GetParamsMatcher**: Get a parameter matcher, the valid result matched by the parameter matcher will be the optimal parameter value.
- **GetCompactInjectUserParams**: Get the user parameters for the compact injection.
- **GetDependencies**: Get the dependency injection result of the specified parameter list.
- **GetConstructorsInjectParams**: Select a suitable constructor and get the parameters that specify the injection.
- **GuardMethodName**: Verify that the function method name is valid.
- **GuardServiceName**: Verify that the function service name is valid.
- **GuardConstruct**: Verify that the current state is allowed to build the service (or call the function).
- **CreateInstance**: Creates an implementation of the specified type through reflection.
- **GetContextualClosure**: Get the closure of the requirement based on the context.
- **MakeFromContextualClosure**: Build an instance of the requirement based on the context closure.
- **GetContextualService**: Get the service name of the requirement according to the context
- **MakeFromContextualService**: Generates a service instance based on the context requirement service name.
- **ResloveFromContextual**: Resolve the service implementation through context.
- **MakeEmptyBindData**: Make an empty binding data.