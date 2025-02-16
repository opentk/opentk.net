# Platform Abstraction Layer 2

> [!IMPORTANT]
> PAL2 is under development and many things might change in the future.
Feedback is wanted!

> [!IMPORTANT]
> Currently PAL2 is basically feature complete on Windows, and most parts work on x11 and macos. There is an slightly outdated SDL backend that works on both Windows and linux, and should in theory work on macos but the SDL binaries nuget package doesn't have macos binaries yet.

## Setup

PAL2 exposes platform APIs through a number of interfaces used to abstract away the underlying operating system.

Most of PAL2 is exposed through the `Toolkit` class. `Toolkit` contains a number of static properties that are the main interface for PAL2. `ToolkitOptions` contains a few configuration items should be set before calling `Toolkit.Init`.

```cs
using OpenTK.Platform;

ToolkitOptions options = new ToolkitOptions();

// ApplicationName is the name of the application
// this is used on some platforms to show the application name.
options.ApplicationName = "OpenTK tutorial";

Toolkit.Init(options);
```

## Logging

PAL2 has the ability to output diagnostic messages through an `ILogger` interface exposed as a `Logger` property of `ToolkitOptions`. This can be useful to find errors in the code and provide debug data that can be used to fix bugs in OpenTK.

OpenTK provides an implementation of this interface called `ConsoleLogger` that writes out all debug messages to the console.
```cs
ToolkitOptions options = new ToolkitOptions();

// ApplicationName is the name of the application
// this is used on some platforms to show the application name.
options.ApplicationName = "OpenTK tutorial";
// Set the logger to use
options.Logger = new ConsoleLogger();

Toolkit.Init(options);
```

## Creating a window

To create an OpenGL window using PAL2 we want to use the `Toolkit.Window` property to access the window functions in PAL2. `Toolkit.Window` contains a function called `Toolkit.Window.Create` which we will use to create a window, and `Toolkit.OpenGL.CreateFromWindow` can be used to create an OpenGL context from the created window.

```cs
OpenGLGraphicsApiHints contextSettings = new OpenGLGraphicsApiHints()
{
    // Here different options of the opengl context can be set.
    Version = new Version(4, 1),
    Profile = OpenGLProfile.Core,
    DebugFlag = true,
    DepthBits = ContextDepthBits.Depth24,
    StencilBits = ContextStencilBits.Stencil8,
};

WindowHandle window = Toolkit.Window.Create(contextSettings);

OpenGLContextHandle glContext = Toolkit.OpenGL.CreateFromWindow(window);
```

This returns a handle to the window and the opengl context which is used to refer to the window and opengl context in pal2 functions.

When a window handle is created it is not visible by default, so we need to show the window. We also need to set the size we want the window to be shown with. This can be done by setting the window size and mode like so:

```cs
// Set the title of the window
Toolkit.Window.SetTitle(window, "OpenTK window");
// Set the size of the window
Toolkit.Window.SetSize(window, 800, 600);
// Bring the window out of the default Hidden window mode
Toolkit.Window.SetMode(window, WindowMode.Normal);
```

Before we can do any calls to OpenGL we need to do two things. 

1. We need to set our created context as the "current" context and 
2. Load all of the functions that OpenGL is going to use.

This can quite easily be done like follows:

```cs
// The the current opengl context and load the bindings.
Toolkit.OpenGL.SetCurrentContext(glContext);
GLLoader.LoadBindings(Toolkit.OpenGL.GetBindingsContext(glContext));
```

[`Toolkit.OpenGL.SetCurrentContext`](xref:OpenTK.Platform.IOpenGLComponent.SetCurrentContext(OpenTK.Platform.OpenGLContextHandle)) sets the given OpenGL context as the "current" context.
<!--FIXME: Say something about what "current" context even means.-->
And [`GLLoader.LoadBindings`](xref:OpenTK.Graphics.GLLoader.LoadBindings(OpenTK.IBindingsContext)) loads all OpenGL functions.
<!--FIXME: Say something about lazy loading?-->

Now we want to set up our event loop so we can interact with our newly created window. Without any event processing most of the window functions will not happen and no window interaction will be possible.

