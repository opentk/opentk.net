# Platform Abstraction Layer 2

> [!IMPORTANT]
> PAL2 is under early development and many things might change in the future.
Feedback is encouraged.

> [!IMPORTANT]
> Currently PAL2 is basically only usable on windows. 
This will change as it is developed more.

## Setup

PAL2 exposes platform APIs through a number of interfaces used to abstract away the underlying operating system.

The first of these interfaces we are going to see is `IWindowComponent` and `IOpenGLComponent` which allows the user to 
create and interact with windows as well as initialize an OpenGL context and draw graphics to the window.

> [!NOTE]
> Currently there is no way for OpenTK to automatically initialize these interfaces to the correct platform implementations, this capability will be available in the future.

The following code initializes these interfaces with a windows implementation.
```cs
IWindowComponent windowComp = new OpenTK.Platform.Native.Windows.WindowComponent();
IOpenGLComp openglComp = new OpenTK.Platform.Native.Windows.OpenGLComponent();

windowComp.Initialize(PalComponents.Window);
openglComp.Initialize(PalComponents.OpenGL);
```

## Creating a window

To create an OpenGL window using PAL2 all you need to do is the following:

```cs
OpenGLGraphicsApiHints contextSettings = new OpenGLGraphicsApiHints()
{
    DoubleBuffer = true,
    sRGBFramebuffer = false,
    Multisamples = 0,
    DepthBits = ContextDepthBits.Depth24,
    StencilBits = ContextStencilBits.Stencil8,
};

WindowHandle window = windowComp.Create(contextSettings);

OpenGLContextHandle glContext = openglComp.CreateFromWindow(window);
```

This returns a handle to the window and the opengl context which is the main method to interact with the window.

To show the created window we need to set the window mode to not be hidden.

```cs
windowComp.SetMode(window, WindowMode.Normal);
```