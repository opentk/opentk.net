# Platform Abstraction Layer 2

> [!IMPORTANT]
> PAL2 is under early development and many things might change in the future.
Feedback is wanted!

> [!IMPORTANT]
> Currently PAL2 is basically feature complete on Windows, and many parts work on x11 and macos. There is an SDL backend that works on both Windows and linux, and should in theory work on macos but the SDL binaries nuget package doesn't have macos binaries yet.

## Setup

PAL2 exposes platform APIs through a number of interfaces used to abstract away the underlying operating system.

The first of these interfaces we are going to see is `IWindowComponent` and `IOpenGLComponent` which allows the user to 
create and interact with windows as well as initialize an OpenGL context and draw graphics to the window.

The following code initializes these interfaces with a platform dependent implementation (or throws `NotSupportedException` for not yet supported OSes).
```cs
using OpenTK.Platform.Native;

IWindowComponent windowComp = new PlatformComponents.CreateWindowComponent();
IOpenGLComponent openglComp = new PlatformComponents.CreateOpenGLComponent();

windowComp.Initialize(PalComponents.Window);
openglComp.Initialize(PalComponents.OpenGL);
```

## Creating a window

To create an OpenGL window using PAL2 all you need to do is the following:

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

WindowHandle window = windowComp.Create(contextSettings);

OpenGLContextHandle glContext = openglComp.CreateFromWindow(window);
```

This returns a handle to the window and the opengl context which is the main method to interact with the window.

When a window handle is created it is not visible by default, so we need to show the window. We also need to set the size we want the window to be shown with. This can be done by setting the window size and mode like so:

```cs
// Set the size of the window
windowComp.SetSize(window, 800, 600);
// Bring the window out of the default Hidden window mode
windowComp.SetMode(window, WindowMode.Normal);
```

Before we can do any calls to OpenGL we need to do two things. 1. We need to set our created context as the "current" context and 2. Load all of the functions that OpenGL is going to use.
This can quite easily be done like follows:

```cs
// The the current opengl context and load the bindings.
openglComp.SetCurrentContext(glContext);
GLLoader.LoadBindings(openglComp.GetBindingsContext(glContext));
```

[`IOpenGLComponent.SetCurrentContext`](xref:OpenTK.Core.Platform.IOpenGLComponent.SetCurrentContext(OpenTK.Core.Platform.OpenGLContextHandle)) sets the given OpenGL context as the "current" context.
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
    windowComp.ProcessEvents();
    // Check if the window was destroyed after processing events.
    if (windowComp.IsWindowDestroyed(window))
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

openglComp.SwapBuffers(glContext);
```

The code shown so far will create a window and then process events while rendering to the window, but running the code there is one crucial flaw to the program. Closing the window doesn't work.

This is becase closing the window is an event that needs to be responeded to and as we are not handling any events, no action is taken when the user wants to quit the application. So lets fix that!

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

This callback gets a few arguments, the two we are interested in right now are `type` and `args`.
`type` tells us the type of the argument we received and `args` contains data specific to the type of event.

To handle the quit message we will do the following:

```cs
void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
{
    if (args is CloseEventArgs closeArgs)
    {
        // Destroy the window that the user wanted to close.
        windowComp.Destroy(closeArgs.Window);
    }
}
```

We use pattern matching `is` on `args` to check if it is of type `CloseEventArgs`. `CloseEventArgs` contains a `window` property which tells us which window the user wanted to close.
So to close the window we call `IWindowComponent.Destroy(WindowHandle)` to destroy the window.

This way of handling events generalizes to other event types. First check for the type of event, then cast the `EventArgs` to the type specific for this type of event which gives appropriate information for handling the event.

The final code we end up with could look like the following:

```cs
using OpenTK.Core.Platform;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Mathematics;
using OpenTK.Platform.Native;

namespace Pal2Sample;

class Sample
{
    static IWindowComponent windowComp = new PlatformComponents.CreateWindowComponent();
    static IOpenGLComponent openglComp = new PlatformComponents.CreateOpenGLComponent();

    static WindowHandle Window;
    static OpenGLContextHandle GLContext;

    public static void Main(string[] args)
    {
        windowComp.Initialize(PalComponents.Window);
        openglComp.Initialize(PalComponents.OpenGL);

        EventQueue.EventRaised += EventRaised;

        OpenGLGraphicsApiHints contextSettings = new OpenGLGraphicsApiHints()
        {
            // Here different options of the opengl context can be set.
            Version = new Version(4, 1),
            Profile = OpenGLProfile.Core,
            DebugFlag = true,
            DepthBits = ContextDepthBits.Depth24,
            StencilBits = ContextStencilBits.Stencil8,
        };

        Window = windowComp.Create(contextSettings);

        GLContext = openglComp.CreateFromWindow(Window);

        // Show window
        windowComp.SetSize(Window, 800, 600);
        windowComp.SetMode(Window, WindowMode.Normal);

        // The the current opengl context and load the bindings.
        openglComp.SetCurrentContext(GLContext);
        GLLoader.LoadBindings(openglComp.GetBindingsContext(GLContext));

        while (windowComp.IsWindowDestroyed(Window) == false)
        {
            // This will process events for all windows and
            //  post those events to the event queue.
            windowComp.ProcessEvents();

            GL.ClearColor(new Color4<Rgba>(64 / 255f, 0, 127 / 255f, 255));
            GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit | ClearBufferMask.StencilBufferBit);

            openglComp.SwapBuffers(GLContext);
        }
    }

    static void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
    {
        if (args is CloseEventArgs closeArgs)
        {
            // Destroy the window that the user wanted to close.
            windowComp.Destroy(closeArgs.Window);
        }
    }
}
```

## What's next?

Now that you have a basic program working you can look more into the different components of PAL2 here: [Components](Components.md) or alternatively read more about the different evens that PAL2 supports: [Event Handling](Event-Handling.md).