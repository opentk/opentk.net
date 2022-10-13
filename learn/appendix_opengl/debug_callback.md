Debug Callback
==============
OpenGL didn't have a comprehensive error notification system at its inception.
The programmer was limited to querying `glError()` after commands, and the
information provided in the return value was nothing more than the code for the
error. However since then, OpenGL has introduced the ability for programmers to
register a callback function which delivers crucial debugging messages like
errors and warnings to your code.

The history of the debug callback dates to OpenGL 3.0, where AMD created the
vendor specific extension [`AMD_debug_output`](https://registry.khronos.org/OpenGL/extensions/AMD/AMD_debug_output.txt 
"Specification document for AMD_debug_output.") providing this functionality.
This extensions was later approved by the OpenGL Architecture Review Board (ARB)
as the extension [`ARB_debug_output`](https://registry.khronos.org/OpenGL/extensions/ARB/ARB_debug_output.txt
"Specification document for ARB_debug_output.") in 2010\. Finally, since OpenGL
4.3, the extension is a core part of the API, as well as the extension
[`KHR_debug`](https://registry.khronos.org/OpenGL/extensions/KHR/KHR_debug.txt
"Specification document for KHR_debug."). This article covers usage of
both `ARB_debug_output` extension and the 4.3+ API. `KHR_debug` however covers
so much more than what was introduced in the original extension.

Firstly, you should create a function which will execute when OpenGL has
debugging information to relay to your program.

```cs
using System;
using System.Runtime.InteropServices;
using OpenTK.Graphics.OpenGL4; // Or alternatively OpenTK.Graphics.OpenGL

// ...

private static void OnDebugMessage(
    DebugSource source,     // Source of the debugging message.
    DebugType type,         // Type of the debugging message.
    int id,                 // ID associated with the message.
    DebugSeverity severity, // Severity of the message.
    int length,             // Length of the string in pMessage.
    IntPtr pMessage,        // Pointer to message string.
    IntPtr pUserParam)      // The pointer you gave to OpenGL, explained later.
{
    // In order to access the string pointed to by pMessage, you can use Marshal
    // class to copy its contents to a C# string without unsafe code. You can
    // also use the new function Marshal.PtrToStringUTF8 since .NET Core 1.1.
    string message = Marshal.PtrToStringAnsi(pMessage, length);

    // The rest of the function is up to you to implement, however a debug output
    // is always useful.
    Console.WriteLine("[{0} source={1} type={2} id={3}] {4}", severity, source, type, id, message);

    // Potentially, you may want to throw from the function for certain severity
    // messages.
    if (type == DebugType.DebugTypeError)
    {
        throw new Exception(message);
    }
}
```

> [!NOTE]
> Neither the access modifier or whether the method is an instance method or a
> static method matters.

> [!WARNING]
> This function is called from native code, which means there are stack frames
> associated with them. If your debugger does not support mixed-mode debugging
> it may affect your user experience if an exception is thrown or occurs within
> the callback.

Then you should create a delegate which encapsulates the function you just
implmented.
# [Base](#tab/delegate-gl)
```cs
private static GLDebugProc DebugMessageDelegate = OnDebugMessage;
```
# [KHR_debug](#tab/delegate-khr)
```cs
private static GLDebugProcKHR DebugMessageDelegate = OnDebugMessage;
```
# [ARB_debug_output](#tab/delegate-arb)
```cs
private static GLDebugProcARB DebugMessageDelegate = OnDebugMessage;
```
***
<br/>

> [!NOTE]
> Creating this reference is critical. A delegate is managed by the .NET garbage
> collector, however, this delegate will be passed to a native function as a
> pointer. The garbage collector cannot track outside references, and if there
> are no references to the delegate, the pointer passed to the native function
> becomes "dangling" (in simpler terms, invalid). This causes an access violation
> whenever OpenGL attempts to call you back.
>
> And as before, access modifiers and instance/static do not matter as long as it
> suits your code.

Finally you can provide OpenGL your delegate as your debug callback. You can now
enable debug output, and optionally enable synchronous output.

# [Base](#tab/enable-gl)
```cs
GL.DebugMessageCallback(DebugMessageDelegate, IntPtr.Zero);
GL.Enable(EnableCap.DebugOutput);

// Optionally
GL.Enable(EnableCap.DebugOutputSynchronous)
```
# [KHR_debug](#tab/enable-khr)
```cs
GL.Khr.DebugMessageCallback(DebugMessageDelegate, IntPtr.Zero);
GL.Enable(EnableCap.DebugOutput);

// Optionally
GL.Enable(EnableCap.DebugOutputSynchronous)
```
# [ARB_debug_output](#tab/enable-arb)
```cs
GL.Arb.DebugMessageCallback(DebugMessageDelegate, IntPtr.Zero);
GL.Enable(EnableCap.DebugOutput);

// Optionally
GL.Enable(EnableCap.DebugOutputSynchronous)
```
***
<br/>

> [!TIP]
> Using synchronous output may decrease your performance significantly as OpenGL
> will only call your function from the thread that the context is owned by.
> However, this will allow you to easily break in the callback function to
> analyze the situtaion, such as viewing the stack trace and finding the culprit
> code. Otherwise the graphics driver is allowed to call the function from any
> thread concurrently. Be careful of the usual pitfalls of multithreading when
> disabled.

> [!NOTE]
> The second paramter of `DebugMessageCallback*` determines the value of the
> `pUserParam` parameter in the callback. This parameter is designed for C users
> which has no concept delegates, but only function pointers (before C23). If you
> are very interested in using this parameter (instead of capturing objects via
> the delegate as recommended) you can use any C# pointer, or a pointer to a C#
> [GCHandle](https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.gchandle?view=net-7.0 "A structure which allows native code to reference a C# object.").
> Common pointer gotchas apply.

References
----------
 * This page is based on the [gist of Vassalware](https://gist.github.com/Vassalware/d47ff5e60580caf2cbbf0f31aa20af5d "The original page this documentation is based on.") on the topic.
 * [AMD_debug_output](https://registry.khronos.org/OpenGL/extensions/AMD/AMD_debug_output.txt "Specification document for AMD_debug_output.")
 * [ARB_debug_output](https://registry.khronos.org/OpenGL/extensions/ARB/ARB_debug_output.txt "Specification document for ARB_debug_output.")
 * [KHR_debug](https://registry.khronos.org/OpenGL/extensions/KHR/KHR_debug.txt "Specification document for KHR_debug.")
 * [System.Runtime.InteropServices.Marshal](https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.marshal?view=net-6.0 "Reference page for System.Runtime.InteropServices.Marshal class.")
 * [System.Runtime.InteropServices.GCHandle](https://learn.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.gchandle?view=net-7.0 "A structure which allows native code to reference a C# object.")