A simple event loop might look like the following:
```cs
while (true)
{
    // This will process events for all windows and
    //  post those events to the event queue.
    Toolkit.Window.ProcessEvents(false);
    // Check if the window was destroyed after processing events.
    if (Toolkit.Window.IsWindowDestroyed(window))
    {
        break;
    }

    // Render something here
}
```

To render something to the window we can do the following:

```cs
GL.ClearColor(new Color4<Rgba>(64 / 255f, 0, 127 / 255f, 255));
GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit | ClearBufferMask.StencilBufferBit);

Toolkit.OpenGL.SwapBuffers(glContext);
```

The code shown so far will create a window and then process events while rendering to the window, but running the code there is one crucial flaw to the program. Closing the window doesn't work.

This is becase closing the window is an event that needs to be responeded to, and as we are not handling any events no action is taken when the user wants to quit the application. So lets fix that!

First step of handling events is to register a callback to the `EventQueue.EventRaised` event. Do this before creating the window like follows:

```cs
EventQueue.EventRaised += EventRaised;
```

Now we need to define the `EventRaised` function.

```cs
void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
{

}
```

> [!WARNING]
> The arguments of the event callback are likely to change.

This callback gets a few arguments, the one we are interested in right now is `args`.
`args` contains data specific to the type of event and we can pattern match on a specific args type to respond to a specific event.

To handle the quit message we will do the following:

```cs
void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
{
    if (args is CloseEventArgs closeArgs)
    {
        // Destroy the window that the user wanted to close.
        Toolkit.Window.Destroy(closeArgs.Window);
    }
}
```

We use pattern matching `is` on `args` to check if it is of type `CloseEventArgs`. `CloseEventArgs` contains a `window` property which tells us which window the user wanted to close.
So to close the window we call `Toolkit.Window.Destroy(WindowHandle)` to destroy the window.

This way of handling events generalizes to other event types. First check for the type of event, then cast the `EventArgs` to the type specific for this type of event which gives appropriate information for handling the event.

The final code we end up with could look like the following:

```cs
using OpenTK.Platform;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Mathematics;
using OpenTK.Core.Utility;
using System;

namespace Pal2Sample;

class Sample
{
    public static void Main(string[] args)
    {
        EventQueue.EventRaised += EventRaised;

        ToolkitOptions options = new ToolkitOptions();

        options.ApplicationName = "OpenTK tutorial";
        options.Logger = new ConsoleLogger();

        Toolkit.Init(options);

        OpenGLGraphicsApiHints contextSettings = new OpenGLGraphicsApiHints()
        {
            // Here different options of the opengl context can be set.
            Version = new Version(4, 1),
            Profile = OpenGLProfile.Core,
            DebugFlag = true,
            DepthBits = ContextDepthBits.Depth24,
            StencilBits = ContextStencilBits.Stencil8,
        };

        WindowHandle window = Toolkit.Window.Create(contextSettings);

        OpenGLContextHandle glContext = Toolkit.OpenGL.CreateFromWindow(window);

        // Show window
        Toolkit.Window.SetTitle(window, "OpenTK window");
        Toolkit.Window.SetSize(window, 800, 600);
        Toolkit.Window.SetMode(window, WindowMode.Normal);

        // The the current opengl context and load the bindings.
        Toolkit.OpenGL.SetCurrentContext(glContext);
        GLLoader.LoadBindings(Toolkit.OpenGL.GetBindingsContext(glContext));

        while (true)
        {
            // This will process events for all windows and
            // post those events to the event queue.
            Toolkit.Window.ProcessEvents(false);
            if (Toolkit.Window.IsWindowDestroyed(window))
            {
                break;
            }

            GL.ClearColor(new Color4<Rgba>(64 / 255f, 0, 127 / 255f, 255));
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit | ClearBufferMask.StencilBufferBit);

            Toolkit.OpenGL.SwapBuffers(glContext);
        }
    }

    static void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
    {
        if (args is CloseEventArgs closeArgs)
        {
            // Destroy the window that the user wanted to close.
            Toolkit.Window.Destroy(closeArgs.Window);
        }
    }
}

```

## What's next?

Now that you have a basic program working you can look more into the different components of PAL2 here: [Components](Components.md) or alternatively read more about the different evens that PAL2 supports: [Event Handling](Event-Handling.md).