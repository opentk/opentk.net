# Introduction

PAL 2 consists of a number of components that allows the user to create windows and interact with the user system. These interfaces are designed to work cross-platform. But sometimes there are functions and settings that are so platform specific that these is no good cross-platform API for them.

Platform specific component APIs can be accessed by casting the component interface to it's concrete platform type. For example, `IClipboardComponent` has windows specific settings related to cloud clipboard syncing and history, which can be accessed like follows: 

```cs
IClipboardComponent clipboardComp = ...;

// By using 'as' and ?. we can run this code on all platforms
// but the effects will only take effect on windows.
(clipboardComp as Native.Windows.WindowComponent)?.CanUploadToCloudClipboard = false;
(clipboardComp as Native.Windows.WindowComponent)?.CanIncludeInClipboardHistory = true;

```

# Windows
The following is a list of windows specific settings and functions.

|Function|Description|
|--------|-----------|
|`WindowComponent`| |
|[`WindowComponent.GetHWND`](xref:OpenTK.Platform.Native.Windows.WindowComponent.GetHWND(OpenTK.Core.Platform.WindowHandle))|Gets the native Win32 `HWND` handle for the window.|
|`OpenGLComponent`| |
|[`OpenGLComponent.GetHGLRC`](xref:OpenTK.Platform.Native.Windows.OpenGLComponent.GetHGLRC(OpenTK.Core.Platform.OpenGLContextHandle))|Gets the native Win32 `HGLRC` handle for the OpenGL context.|
|`DisplayComponent`| |
|[`DisplayComponent.GetAdapter`](xref:OpenTK.Platform.Native.Windows.DisplayComponent.GetAdapter(OpenTK.Core.Platform.DisplayHandle))|Gets the native Win32 adapter name (e.g. `\\.\DISPLAY1`) for the display handle.|
|[`DisplayComponent.GetMonitor`](xref:OpenTK.Platform.Native.Windows.DisplayComponent.GetMonitor(OpenTK.Core.Platform.DisplayHandle))|Gets the native Win32 monitor name (e.g. `\\.\DISPLAY1\Monitor0`) for the display handle.|
|`ShellComponent`| |
|[`ShellComponent.SetImmersiveDarkMode`](xref:OpenTK.Platform.Native.Windows.ShellComponent.SetImmersiveDarkMode(OpenTK.Core.Platform.WindowHandle,System.Boolean))|Sets the `DWMWA_USE_IMMERSIVE_DARK_MODE` flag on the window causing the titlebar be rendered in dark mode colors. This is only officially supported from Window 11 Build 220000, but can sometimes work on Windows 10.|
|[`ShellComponent.SetCaptionTextColor`](xref:OpenTK.Platform.Native.Windows.ShellComponent.SetCaptionTextColor(OpenTK.Core.Platform.WindowHandle,OpenTK.Mathematics.Color3{OpenTK.Mathematics.Rgb}))|Used to set the text color in the windows titlebar. This is only officially supported from Window 11 Build 220000, but can sometimes work on Windows 10.|
|[`ShellComponent.SetCaptionColor`](xref:OpenTK.Platform.Native.Windows.ShellComponent.SetCaptionColor(OpenTK.Core.Platform.WindowHandle,OpenTK.Mathematics.Color3{OpenTK.Mathematics.Rgb}))|Used to set the color of the window titlebar. This is only officially supported from Window 11 Build 220000, but can sometimes work on Windows 10.|
|`IconComponent`| |
|[`IconComponent.CreateFromIcoFile`](xref:OpenTK.Platform.Native.Windows.IconComponent.CreateFromIcoFile(System.String))|Used to create an `IconHandle` from a `.ico` file. An icon created using this function will be able to dynamically pick resolution if the file contains multiple resolutions.|
|[`IconComponent.CreateFromIcoResource(byte[])`](xref:OpenTK.Platform.Native.Windows.IconComponent.CreateFromIcoResource(System.Byte[]))|Used to create an `IconHandle` from a `.ico` file embedded as a `.resx` resource. An Icon created using this function will **not** be able to dynamically pick resolution.|
|[`IconComponent.CreateFromIcoResource(String)`](xref:OpenTK.Platform.Native.Windows.IconComponent.CreateFromIcoResource(System.String))|Used to create an `IconHandle` from a win32 embedded resource created using an `.rc` file and `<Win32Resource>`.|
|[`IconComponent.GetBitmapData`](xref:OpenTK.Platform.Native.Windows.IconComponent.GetBitmapData(OpenTK.Core.Platform.IconHandle,System.Span{System.Byte}))|Allows for extracting icon data. Works with system icons.|
|[`IconComponent.GetBitmapByteSize`](xref:OpenTK.Platform.Native.Windows.IconComponent.GetBitmapByteSize(OpenTK.Core.Platform.IconHandle))|Returns the size in bytes of the bitmap data. For use with [`IconComponent.GetBitmapData`](xref:OpenTK.Platform.Native.Windows.IconComponent.GetBitmapData(OpenTK.Core.Platform.IconHandle,System.Span{System.Byte})).|
|`CursorComponent`| |
|[`CursorComponent.CreateFromCurFile`](xref:OpenTK.Platform.Native.Windows.CursorComponent.CreateFromCurFile(System.String))|Used to create a `CursorHandle` from a `.cur` file.|
|[`CursorComponent.CreateFromCurResorce(byte[])`](xref:OpenTK.Platform.Native.Windows.CursorComponent.CreateFromCurResorce(System.Byte[]))|Used to create a `CursorHandle` from a `.cur` file embedded in a `.resx` file. Does not support `.ani` files.|
|[`CursorComponent.CreateFromCurResorce(String)`](xref:OpenTK.Platform.Native.Windows.CursorComponent.CreateFromCurResorce(System.String))|Used to create a `CursorHandle` from a `.cur` file embedded in a `.rc` file and using `<Win32Resource>`.|
|[`CursorComponent.GetImage`](xref:OpenTK.Platform.Native.Windows.CursorComponent.GetImage(OpenTK.Core.Platform.CursorHandle,System.Span{System.Byte}))|Allows for extracting cursor data. Works with system cursors.|
|`ClipboardComponent`| |
|[`ClipboardComponent.CanIncludeInClipboardHistory`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.CanIncludeInClipboardHistory)|Toggles whether clipboard data set using [`SetClipboardText`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.SetClipboardText(System.String)) is allowed to be stored in the clipboard history.|
|[`ClipboardComponent.CanUploadToCloudClipboard`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.CanUploadToCloudClipboard)|Toggles whether clipboard data set using [`SetClipboardText`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.SetClipboardText(System.String)) can be by synced between the users devices.|
|[`ClipboardComponent.SetClipboardAudio`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.SetClipboardAudio(OpenTK.Core.Platform.AudioData))|Allows uploading audio data to the clipboard. Audio clipboard support is generally poorly supported by audio applications.|
|[`ClipboardComponent.SetClipboardBitmap`](xref:OpenTK.Platform.Native.Windows.ClipboardComponent.SetClipboardBitmap(OpenTK.Core.Platform.Bitmap))|Allows uploading image data to the clipboard.|

