# Introduction

PDFRasterizer.NET 4.0 is a .NET class library for rasterizing PDF documents.

This developer guide will help you understand the PDFRasterizer.NET class library. It walks through the main concepts and illustrates them with code samples. 

## What's new

Starting with version 4.0, PDFRasterizer.NET renders pixel perfect due to an overhaul of our render engine. The difference are presented in detail in [Precise rendering](preciserendering).

PDFRasterizer.NET 4.0 is .NET Core and Xamarin compatible. The .NET Standard 1.5 and .NET Standard 2.0 builds allow you to render PDF documents on Linux and MacOS. 
A .NET Framework 4.6 build is included as well.

## Install

The previous version PDFRasterizer.NET 3.0 was implemented as a single assembly that could be easily referenced and XCopy deployed.
The current version 4.0 has dependencies that cannot be (easily) resolved manually. PDFRasterizer.NET 4.0 uses the NuGet package manager for .NET.

```
Install-Package TallComponents.PDFRasterizer4
```

IDE's such as Microsoft's Visual Studio .NET and JetBrains' Rider provide a user interface for installing the PDFRasterizer.NET 4.0 NuGet packag.

See PDFRasterizer.NET 4.0's page on nuget.org for details and history:
[https://www.nuget.org/packages/TallComponents.PDFRasterizer4](https://www.nuget.org/packages/TallComponents.PDFRasterizer4/)

## Licensing

PDFRasterizer.NET 4.0 runs either in licensed or evaluation mode. 
In evaluation mode, rendered images are stamped with an evaluation notice.  
To run in licensed mode, a valid license key must be registered. Contact sales@tallcomponents.com for details.




