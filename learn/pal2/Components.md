# Components

PAL2 is build on top of a set of components that are responsible for their own part of the platform API. The idea is to allow partial implementations of platforms to easily be created and allow support for new platforms to be added in steps. It also groups related APIs into a specific namespace.

There are currently 10 components in PAL2.

|Component|Description|
|---------|-----------|
|[`IWindowComponent`](xref:OpenTK.Core.Platform.IWindowComponent)|This component is responsible for creating, destroying and managing windows. It is also responsible for processing events.|
|[`IOpenGLComponent`](xref:OpenTK.Core.Platform.IOpenGLComponent)|This components is responsible for creating and managing OpenGL contexts. `SwapBuffers()` lives here.|
|[`IDisplayComponent`](xref:OpenTK.Core.Platform.IDisplayComponent)|This component contains functions for retrieving information about displays (aka, monitors or screens) connected to the system. Allows for enumeration of video modes etc. |
|[`ICursorComponent`](xref:OpenTK.Core.Platform.ICursorComponent)|This component is responsible for loading and managing cursor images that can be set as the mouse cursor.|
|[`IIconComponent`](xref:OpenTK.Core.Platform.IIconComponent)|This component is responsible for loading and managing icon images that can be used as window icons.|
|[`IMouseComponent`](xref:OpenTK.Core.Platform.IMouseComponent)|This component allows for getting and setting of the mouse cursor position.|
|[`IKeyboardComponent`](xref:OpenTK.Core.Platform.IKeyboardComponent)|This component is responsible for translating platform dependent keycodes and scancodes to a cross platform representation. This component also exposes information about keyboard layout and has functions for IME input.|
|[`IJoystickComponent`](xref:OpenTK.Core.Platform.IJoystickComponent)|This component is responsible for exposing joystick input and interfacing with joystick controllers.|
|[`IClipboardComponent`](xref:OpenTK.Core.Platform.IClipboardComponent)|This component is used to read and write to the clipboard.|
|[`IShellComponent`](xref:OpenTK.Core.Platform.IShellComponent)|This component exposes functions for interacting with the system more generally. Things like battery status and user prefered theme is exposed here.|

Some platform implementation of these interfaces contains platform specific functions that can be called. A complete description of these function can be found in [Platform Spcific Settings](Platform-Specific-Settings.md).
