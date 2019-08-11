---
title: Introduction
---

# Introduction

<a href="https://github.com/catlib/core/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" title="license-mit" /></a> <a href="https://ci.appveyor.com/project/catlib/core"> <img src="https://ci.appveyor.com/api/projects/status/tk3o571mwbw2rykj?svg=true" title="Build status"/></a> <a href="https://codecov.io/gh/CatLib/Core"><img src="https://codecov.io/gh/CatLib/Core/branch/master/graph/badge.svg" alt="Codecov" /></a> <img src="https://img.shields.io/nuget/dt/CatLib.Core.svg" alt="Downloads" />

## What is CatLib?

CatLib is a progressive service provider frameworks. The framework provides multiple implementations for the client and decouples them from multiple implementations. Service provider changes are transparent to their clients, which provides better scalability. It is easy to get started, and also easy to integrate with third-party libraries or existing projects.

- **CatLib Core**: Is the smallest available framework. Providing only the most basic functionality is ideal for other framework developers.
- **CatLib For Unity**: Added support for Unity's proprietary components (requires Unity 2017+) based on the Framework.

## The advantages of CatLib

- CatLib is a progressive framework that seamlessly blends with existing frameworks. No matter where your project is, you can easily access CatLib.
- The dependency injection provided by CatLib can greatly help project decoupling.
- CatLib offers a wide range of reliable, sustainable public components to help companies reduce development costs.
- Based on the MIT protocol, companies can build a proprietary public component library through CatLib's componentization solution to improve project development efficiency and quality.
- Lightweight framework, all components can be removed, you can choose only the components that are right for you.
- Complete documentation and low learning costs.
- For interface programming, the underlying components are not perceptually replaced.

## Learning roadmap

CatLib is easy to use. You only need to have a good C# foundation. You can invest in development very quickly by reading this guide.

- [Service provider](architecture/service-provider.html): Understand the basic concepts of service providers and system architecture.
- [Application](architecture/application.html): Understand the operational lifecycle of the core framework and the capabilities it has.
- [Container](architecture/container.html): Learn how the core container works and how it works.
- [Facade](architecture/facade.html): Understand the concept of service facades and compare conventional static methods.

## Installed

##### Installed with nuget

```PM
Install-Package CatLib.Core -Version 2.0.0
```

##### Download releases

Download [latest version](https://github.com/CatLib/Core/releases) from Github.

## Technical Support

- email: support@catlib.io
- slack: catlib.slack
- QQ gruop: [150371044](//shang.qq.com/wpa/qunwpa?idkey=ac3de81fa9b3a4379f80c44e05ff021bcfb51c0fb9092b0741762265a911878b) (verification: CatLib Support)