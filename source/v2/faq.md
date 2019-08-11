---
title: FAQ
---

# FAQ

This document describes the errors that developers often encounter to help developers resolve issues quickly.

## Dependency injection

- Attribute dependency injection(InjectAttribute) is always performed after constructor dependency injection. Please do not use attribute dependency injection objects in the constructor.
- Occurs when injected for a class: `System.MissingMethodException`: There is no constructor that defines no arguments for this object. Please check if the constructor is accessible by `public`.
- Check if the service provider is already registered when an `UnresolvableException` occurs

## Runtime exception

- Compiled without error, the runtime prompts `TypeLoadException: Could not load type ...`, there may be a module A that is dependent on the upgrade, resulting in a problem with the dll version, please recompile the dll that depends on the A module.