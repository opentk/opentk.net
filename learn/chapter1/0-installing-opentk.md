# Installing OpenTK

> Note: This tutorial does not apply to OpenTK 4

Welcome to the first entry to the LearnOpenTK tutorials. This guide will teach you how to get OpenTK from NuGet, so that you can get started with the rest of the tutorial. This will be a short one, we promise.

### NuGet
OpenTK is shipped on NuGet, the official package manager for .NET. Here is the [url](https://nuget.org/packages/OpenTK). OpenTK 3 can be installed on .NET Framework 2.0, and any derivative Mono version.

In Visual Studio 2013/2015/2017, to access the NuGet package manager simply click on `TOOLS`, hover over `NuGet` and select `Package Manager Console`

![Tools > NuGet > Package Manager Console](0-dropdown-nuget.png)

This will bring up the Package Manager Console - a PowerShell extension for Visual Studio and NuGet. To install OpenTK, simply type the following command into the Package Manager Console.

```
Install-Package OpenTK
```

![Install OpenTK](0-nuget-package-manager.png)

And that's it! OpenTK is installed and you're ready to proceed with the next tutorials!