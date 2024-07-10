### OpenTK

The Open Toolkit is set of fast, portable, low-level C# bindings for OpenGL, OpenGL ES, OpenAL, and OpenCL.  It runs on all major platforms and powers hundreds of apps, games, and scientific research programs.

OpenTK provides several utility libraries, including a math/linear algebra package, a windowing system, and input handling.

OpenTK is not a game engine, nor does it try to be one:  It offers a useful set of tools and lets you decide how to use them.  It is the ideal starting point if you want to write a game or scientific application from the ground up.

### Current Status

- **OpenTK 4.x** is the current, stable version.  It targets .NET Core 3.x and .NET 5+.
   - [OpenTK 4.x](https://www.nuget.org/packages/OpenTK/) [![NuGet](https://img.shields.io/nuget/v/OpenTK.svg)](https://www.nuget.org/packages/OpenTK/) [![NuGet](https://img.shields.io/nuget/dt/OpenTK.svg)](https://www.nuget.org/packages/OpenTK/)
   - [OpenTK 4.x GLWpfControl](https://www.nuget.org/packages/OpenTK.GLWpfControl/) (WPF control)
   - [OpenTK 4.x GLControl](https://www.nuget.org/packages/OpenTK.WinForms) (WinForms control)
- **OpenTK 3.x** is the legacy version.  It targets .NET Framework 4.x.
   - [OpenTK 3.x](https://www.nuget.org/packages/OpenTK/3.3.1)
   - [OpenTK GLControl 3.x](https://www.nuget.org/packages/OpenTK.GLControl/) [![NuGet](https://img.shields.io/nuget/v/OpenTK.GLControl.svg)](https://www.nuget.org/packages/OpenTK.GLControl/) [![NuGet](https://img.shields.io/nuget/dt/OpenTK.GLControl.svg)](https://www.nuget.org/packages/OpenTK.GLControl/)
- **OpenTK 5.x** is the next version of OpenTK. It brings a new OpenGL bindings generator, Vulkan bindings and a new alternative windowing API written completely in C# (called PAL2). We welcome any [contributions](https://github.com/opentk/opentk/)!
   - [OpenTK 5.x](https://www.nuget.org/packages/OpenTK/5.0.0-pre.10) (preview package)
   - [OpenTK 5.x documentation](https://nogginbops.github.io/opentk.net/) (preview containing PAL2 tutorials)

### Help and Support

If you have a question or want to know how to get started, check our extensive [FAQ](faq.md), or try our tutorials for [OpenTK 4 and OpenTK 3](learn/index.md).

The OpenTK source code is available [on GitHub](https://github.com/opentk), and the latest build can be found [on Nuget](https://www.nuget.org/packages/OpenTK/).  If you find a bug or defect, please let us know with a [GitHub issue](https://github.com/opentk/opentk/issues).

### Project status (2024-07-10)

OpenTK 3 (.NET framework) has had it's probably last release and will likely not be updated anymore.

OpenTK 4 (.NET core 3.1+) is considered the "current" version of OpenTK. We try and fix any issues reported. If you have any problems, report them here: https://github.com/opentk/opentk/issues/new

OpenTK 5 (.NET 5+) is currently under active development. Not considered stable yet. OpenTK 5 brings a new OpenGL bindings generator, a Vulkan bindings generator and a new alternative windowing system written completely in C#. Follow the progress here: https://github.com/opentk/opentk/issues/1207

### NuGet

[OpenTK](https://www.nuget.org/packages/OpenTK/) [![NuGet](https://img.shields.io/nuget/v/OpenTK.svg)](https://www.nuget.org/packages/OpenTK/) [![NuGet](https://img.shields.io/nuget/dt/OpenTK.svg)](https://www.nuget.org/packages/OpenTK/)

[OpenTK.GLControl](https://www.nuget.org/packages/OpenTK.GLControl/) [![NuGet](https://img.shields.io/nuget/v/OpenTK.GLControl.svg)](https://www.nuget.org/packages/OpenTK.GLControl/) [![NuGet](https://img.shields.io/nuget/dt/OpenTK.GLControl.svg)](https://www.nuget.org/packages/OpenTK.GLControl/)

[OpenTK.GLWpfControl](https://www.nuget.org/packages/OpenTK.GLWpfControl/) [![NuGet](https://img.shields.io/nuget/v/OpenTK.GLWpfControl.svg)](https://www.nuget.org/packages/OpenTK.GLWpfControl/) [![NuGet](https://img.shields.io/nuget/dt/OpenTK.GLWpfControl.svg)](https://www.nuget.org/packages/OpenTK.GLWpfControl/)

If you encounter any issues, please let us know with a GitHub issue.

We have an active, friendly [Discord server](https://discord.gg/6HqD48s); if you need help, feel free to join and ask a question in the `#support` channel!

### License

The Open Toolkit is distributed under the permissive MIT/X11 license and is absolutely free.

[![View License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/opentk/opentk/blob/master/License.txt)
