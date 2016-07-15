---
layout: page
title: FAQ
permalink: /faq/
---

#### What platforms are supported?
Windows, Linux, OSX, Android and iOS.

#### Which version is the official one?
[https://github.com/opentk/opentk/](https://github.com/opentk/opentk/ "The official OpenTK repository")

#### How does this relate to the Xamarin OpenTK?
The Xamarin version is a fork of an ancient version.
You should prefer this where possible.

#### How can I get it?
Add the NuGet package to your project.

#### Who uses OpenTK?
OpenTK is used by numerous scientific renderers and games.

### How fast is OpenTK?
For a .Net library, OpenTK is very fast. It uses hand-optimized IL assembly to minimize overhead when calling OpenGL functions. However, keep in mind that the underlying runtimes (.Net/Mono) use garbage collection which introduces some unique performance considerations - refer to our documentation for more information. We place significant emphasis on performance, so please open an issue if you believe something could run faster!

### What kind of abstraction does OpenTK provide?
The Open Toolkit mirrors the raw OpenGL API, with the addition of type-safe enums. This was a conscious design decision that ensures high performance, easy translation of C/C++ code, and beyond that, matches the OpenTK philosophy of being a toolkit, rather than a framework or engine.

#### How do I use this?
1. Add the NuGet Package to your project
1. Open the appropriate namespace
1. Start coding

#### I need help!
Try asking on the [Gitter channel](https://gitter.im/opentk/opentk).

[![Gitter](https://badges.gitter.im/opentk/opentk.svg)](https://gitter.im/opentk/opentk?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

#### Something is broken!
[Open an issue at the repo](https://github.com/opentk/opentk/).

#### Who are the maintainers?
[@thefiddler](https://github.com/thefiddler) is the head-honcho.
Others include [@amulware](https://github.com/amulware), [@wcdeich4](https://github.com/wcdeich4), [@varon](https://github.com/varon), and [@cra0zy](https://github.com/cra0zy).

#### How can I help?
Check outstanding issues, or drop us a message in Gitter.
