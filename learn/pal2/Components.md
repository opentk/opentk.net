# Components

PAL2 is build on top of a set of components that are responsible for their own part of the platform API. The idea is to allow partial implementations of platforms to easily be created and allow support for new platforms to be added in steps. It also groups related APIs into a specific namespace.

There are currently 12 components in PAL2.

|Toolkit|Component|Description|
|-------|---------|-----------|
|`Toolkit.Window`|[`IWindowComponent`](xref:OpenTK.Platform.IWindowComponent)|This component is responsible for creating, destroying and managing windows. It is also responsible for processing events.|
|`Toolkit.OpenGL`|[`IOpenGLComponent`](xref:OpenTK.Platform.IOpenGLComponent)|This components is responsible for creating and managing OpenGL contexts. `SwapBuffers()` lives here.|
|`Toolkit.Vulkan`|[`IVulkanComponent`](xref:OpenTK.Platform.IVulkanComponent)|This components contains convenience functions for creating a vulkan surface from a window.|
|`Toolkit.Display`|[`IDisplayComponent`](xref:OpenTK.Platform.IDisplayComponent)|This component contains functions for retrieving information about displays (aka, monitors or screens) connected to the system. Allows for enumeration of video modes etc. |
|`Toolkit.Cursor`|[`ICursorComponent`](xref:OpenTK.Platform.ICursorComponent)|This component is responsible for loading and managing cursor images that can be set as the mouse cursor.|
|`Toolkit.Icon`|[`IIconComponent`](xref:OpenTK.Platform.IIconComponent)|This component is responsible for loading and managing icon images that can be used as window icons.|
|`Toolkit.Mouse`|[`IMouseComponent`](xref:OpenTK.Platform.IMouseComponent)|This component allows for getting and setting of the mouse cursor position.|
|`Toolkit.Keyboard`|[`IKeyboardComponent`](xref:OpenTK.Platform.IKeyboardComponent)|This component is responsible for translating platform dependent keycodes and scancodes to a cross platform representation. This component also exposes information about keyboard layout and has functions for IME input.|
|`Toolkit.Joystick`|[`IJoystickComponent`](xref:OpenTK.Platform.IJoystickComponent)|This component is responsible for exposing joystick input and interfacing with joystick controllers.|
|`Toolkit.Clipboard`|[`IClipboardComponent`](xref:OpenTK.Platform.IClipboardComponent)|This component is used to read and write to the clipboard.|
|`Toolkit.Shell`|[`IShellComponent`](xref:OpenTK.Platform.IShellComponent)|This component exposes functions for interacting with the system more generally. Things like battery status and user prefered theme is exposed here.|
|`Toolkit.Dialog`|[`IDialogComponent`](xref:OpenTK.Platform.IDialogComponent)|This component has functions for creating modal message boxes and open/save file dialogs.|

Some platform implementation of these interfaces contains platform specific functions that can be called. A complete description of these function can be found in [Platform Spcific Settings](Platform-Specific-Settings.md).
