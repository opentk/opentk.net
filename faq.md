#### What platforms are supported?
Windows, Linux, OSX, Android and iOS.

#### Which version is the official one?
* [https://github.com/opentk/opentk/](https://github.com/opentk/opentk/) is the official source-repository.
* [https://www.nuget.org/packages/OpenTK/](https://www.nuget.org/packages/OpenTK/) is the official nuget-package.

Please note any other distribution of the package is not officially supported.

#### How does this relate to the Xamarin OpenTK?
The Xamarin version is a fork of an ancient version.
You should prefer this where possible.

#### How can I get it?
Add the [NuGet package](https://www.nuget.org/packages/OpenTK/) to your project.

#### Who uses OpenTK?
OpenTK is used by numerous scientific renderers and games.

#### How fast is OpenTK?
For a .Net library, OpenTK is very fast. It uses hand-optimized IL assembly to minimize overhead when calling OpenGL functions. However, keep in mind that the underlying runtimes (.Net/Mono) use garbage collection which introduces some unique performance considerations - refer to our documentation for more information. We place significant emphasis on performance, so please open an issue if you believe something could run faster!

#### What kind of abstraction does OpenTK provide?
The Open Toolkit mirrors the raw OpenGL API, with the addition of type-safe enums. This was a conscious design decision that ensures high performance, easy translation of C/C++ code, and beyond that, matches the OpenTK philosophy of being a toolkit, rather than a framework or engine.

#### How do I use this?
1. Add the NuGet Package to your project
2. Open the appropriate namespace
3. Start coding

#### I need help!
Try asking on our [Discord server](https://discord.gg/6HqD48s).

[![Discord](https://discordapp.com/api/guilds/337627185248468993/widget.png)](https://discord.gg/6HqD48s)

#### Something is broken!
[Open an issue at the repo](https://github.com/opentk/opentk/).

#### Who are the maintainers?
[@thefiddler](https://github.com/thefiddler) is the original author.
Current maintainers are [@frederikja163](https://github.com/frederikja163), [@jvbsl](https://github.com/jvbsl), [@PJB3005](https://github.com/PJB3005), and [@varon](https://github.com/varon).
Previous maintainers include [@Nihlus](https://github.com/Nihlus), and [@Perksey](https://github.com/Perksey)

#### How can I help?
Check outstanding issues, or drop us a message in Discord.