# Linux X11
The following is a list of linux x11 specific settings and functions.

|Function|Description|
|--------|-----------|
|`WindowComponent`| |
|[`X11WindowComponent.DemandAttention`](xref:OpenTK.Platform.Native.X11.X11WindowComponent.DemandAttention(OpenTK.Core.Platform.WindowHandle))|Shows a popup to the user that the window is ready.|
|[`X11WindowComponent.GetX11Display`](xref:OpenTK.Platform.Native.X11.X11WindowComponent.GetX11Display)|Gets the X11 `Display` used by OpenTK.|
|[`X11WindowComponent.GetX11Window`](xref:OpenTK.Platform.Native.X11.X11WindowComponent.GetX11Window(OpenTK.Core.Platform.WindowHandle))|Gets the X11 `Window` for this window handle.|
|`X11OpenGLComponent`||
|[`X11OpenGLComponent.EXT_swap_control_GetMaxSwapInterval`](xref:OpenTK.Platform.Native.X11.X11OpenGLComponent.EXT_swap_control_GetMaxSwapInterval)|Queries `EXT_swap_control` for the maximum supported swap interval.|
|[`X11OpenGLComponent.GetGLXContext`](xref:OpenTK.Platform.Native.X11.X11OpenGLComponent.GetGLXContext(OpenTK.Core.Platform.OpenGLContextHandle))|Get the GLX `GLXContext` for the OpenGL context.|
|[`X11OpenGLComponent.GetGLXWindow`](xref:OpenTK.Platform.Native.X11.X11OpenGLComponent.GetGLXWindow(OpenTK.Core.Platform.OpenGLContextHandle))|Get the GLX `GLXWindow` for the OpenGL context.|
|`X11DisplayComponent`||
|[`X11DisplayComponent.GetRRCrtc`](xref:OpenTK.Platform.Native.X11.X11DisplayComponent.GetRRCrtc(OpenTK.Core.Platform.DisplayHandle))|Get the X11 RandR `RRCrtc` for the display handle.|
|[`X11DisplayComponent.GetRROutput`](xref:OpenTK.Platform.Native.X11.X11DisplayComponent.GetRROutput(OpenTK.Core.Platform.DisplayHandle))|Get the X11 RandR `RROutput` for the display handle.|
|`X11IconComponent`||
|[`X11IconComponent.Create(int, int, IconImage[])`](xref:OpenTK.Platform.Native.X11.X11IconComponent.Create(System.Int32,System.Int32,OpenTK.Platform.Native.X11.X11IconComponent.IconImage[]))|Used to create multi-resolution icons.|

# macOS
The following is a list of macOS specific settings and functions.

