---
title: Event Dispatcher
---

# Event Dispatcher

The event dispatcher is a good way to decouple applications. The CatLib event system allows us to subscribe to and listen to various events that appear in the program.

[Application](../architecture/application.html) has provided an simple event system by default for global event use. If you want to define a private scope event you can do this:

``` csharp
var dispatcher = new EventDispatcher();
```

## Add listener

With the `AddListener` method you can register a listener for an event.

``` csharp
dispatcher.AddListener("event.name", (object sender, EventArgs args) =>
{
});
```

## Dispatch

If you want to trigger an event, you can trigger the event with `Dispatch`. This method will dispatch the event to all of its registered listeners.

``` csharp
dispatcher.Dispatch("event.name", eventArgs);
```

## Has listener

You can use `HasListener` to determine if an event listener exists.

``` csharp
dispatcher.HasListener("event.name");
```

## Stoppable event

The CatLib event system supports implementing an `IStoppableEvent` interface to determine whether to terminate the delivery of an event.

``` csharp
public class FooEventArgs : EventArgs, IStoppableEvent
{
    public bool IsPropagationStopped => true;
}
```

## Remove listener

You can removed the specified event listener with the `RemoveListener` method.

``` csharp
dispatcher.RemoveListener("event.name", listener);
```

#### Remove all listeners under the event

```csharp
dispatcher.RemoveListener("event.name");
```