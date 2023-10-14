# Almost Native Graphics Layer Engine ([ANGLE](https://chromium.googlesource.com/angle/angle/+/main/README.md))
ANGLE allows for the creation of compliant OpenGL ES contexts running on underlying platform APIs. Most often used by browsers to provide WebGL, but also useful for writing code that is portable between desktop applications and mobile phones. PAL2 has built-in support for ANGLE using [`ANGLEOpenGLComponent`](xref:OpenTK.Platform.Native.ANGLE.ANGLEOpenGLComponent). [`PlatformComponents.PreferANGLE`]() can be used to prefer loading `ANGLEOpenGLComponent` over the native `IOpenGLComponent`. Contexts created by `ANGLEOpenGLComponent` will be OpenGL ES (**not** Desktop OpenGL contexts) contexts of thee requested version.

To be able to run `ANGLEOpenGLComponent` the ANGLE native dependencies need to be available to load. On windows those are: `libEGL.dll`, `libGLESv1_CM.dll`, `libGLESv2.dll`, and `d3dcompiler_47.dll` (as of 2023-10-14).

For other platforms such as linux and macOS other dependencies are needed. These will be documented in time, and ideally a nuget package with the native ANGLE dependencies will be created.