|Function|Description|
|--------|-----------|
|`MacOSWindowComponent`| |
|[`MacOSWindowComponent.SetDockIcon`](xref:OpenTK.Platform.Native.macOS.MacOSWindowComponent.SetDockIcon(OpenTK.Core.Platform.WindowHandle,OpenTK.Core.Platform.IconHandle))|Sets the dock icon of the application|
|[`MacOSWindowComponent.GetNSWindow`](xref:OpenTK.Platform.Native.macOS.MacOSWindowComponent.GetNSWindow(OpenTK.Core.Platform.WindowHandle))|Gets the `NSWindow` of the window handle. This is an instance of the `NSOpenTKWindow` subclass of `NSWindow`.|
|[`MacOSWindowComponent.GetNSView`](xref:OpenTK.Platform.Native.macOS.MacOSWindowComponent.GetNSView(OpenTK.Core.Platform.WindowHandle))|Gets the `NSView` of the window handle. This is an instance of the `NSOpenTKView` subclass of `NSView`.|
|`MacOSOpenGLComponent`| |
|[`MacOSOpenGLComponent.GetNSOpenGLContext`](xref:OpenTK.Platform.Native.macOS.MacOSOpenGLComponent.GetNSOpenGLContext(OpenTK.Core.Platform.OpenGLContextHandle))|Gets the `NSOpenGLContext` of the OpenGL context handle.|
|`MacOSIconComponent`||
|[`MacOSIconComponent.CreateSFSymbol`](xref:OpenTK.Platform.Native.macOS.MacOSIconComponent.CreateSFSymbol(System.String,System.String))|Creates a IconHandle from a SF symbol name.|
|`MacOSDisplayComponent`||
|[`MacOSDisplayComponent.GetSafeArea`](xref:OpenTK.Platform.Native.macOS.MacOSDisplayComponent.GetSafeArea(OpenTK.Core.Platform.DisplayHandle,OpenTK.Mathematics.Box2i@))|Gets the area of the screen that is not covered by things like camera housings.|
|[`MacOSDisplayComponent.GetSafeLeftAuxArea`](xref:OpenTK.Platform.Native.macOS.MacOSDisplayComponent.GetSafeLeftAuxArea(OpenTK.Core.Platform.DisplayHandle,OpenTK.Mathematics.Box2i@))|Gets the visible area of the screen to the left of the camera housing or an empty area if there is no camera housing.|
|[`MacOSDisplayComponent.GetSafeRightAuxArea`](xref:OpenTK.Platform.Native.macOS.MacOSDisplayComponent.GetSafeRightAuxArea(OpenTK.Core.Platform.DisplayHandle,OpenTK.Mathematics.Box2i@))|Gets the visible area of the screen to the right of the camera housing or an empty area of there is no camera housing.|
|[`MacOSDisplayComponent.GetDirectDisplayID`](xref:OpenTK.Platform.Native.macOS.MacOSDisplayComponent.GetDirectDisplayID(OpenTK.Core.Platform.DisplayHandle))|Gets the `CGDirectDisplayID` for the display handle.|
|`MacOSCursorComponent`||
|[`MacOSCursorComponent.Create(Frame[] frames, float delay)`](xref:OpenTK.Platform.Native.macOS.MacOSCursorComponent.Create(OpenTK.Platform.Native.macOS.MacOSCursorComponent.Frame[],System.Single))|Used to create an animated cursor.|
|[`MacOSCursorComponent.IsAnimatedCursor`](xref:OpenTK.Platform.Native.macOS.MacOSCursorComponent.IsAnimatedCursor(OpenTK.Core.Platform.CursorHandle))|Used to check if a given cursor is animated.|
|[`MacOSCursorComponent.UpdateAnimation`](xref:OpenTK.Platform.Native.macOS.MacOSCursorComponent.UpdateAnimation(OpenTK.Core.Platform.CursorHandle,System.Double))|Used to update the animation of a given cursor.|

# ANGLE

When using ANGLE to create OpenGL contexts `ANGLEOpenGLComponent` exposes a few ANGLE specific functions.

|Function|Description|
|--------|-----------|
|`ANGLEOpenGLComponent`| |
|[`ANGLEOpenGLComponent.GetEglDisplay`](xref:OpenTK.Platform.Native.ANGLE.ANGLEOpenGLComponent.GetEglDisplay)|Gets the `EGLDisplay` used by OpenTK.|
|[`ANGLEOpenGLComponent.GetEglContext`](xref:OpenTK.Platform.Native.ANGLE.ANGLEOpenGLComponent.GetEglContext(OpenTK.Core.Platform.OpenGLContextHandle))|Gets the `EGLContext` for the OpenGL context.|
|[`ANGLEOpenGLComponent.GetEglSurface`](xref:OpenTK.Platform.Native.ANGLE.ANGLEOpenGLComponent.GetEglSurface(OpenTK.Core.Platform.OpenGLContextHandle))|Gets the `EGLSurface` for the OpenGL context.|