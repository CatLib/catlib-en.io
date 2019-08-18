---
title: Upgrade
---

# Upgrade

> CatLib's version standard is: [Semver semantic version standard](http://semver.org/lang/zh-CN/)

## Summary

Version format: `major.minor.revision.increment` rule is as follows:

- **Major**: When you do an incompatible API modification.
- **Minor**: When you do a backward compatible feature addition,
- **Revision**: When you made a backward compatibility fix.

## [v2.0.0 (unreleased)](https://github.com/CatLib/Core/releases/tag/v2.0.0)

#### Added

- `Inject` allowed to be set to optional.({% issues 253%} )

#### Changed

- Comments translated from Chinese into English({% issues 133%} )
- Defined Container.Build as a virtual function({% issues 210%} )
- Optimizes the constructor of `MethodContainer`({% issues 218%} )
- The default project uses the .net standard 2.0({% issues 225%} )
- Rename Util helper class to Helper class Change access level to internal.({% issues 230%} )
- `Application.IsRegisted` changed(rename) to `IsRegistered`({% issues 226%} ) 
- Use `VariantAttribute` to mark variable types instead of `IVariant`({% issues 232%} )
- `Guard` Will be expandable with `Guard.That`({% issues 233%} )
- Fixed the problem of container exception stack loss({% issues 234%} )
- Adjusted the internal file structure to make it clearer({% issues 236%} ).
- Add code analyzers ({% issues 206%} )
- Refactoring event system ({% issues 177%} )
- Refactoring `RingBuffer` make it inherit from `Stream`.({% issues 238%} )
- Namespace structure adjustment(optimization).({% issues 241%} )
- `App` can be extended by `That` (Handler rename to that) and removed `HasHandler` API ({% issues 242%} )
- Unnecessary inheritance: WrappedStream({% issues 247%} )
- Clean up useless comment({% issues 249%} ).
- `Guard.Require` can set error messages and internal exceptions({% issues 250).
- Exception class implementation support: SerializationInfo build({% issues 252%} ).
- Refactoring unit test, import moq.({% issues 255%} )
- `CodeStandardException` replaces to `LogicException`({% issues 257%} )
- Exception move to namespace `CatLib.Exception`({% issues 258%} )
- `Facade<>.Instance` changed to `Facade<>.That`({% issues 259%} )
- `Application.StartProcess` migrate to `StartProcess`({% issues 260%} )
- `Arr` optimization, lifting some unnecessary restrictions ({% issues 263%} )
- `Str` optimization, lifting some unnecessary restrictions ({% issues 264%} )
- Refactoring `SortSet`({% issues 265%} )
- Removed global params in application constructor. use Application.New() instead.({% issues 267%} )
- Containers are no longer thread-safe by default({% issues 270%} )

#### Fixed

- Fixed a bug that caused `Arr.Fill` to not work properly under special circumstances. ({% issues 255%} )

#### Removed

- Removed `ExcludeFromCodeCoverageAttribute` ({% issues 229%} )
- Removed unnecessary interface design `ISortSet`({% issues 211%} ).
- Removed `Version` classes and `Application.Compare` method.({% issues 212%} ).
- Removed `Template`  supported({% issues 213%} ).
- Removed `FilterChain` supported({% issues 214%} ).
- Removed `Enum` supported({% issues 215%} ).
- Removed `IAwait` interface({% issues 217%} ).
- Removed `Container.Flash`  api({% issues 219%} ).
- Removed `Arr.Flash` method({% issues 220%} ).
- Removed `Dict` helper class({% issues 221%} ).
- Removed `ThreadStatic` helper class({% issues 223%} ).
- Removed `QuickList` supported({% issues 224%} ).
- Removed `Storage` supported({% issues 228%} )
- Removed `SystemTime` class({% issues 235%} ).
- Removed `ICoroutineInit` feature from core library({% issues 243%} ).
- Removed the priority attribute, depending on the loading order({% issues 244%} ).
- Removed `Util.Encoding` ({% issues 245%} ).
- Removed `Str.Encoding`({% issues 246%} )
- Removed `IServiceProviderType` feature in core library({% issues 246%} ).
- Removed unnecessary extension functions({% issues 247%} ).
- Removed `PipelineStream` stream.({% issues 256%} )
- Removed all `Obsolete` method and clean code.({% issues 261%} )
- Removed `App.Version`.({% issues 266%} )

## 1.x

The minimum version of the English version is 2.0 so the migration does not exist in the English version. Learn [chinese version](https://cn.catlib.io).
