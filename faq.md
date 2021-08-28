## Basics & Requirements

### What is OpenTK?

OpenTK is a library that provides high-speed access to native [OpenGL](#What-is-OpenGL), [OpenCL](#What-is-OpenCL), and [OpenAL](#What-is-OpenAL) for .NET programs.  OpenTK aims to make working with OpenGL, OpenCL, and OpenAL in .NET-supported languages like C# similar in both performance and feel to the equivalent C code, while still running in a managed (and safe!) environment.

Note that OpenTK is _not_ a high-level library:  It’s not a game engine, or a framework, or a full renderer, or a complete audio system by itself.  Instead, it’s the low-level foundation on which you can _build_ those kinds of things.  If you want a framework or a renderer or a game engine that’s already built, there are lots of those elsewhere on the Internet.

OpenTK also includes a convenient math library for common graphics types like vectors and matrices, so that you don’t have to implement those yourself or find another library to provide them.  These types are designed to be very fast and flexible, and they include considerable functionality out-of-the-box too.

#### What is OpenGL?

[OpenGL](https://en.wikipedia.org/wiki/OpenGL) is a cross-platform graphics-rendering library, originally developed by [Silicon Graphics (SGI)](https://www.sgi.com), and now maintained by the [Khronos group](https://www.khronos.org).  OpenGL is used for everything from video games to CAD tools to web browsers to mobile phones.

#### What is OpenCL?

[OpenCL](https://en.wikipedia.org/wiki/OpenCL) is a cross-platform parallel-computation specification, originally developed by [Apple](https://www.apple.com), and now maintained by the [Khronos group](https://www.khronos.org).  It’s primarily used for performing computation on GPUs that would otherwise be computed less efficiently on CPUs, but it can also be used to control digital signal processors (DSPs), field-programmable gate arrays (FPGAs), and other kinds of hardware compute accelerators.

#### What is OpenAL?

[OpenAL](https://openal.org/) is a cross-platform 3D audio library, originally developed by [Loki Software](https://en.wikipedia.org/wiki/Loki_Software), and released as an open specification by [Creative Labs](https://us.creative.com/), with multiple implementations in both hardware and software.  OpenAL is used in lots of games to provide both sound effects and music and is the successor to EAX and A3D.

### What operating systems are supported by OpenTK?

OpenTK runs on Windows, Linux, and MacOS X.

(Older versions of OpenTK supported Android and iOS as well; current OpenTK *might* work on these, but it’s untested.)

### What hardware is supported?

OpenTK supports any hardware that is supported by your OS’s native OpenGL, OpenCL, and OpenAL bindings:  Since OpenTK is just a .NET layer around the native libraries, it does whatever they do.

### Which .NET versions are supported?

- **OpenTK 4.x+** runs on .NET Core 3.x and .NET 5+.

- **OpenTK 3.x** runs on .NET Framework 4.x.

### Which version of OpenTK should I use?

If you’re able to use **.NET Core or .NET 5+**, you should use .NET Core or .NET 5+, and then use the latest version of **OpenTK 4** or higher with it.

If you’re stuck on the older .NET Framework 4.x, you will need to use OpenTK 3.x with it.  OpenTK 4.x+ is *not* compatible with .NET Framework, and there are no plans to support newer releases of OpenTK on .NET Framework.

### What’s the status of OpenTK 5.0?

OpenTK 5.0 is currently in development, and is not yet ready for release.  We have a [TODO list](https://github.com/opentk/opentk/issues/1207) tracks its current status.  We’d welcome any help, if you’d like to contribute!

### Which release is the official release?

- https://github.com/opentk/opentk/ is the official source-code repository for OpenTK.  There are also several related projects managed by [the OpenTK team](https://github.com/opentk/), including the [GLControl](https://github.com/opentk/GLControl) and [GLWpfControl](https://github.com/opentk/GLWpfControl) and [ObjectTK](https://github.com/opentk/ObjectTK).
- https://www.nuget.org/packages/OpenTK/ is the official Nuget package for OpenTK.  Similarly, there are [other Nuget packages](https://www.nuget.org/profiles/OpenToolkit) for the related projects.

All other distributions of OpenTK are not officially supported by the OpenTK team.  In particular:

- The Xamarin/Microsoft OpenTK package is a fork of a _very_ old version of OpenTK 1.x.  **The OpenTK team does not support the Xamarin code.**
- The OpenTK.NETCore package (now unlisted) is a fork of OpenTK 3.  .NET Core projects should use OpenTK 4+ instead.

### Which versions of OpenGL are supported?

OpenTK primarily supports **OpenGL 3+** and **OpenGL ES 2+** — in other words, *modern* OpenGL.  The fixed-function pipeline (FFP) — a.k.a. “classic OpenGL” — can be made to work, but it is not directly supported.

### What about UI frameworks?

OpenTK can be used without a UI framework:  It is capable of spawning a window and taking input directly, which is often sufficient for many programs like games.

But if you need to integrate OpenTK with an existing UI, it includes some packages to do so:

- **OpenTK 4.x** has —
  - A WinForms-compatible [GLControl](https://github.com/opentk/GLControl) Nuget package.
  - A WPF-compatible [GLWpfControl](https://github.com/opentk/GLWpfControl) Nuget package.

- **OpenTK 3.x** contains its own WinForms-compatible `GLControl` component.

### How fast is OpenTK?

For a .NET library, OpenTK is *very* fast.  OpenTK 3 and 4 use hand-optimized IL assembly to minimize overhead when calling OpenGL functions, and OpenTK 5 is planned to use the new C# function pointers to accomplish the same thing.

We take great pains to make it as efficient as possible.  However, keep in mind that the underlying runtimes (.NET/Mono) use both JIT compilation and garbage collection, which can complicate the definition of “fast.”  Still, we place significant emphasis on performance, so if you believe something could run faster, please [open an issue on our GitHub](https://github.com/opentk/opentk/issues)!

### Does OpenTK support...

#### Vulkan or Metal?

OpenTK does not currently support [Vulkan](https://www.vulkan.org/) or [Metal](https://developer.apple.com/metal/).  OpenTK 5 may not either, but the changes for OpenTK 5 may make it easier to support Vulkan in the future.  Feel free to submit a pull request and [help out](#How-can-I-help)!

#### DirectX or Direct3D?

OpenTK is focused on cross-platform, portable, open standards.  There are other libraries that support DirectX if you want to exclusively target Microsoft platforms.

#### OpenVG? OpenVX? OpenXR?

OpenTK does not currently support the Khronos group’s other open standards.  Feel free to submit a pull request and [help out](#How-can-I-help)!

## Installing and Using OpenTK

### How do I get OpenTK?

OpenTK is distributed as a Nuget package.  The release [Nuget package](https://www.nuget.org/packages/OpenTK/) can be found on [nuget.org](https://nuget.org), and it can be installed via Nuget using your IDE, or via the `nuget` command-line tool.  

The source code is in [our GitHub repository](https://github.com/opentk/opentk).

In Visual Studio, installing OpenTK is as simple as installing any other Nuget package:

- Right-click on your C# project in the Solution Explorer.
- Choose “Manage Nuget packages.”
- Type `opentk` into the Search box and press Enter.
- Click on “**OpenTK** by Team OpenTK.”
- Click on the “Install” button.

### Which `using` directives do I need?

OpenTK works just like the underlying OpenGL/CL/AL calls, so if you know how to program one of those libraries, you already know most of what you’ll need to know.

First, add a `using` directive for the underlying subsystem you want to use.  For example, you probably want to use one or more of these:

- `using OpenTK.Graphics.OpenGL;`
- `using OpenTK.Graphics.OpenGL4;`
- `using OpenTK.Graphics.ES30;`
- `using OpenTK.Audio.OpenAL;`
- `using OpenTK.Compute.OpenCL;`

Which ones to use depends on which flavors of OpenGL/CL/AL you’re targeting.  We generally recommend with OpenGL that you should use `OpenGL4` unless you need the legacy fixed-function pipeline (FFP); the `OpenGL4` namespace works for all OpenGL versions, but it excludes the FFP functions and their overhead.  (In OpenTK 5, we plan to rename these namespaces to make this distinction more obvious.)

### How do I create a window using OpenTK?

There are a lot of older, outdated explanations for this across the Internet.  For OpenTK 4+, you’ll want to inherit a class from `NativeWindow` or `GameWindow`, instantiate your custom window, and then poll for events.  Here’s a simple, complete, working example `Program.cs` that uses `GameWindow`:

```c#
using OpenTK.Mathematics;
using OpenTK.Windowing.Desktop;

namespace MyOpenTKExample
{
    public static class Program
    {
        public static void Main()
        {
            var nativeWindowSettings = new NativeWindowSettings
            {
                Size = new Vector2i(800, 600),
                Title = "My OpenTK Example Program"
            };
            
            using (var window = new MyExampleWindow(GameWindowSettings.Default,
                                                    nativeWindowSettings))
            {
                window.Run();
            }
        }
    }
    
    public class MyExampleWindow : GameWindow
    {
        public MyExampleWindow(GameWindowSettings gameWindowSettings,
                              NativeWindowSettings nativeWindowSettings)
            : base(gameWindowSettings, nativeWindowSettings)
        {
        }
        
        protected override void OnUpdateFrame(FrameEventArgs e)
        {
			// This gets called every 1/60 of a second.
            if (KeyboardState.IsKeyDown(Keys.Escape))
                Close();
            
            base.OnUpdateFrame(e);
        }
        
        protected override void OnRenderFrame(FrameEventArgs e)
        {
            // Show that we can use OpenGL: Clear the window to cornflower blue.
            GL.ClearColor(0.39f, 0.58f, 0.93f, 1.0f);
            GL.Clear(ClearBufferMask.ColorBufferBit);
        }
    }
}
```

More sophisticated uses will load textures and shaders and models during the `OnLoad()` handler, clean up during the `OnUnload()` handler, and do a lot more than just wait for the user to press `Escape` to close the window — but the code above is enough to get you started.

(Creating a window in OpenTK 3 requires a similar amount of code, but it looks a bit different from this example.  New projects should use OpenTK 4+.)

### How do I call OpenGL/CL/AL functions?

In your code, you can invoke the OpenGL/CL/AL functions directly using the static classes that OpenTK exposes.  OpenTK provides all of the same functions that the underlying native libraries provide, just with the initial `gl*` or `cl*` or `al*` prefix turned into a static class name.  So, for example:

- In C you’d call `int error = glGetError();`.  In C# you’d write `ErrorCode error = GL.GetError();`.
- In C you’d call `glBindVertexArray(handle);`.  In C# you’d write `GL.BindVertexArray(handle);`.
- In C you’d write `glCreateShader(GL_FRAGMENT_SHADER);`.  In C# you’d write `GL.CreateShader(ShaderType.FragmentShader)`.

As you can see, the programming models are nearly identical, by design:  If you know how to use OpenGL/OpenCL/OpenAL, you know how to use OpenTK.

**Arrays and pointers**

Passing arrays from .NET to OpenGL/CL/AL (or to any C library) is a complex topic, but OpenTK has built-in support for it:  See the section below on “[How do I pass pointers and arrays?](#How-do-I-pass-pointers-and-arrays)”.

**Constant symbols**

Note in the third example that the constants change syntax slightly:  They aren’t global `#define` directives as in C, but instead have some minor alterations to make them more .NET-friendly:

- The constants are grouped into strong `enum` types like `ErrorCode` and `ShaderType` — they’re not just a big pile of unrelated values like they are in C.
- The names are changed from `SHOUT_CASE` to `PascalCase` to match C# style conventions.
- The constants' `GL_` prefixes have been removed for readability.

There is also a special enum type, `All`, that contains *all* of the GL constants; this can simplify porting code from other languages, and it ensures that all of the OpenGL constants are available.  `All` should be avoided in favor of more specialized enum types where possible.

### Where do I start with OpenGL?

Check out [our tutorial here](https://opentk.net/learn/index.html), which will teach you how to build a basic working program that can draw simple objects.  You can learn more about OpenGL in depth over at [Learn OpenGL](https://learnopengl.com/); our tutorials are based on theirs.

### What’s the difference between OpenGL and OpenGL ES?

OpenGL ES is “OpenGL for Embedded Systems.”  It’s a subset of OpenGL, designed for low-power and mobile devices.  It has many similarities with modern OpenGL, especially OpenGL ES 2.0 and later, but it typically strips out lesser-used functionality.  However, OpenGL ES is not a _proper_ subset of OpenGL, so an exact comparison between them is difficult.

But typically, if you’re coding for desktop PCs or Macs, you’ll want to use OpenGL 4.x; while if you’re targeting a mobile phone or a gaming console, you’ll want to use the newest version of OpenGL ES that’s supported on it.

### How do I load textures?

That’s mostly outside the scope of this FAQ, but there are a variety of techniques for loading images from files in .NET, and once the image is loaded into main memory, you then use one of the various GL texture functions to load it into the GPU, like `GL.TexImage2D()`.  For more details, read through the section on [loading and creating textures](https://opentk.net/learn/chapter1/5-textures.html) in our tutorial, derived from the [tutorial on textures](https://learnopengl.com/Getting-started/Textures) at [Learn OpenGL](https://learnopengl.com).

### How do I create shaders?

That’s outside the scope of this FAQ, but there’s a [tutorial on shaders and GLSL](https://opentk.net/learn/chapter1/4-shaders.html) as part of our lessons here, derived from the [tutorial on shaders and GLSL](https://learnopengl.com/Getting-started/Shaders) at [Learn OpenGL](https://learnopengl.com).

### How do I handle errors?

The same way as in OpenGL/CL/AL!  OpenTK exposes the same `glError()` functions, so you can test for errors in C# the same way you would in C.  Here’s a simple example:

*Example code in C:*

```c
int shader = glCreateShader(GL_FRAGMENT_SHADER);

int errorCode = glGetError();
if (errorCode) {
    printf("Uh oh.");
    exit(-1);
}
```

*C# equivalent:*

```c#
int shader = GL.CreateShader(ShaderType.FragmentShader);

ErrorCode errorCode = GL.GetError();
if (errorCode != ErrorCode.NoError)
{
    throw new UhOhException(...);
}
```

Arguably, throwing exceptions is a better way to handle errors than aborting.

`Gl.GetError()` in OpenTK reads errors from a queue, just like `glGetError()` does — it’s just a passthrough to the same underlying error state.

#### That seems like a lot of if-statements; is there a better way?

OpenGL 4.3+ offers a new technique, called a _debug message callback_.  (Many older OpenGL drivers also support this via the `KHR_debug` extension.)  The general idea behind this is to “register” a _debug callback_ function with OpenGL; any time OpenGL encounters an error, it will call your function immediately, and pass useful things to it like an error message.  First, you’ll need to declare an error-handling method that OpenGL can call, something like this:

```c#
private static void DebugCallback(DebugSource source, DebugType type, int id,
    DebugSeverity severity, int length, IntPtr message, IntPtr userParam)
{
    string messageString = Marshal.PtrToStringAnsi(message, length);
    Console.WriteLine($"{severity} {type} | {messageString}");

    if (type == DebugType.DebugTypeError)
        throw new Exception(messageString);
}
```

Next, you’ll need to create a delegate from it as a handle that the GC can’t collect, and then register the resulting function with OpenGL:

```c#
private static DebugProc _debugProcCallback = DebugCallback;
private static GCHandle _debugProcCallbackHandle;

...
    
_debugProcCallbackHandle = GCHandle.Alloc(_debugProcCallback);

GL.DebugMessageCallback(_debugProcCallback, IntPtr.Zero);
GL.Enable(EnableCap.DebugOutput);
GL.Enable(EnableCap.DebugOutputSynchronous);
```

Vassalware, a member of the OpenTK Team, has a [more extensive discussion of this technique](https://gist.github.com/Vassalware/d47ff5e60580caf2cbbf0f31aa20af5d).

Note that to use this technique, you may need to change your `NativeWindowSettings` to update the API version of OpenGL to 4.3 or higher; or you may need to check if your drivers support the `KHR_debug` extension.

### How do I pass pointers and arrays?

OpenTK includes several method overloads for every OpenGL/CL/AL function that requires a pointer:  These overloads are designed to make working with the functions as natural as possible in .NET while still providing high performance.

So, for example, consider [`glBufferData()`](https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBufferData.xhtml).  This OpenGL function takes four parameters:

```c
void glBufferData(GLenum target, GLsizeiptr size, const void *data, GLenum usage);
```

The C form of this requires you to pass an array of `data` that could be composed from a variety of different data structures; so how can we invoke this function in .NET?

OpenTK exposes this single function with a dozen different overloads on the `GL` class!

```C#
1. void BufferData<T>(BufferTarget target, int size, T[] data, BufferUsageHint usage)
2. void BufferData<T>(BufferTarget target, int size, T[,] data, BufferUsageHint usage)
3. void BufferData<T>(BufferTarget target, int size, T[,,] data, BufferUsageHint usage)
4. void BufferData<T>(BufferTarget target, int size, ref T data, BufferUsageHint usage)
5. void BufferData   (BufferTarget target, int size, IntPtr data, BufferUsageHint usage)

6. void BufferData<T>(BufferTarget target, IntPtr sz, T[] data, BufferUsageHint usage)
7. void BufferData<T>(BufferTarget target, IntPtr sz, T[,] data, BufferUsageHint usage)
8. void BufferData<T>(BufferTarget target, IntPtr sz, T[,,] data, BufferUsageHint usage)
9. void BufferData<T>(BufferTarget target, IntPtr sz, ref T data, BufferUsageHint usage)
10. void BufferData  (BufferTarget target, IntPtr sz, IntPtr data, BufferUsageHint usage)
```

(Each of the generic forms has a `where T: struct` constraint, but it’s been omitted for brevity.)

Overloads #1-5 differ from #6-10 only by using `IntPtr` instead of `int` to represent the size of the data, so let’s look at what techniques overloads #1-5 support:

- You can pass managed arrays of `struct` objects via the `T[]`, `T[,]` and `T[,,]` forms:  Forms #1-3 support one-dimensional, two-dimensional, and three-dimensional arrays of managed `struct`s, respectively.
- You can pass a `ref` to a single `struct` using form #4.
- You can pass a raw pointer from an `unsafe`/`fixed` block of memory via the `IntPtr` form (#5).

(And in OpenTK 5, we plan to support `ReadOnlySpan<T>` as well, adding yet another way to do it!)

**Passing managed arrays** (forms #1-3)

The first three overloads let you pass arrays of any `struct` type, so that every major atomic type is supported — `int`, `long`, `short`, `byte`, `float`, `double`, custom structs like a `Color` or `DateTime`, and more.  This means that simple, straightforward OpenGL code is very similar between C and C#, as in the example below.

*Example code in C:*

```C
unsigned char *data = (unsigned char *)malloc(bufferSize);
...
glBufferData(GL_TEXTURE_BUFFER, bufferSize, data, GL_STATIC_DRAW);
```

*C# equivalent:*

```c#
byte[] data = new byte[bufferSize];
...
GL.BufferData(BufferTarget.TextureBuffer, bufferSize, data, BufferUsageHint.StaticDraw);
```

**Passing refs** (form #4)

The fourth overload takes a `ref` to a `struct` type, which you can use to pass simple structs directly.  Here’s an example where both C and C# allocate a struct on the stack and then pass it to OpenGL:

*Example code in C:*

```C
MyStruct data;
...
glBufferData(GL_TEXTURE_BUFFER, bufferSize, data, GL_STATIC_DRAW);
```

*C# equivalent:*

```c#
MyStruct data;
...
GL.BufferData(BufferTarget.TextureBuffer, bufferSize, data, BufferUsageHint.StaticDraw);
```

**Passing raw pointers** (form #6)

OpenTK also lets you pass raw pointers to unmanaged or pinned memory directly, if you need maximum power and speed.  The two forms below are equivalent:

*Example code in C:*

```C
unsigned char *data = (unsigned char *)malloc(bufferSize);
...
glBufferData(GL_TEXTURE_BUFFER, bufferSize, data, GL_STATIC_DRAW);
```

*C# equivalent:*

```c#
byte[] data = new byte[bufferSize];
...
unsafe
{
    fixed (byte* ptr = data)
    {
        GL.BufferData(BufferTarget.TextureBuffer, bufferSize, (IntPtr)ptr,
                      BufferUsageHint.StaticDraw);
    }
}
```

On OpenTK 4 and earlier, this is also how you pass a `ReadOnlySpan<T>` to OpenTK; you need to pin the data yourself with the `fixed` keyword and then pass it as a pointer, as in the example below:

```C#
ReadOnlySpan<float> data = stackalloc float[1024];
...
unsafe
{
    fixed (float* ptr = data)
    {
        GL.BufferData(BufferTarget.TextureBuffer, bufferSize, (IntPtr)ptr,
                      BufferUsageHint.StaticDraw);        
    }
}
```

### How do I control OpenGL contexts?

On many OSes, there is the notion of an *OpenGL context*, a container that holds all of the allocated OpenGL resources like textures and shaders and buffers.  There is also typically the concept of a *current* context — there’s usually only one context “active” at a time, and you have to switch between them if you have more than one.

OpenTK exposes this through `NativeWindow` in the form of its `Context` property and its `MakeCurrent()` method.  `Context` is an `IGraphicsContext`, and includes low-level OpenGL operations like `MakeCurrent()` and `MakeNoneCurrent()` and `SwapBuffers()`.

The graphics context is created automatically for you when a `NativeWindow` is created, so the only thing you may need to do is occasionally invoke `MakeCurrent()` or `MakeNoneCurrent()`.

(If you only have one OpenGL context, you can ignore all of this.)

### Is OpenTK thread-safe?

Yes, but OpenGL/CL/AL often aren’t!

As a general rule, you should only ever invoke OpenGL/CL/AL from a single thread, because they often aren’t thread-safe or have serious threading limitations, depending on your underlying OS or implementation.  OpenTK doesn’t change the OS’s behavior:  It just provides access to whatever the OS already does.

A more complete answer is far outside the scope of this FAQ.

## Beginner Questions

### How do I render my first triangle?

The same way you would with OpenGL in C!  The semantics are the same — only the syntax is different, since you’ll likely be doing it in C#, not C.

Rendering your first triangle can be challenging just because there are many new ideas and new concepts to learn.  It’ll take some code, and some learning, and some patience, but you can do it!

We have some [introductory tutorials](https://opentk.net/learn/index.html) that should help you get your first project off the ground.  There are a few major parts to drawing your first triangle:

- You’ll need to first [create a window](https://opentk.net/learn/chapter1/1-creating-a-window.html) (there’s a [quick example above](#How-do-I-create-a-window-using-OpenTK)).
- You’ll need to fill some [buffers](#How-do-shadersbuffersvertex-arraysetc-work) with the data for your triangle — you’ll need at least a VBO and vertex attributes.
- You’ll need to create and load some [shaders](https://opentk.net/learn/chapter1/4-shaders.html) — at least a simple vertex shader and a simple fragment shader.
- Finally, in your window’s render phase, you’ll need to tell OpenGL to [use your shaders to draw the triangle data in your buffers](https://opentk.net/learn/chapter1/2-hello-triangle.html).

### How do I draw things more complex than a single triangle?

*Lots* of triangles!

OpenGL has always supported things like triangle strips and triangle fans (and their modern equivalent, the [EBO](#How-do-shadersbuffersvertex-arraysetc-work)) to make it easy to render “more complex polygons”, but even the most complex graphical scene is really broken down into just lots and lots of triangles.  A rectangle, for example, is just two triangles that share an edge.  A pentagon can be made from three triangles, and a hexagon can be made from four.  A cube is made up of six rectangles — or twelve triangles.

So once you can render a single triangle, you just use the same techniques to render *more* of them, with different colors, textures, and positions, to make anything you can imagine.

### How do shaders/buffers/vertex arrays/etc. work?

This is a common question when you’re new to modern OpenGL — there’s a lot of new terminology, and it’s very confusing!  We recommend reading some of the [introductory articles at Learn OpenGL](https://learnopengl.com/Getting-started/Hello-Triangle) (and some of our similar [articles converted to C#](https://opentk.net/learn/chapter1/2-hello-triangle.html)) to understand what all these strange concepts are and how they fit together.

But the short short *short* explanations for the mort important terms are:

- *Triangles* are what your GPU actually knows how to draw.
  
- *Textures* are the parts of the GPU’s memory that you upload your image data into.
  
- *Buffers* are parts of the GPU’s memory that you upload your triangle data into.
  
  - *Vertex buffers* (VBOs) store information about the corners (vertices) of your triangles, like where they are in space, and what parts of a texture to use.
  - *Vertex attributes* describe how the vertex buffer is laid out:  They assign meaning to each value in the vertex buffer.
    - Vertex attributes and VBOs together describe an “array of structs” containing all of your data.  The VBO is the “array” part, and the attributes are the definition of each “struct.”
  - *Vertex array objects* (VAOs) store vertex attribute data in the GPU’s memory, so that you don’t have to send it to the GPU for every triangle.
  - *Element buffers* (EBOs) describe which vertices should be used to make each triangle, and let you reuse vertices between multiple triangles.
  
- *Shaders* are little programs that tell your GPU how to draw things like triangles.
  
  - *Vertex shaders* calculate where the corners of the triangles are, using the vertex data.
  - *Fragment shaders* calculate the color of the pixels inside the triangles, using the coordinate data from the vertex shader, and image data from various textures.
  - *Compute shaders* are specialty programs that use the GPU for doing general-purpose math that the CPU would normally do (*only much slower!*).
  
  

### How do I use OpenTK with C++?

OpenTK is an OpenGL/CL/AL library for *managed* .NET languages like C# and F#.  You can use OpenTK with managed C++ (i.e., C++/CLI), but you neither need it nor likely want it for plain C++.

### How do I use the fixed-function pipeline (FFP)?

Modern OpenGL doesn’t use things like `glBegin()` and `glDrawTriangle()` — even though those kinds of functions were easy to use, they didn’t scale well.  Modern OpenGL uses shaders and buffers instead, which can handle much more complex rendering than the older OpenGL fixed-function pipeline did.

If you’re trying to use the old FFP functions, they’re *exposed* by OpenTK, but they won’t work out-of-the-box, because you need to turn on *compatibility mode* to enable them.

When creating a `NativeWindow`, you pass a `NativeWindowSettings` object to describe what kind of OpenGL context you need.  To enable *compatibility mode*, you change the `ContextProfile` of the `NativeWindowSettings`:

```c#
var settings = new NativeWindowSettings
{
    ContextProfile = ContextProfile.Compatibility
};

using (var window = new NativeWindow(settings))
{
    window.Run();
}
```

This will provide access to the older FFP functions, at a small cost in memory overhead and performance. (But if you’re concerned about performance, you shouldn’t be using the FFP anyway!)

### How do I center my window on the screen?

OpenTK 4.4 adds support for centering a `NativeWindow` or `GameWindow` directly:

```c#
myWindow.CenterWindow();
```

You can also center-and-resize the window as a single operation, which is a common need when an application starts:

```c#
myWindow.CenterWindow(new Vector2i(800, 600));
```

This overload avoids flicker if you need to both move *and* resize the window.

For OpenTK 4.x prior to OpenTK 4.4, you need to query the window’s `Monitor` information and reposition the window manually, as described [here](https://github.com/opentk/opentk/issues/1213#issuecomment-742068414).  For OpenTK 3.x, you will need to manually position the window relative to its given `DisplayDevice`.

### How can I control the initial appearance of my window?

The `NativeWindowSettings` class, an instance of which you must pass to your window’s constructor, contains many properties for controlling how your window initially appears (or doesn’t appear).  Here are the major properties you might want to use to control how your window looks and behaves:

```c#
public class NativeWindowSettings
{
    public string Title { get; set; }              // Text for the title bar
    public WindowIcon Icon { get; set; }           // Array of images of various sizes
    
    public bool StartFocused { get; set; }         // True by default
    public bool StartVisible { get; set; }         // True by default
    
    public Vector2i? Location { get; set; }        // Top-left corner position
    public Vector2i? Size { get; set; }            // Client/content size, in pixels
    
    public WindowBorder WindowBorder { get; set; } // Fixed, Resizable, Hidden (no bord.)
    public WindowState WindowState { get; set; }   // Normal, Minim., Maxim., Hidden
    public bool IsFullscreen { get; set; }         // Like Maximized, only bigger :-)
}
```

A number of these, like `Location` and `Size` and `WindowState` and `IsFullscreen`, are re-exposed after the window is constructed as properties on the window itself, so you can use the equivalent window properties to update your window’s appearance and state as well.

If you want to delay your window’s position/layout until after it has been constructed and important data for it has been loaded (like its position), a common pattern is something like this:

```c#
var nativeSettings = new NativeWindowSettings { StartVisible = false };
using (var myWindow = new MyWindow(nativeSettings, gameSettings))
{
    myWindow.Run();
}

...
    
public class MyWindow : GameWindow
{
    ...
        
    public override OnLoad()
    {
        // Do setup...
        ...
            
        // Now show it.
        CenterWindow();
        WindowState = WindowState.Normal;
    }
}
```

(Note that OpenTK 3 used a very different technique for setting up its initial window state:  Window parameters are passed as part of the `GameWindow` or `NativeWindow` constructor; there is no such thing as `NativeWindowSettings`; and configuration options are fairly limited.)

### What’s up with OpenTK’s matrices?

For historical reasons, OpenTK’s built-in `Matrix` structs store their data in row-major order, not column-major order.  This provides consistent backward-compatibility with existing software that uses OpenTK, but it can be confusing, for two reasons:

- First, if you copy other code found online for OpenGL, you may need to *transpose* the matrices their code uses to get the expected behavior.
- Second, matrix-matrix multiplications typically need to be done in _reverse_ order of those of most other libraries.

This is a common source of confusion when using OpenTK, since it often prevents “code found on the Internet” from working directly.  We’re sorry if it causes you trouble, but it’s easy to work around it, and because so much existing software depends on it, it’s not something we plan to change any time soon.

### What’s the difference between NativeWindow and GameWindow?

OpenTK includes two classes, `NativeWindow` and `GameWindow`, for opening and displaying OpenGL windows.  What’s the difference between these two classes?

- `NativeWindow` is the lowest-level class.  It includes the bare minimum of required mechanics to open and display a window, and not much else.  It’s small, light, and fast, and good for situations where you don’t need an update/render loop or you intend to write most of the update/render logic yourself.  (Note that `NativeWindow` is not the lowest *level*, merely the lowest level in managed .NET:  `NativeWindow` wraps native functionality provided by GLFW.)
- `GameWindow` is a higher-level class that inherits from `NativeWindow` and adds functionality often needed by video games.  It provides timing logic, separated update and render methods, and a variety of ways to synchronize and control the game’s performance.

If you’re making a game, you probably want to use `GameWindow` as your base class.  Otherwise, you probably want to use `NativeWindow`.

### What’s the relationship between OpenTK and GLFW?

OpenTK 4.x+ uses the [GLFW](https://www.glfw.org/) C++ library to provide cross-platform backend support:  GLFW is responsible for opening windows, handling input, and handling as much as possible of the OS dependencies in a portable way.  There’s no reason for us to implement a portability layer when a good, well-supported one already exists.

As an example, consider the `NativeWindow.Title` property, which lets you read and update the window’s title.  Here’s its entire implementation, verbatim:

```c#
public string Title
{
    get => _title;
    set
    {
        unsafe
        {
            GLFW.SetWindowTitle(WindowPtr, value);
            _title = value;
        }
    }
}
```

As you can see, the `set` accessor does little more than ask GLFW to update the window title with the new string.  GLFW is responsible for actually invoking `SetWindowTextW()` on Windows, or `XSetWindowTitle()` on Linux, or `[window setTitle:title]` on MacOS X — and for dealing with all of the marshalling and proxying and allocation and character-set conversions and quirks and compatibility issues.

The OpenTK Nuget package includes a copy of a compiled build of GLFW inside it.  It’s worth knowing that it’s there and that it provides a lot of the low-level magic, but most of the time, you can safely ignore that GLFW exists, and you can work exclusively with OpenTK’s managed .NET classes and interfaces.

## Help & Support

### Where can I get help?

We recommend you visit our [Discord server](https://discord.gg/6HqD48s) and ask a question in the **`#support` channel**.  Our server is highly-active, so if you’re stuck, someone will be happy to help you.

### I think I found a bug; how do I report it?

Please read through the open tickets in the [OpenTK Issues on GitHub](https://github.com/opentk/opentk/issues), and see if any of them match your bug.  If not, please open a new ticket, and be sure to include all of the information requested in the template.

### Who maintains OpenTK?

**The OpenTK Team!**  The current maintainers are [@frederikja163](https://github.com/frederikja163), [@jvbsl](https://github.com/jvbsl), [@NogginBops](https://github.com/NogginBops), [@PJB3005](https://github.com/PJB3005), and [@varon](https://github.com/varon).  Previous maintainers include [@Nihlus](https://github.com/Nihlus) and [@Perksey](https://github.com/Perksey), and the original author of OpenTK was [@thefiddler](https://github.com/thefiddler).  And we have lots of other contributors, too, who have helped add features and fix bugs.

### How can I help?

Check the open tickets in the [OpenTK Issues on GitHub](https://github.com/opentk/opentk/issues) and see if one of them sounds interesting to work on.  You can also drop by our [Discord server](https://discord.gg/6HqD48s) and offer to help in the **`#development` channel**; we’d be happy for any contribution you’re willing to make!
