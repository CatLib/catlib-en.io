---
title: Guard
---

# Guard

Guard allows you to write assertion code in simple and elegant code. The guard is expandable.

## Methods

#### That

Guard instances can be obtained via **That**, so you can extend the Guard with the extension function.

```csharp
var guard = Guard.That;
```

#### Requires

Validate the condition and throw an exception if the condition fails.

```csharp
Guard.Requres<ArgumentNullException>(arg != null, $"Argument {nameof(arg)} cannot be null.");
```

#### ParameterNotNull

Verify that the specified parameter is not empty.

```csharp
Guard.ParameterNotNull(arg, nameof(arg));
```

## Extend

When you are guarding, you can extend the exception's extend method so that you can generate some complex exception relationships.

```csharp
Guard.Extend<ArgumentNullException>((message, innerException, state) =>
{
    return new ArgumentNullException(message, innerException);
})
```