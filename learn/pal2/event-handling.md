
# Introduction

OpenTK provides a number of events that can be used to respond to user actions or system changes. These are exposed through a global event queue.

To register for events all you have to do is register to the `EventQueue.EventRaised` event. The handler should look like this:
```cs
void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
{

}
```
and is registered as follows:
```cs
EventQueue.EventRaised += EventRaised;
```

With this we are ready to handle some events!

The first event most applications are going to want to handle is the `Close` event. We use pattern matching to see if `args` is of the type `CloseEventArgs` which tells us this is a `Close` event.

```cs
void EventRaised(PalHandle? handle, PlatformEventType type, EventArgs args)
{
    if (args is CloseEventArgs close)
    {
        // Destroy the window that the user wanted to close.
        windowComp.Destroy(closeArgs.Window);
    }
}
```

# Window Events

Here follows a list of events that extend [WindowEventArgs](xref:OpenTK.Core.Platform.WindowEventArgs) and what they mean.

|Event Enum|EventArgs type|Description|
|---------|--------------|-----------|
|[`Close`](xref:OpenTK.Core.Platform.PlatformEventType.Close)|[`CloseEventArgs`](xref:OpenTK.Core.Platform.CloseEventArgs)|This event is triggered when the user presses the exit button of the window. `CloseEventArgs.Window` contains the handle to the window the user wanted to close.|
|[`Focus`](xref:OpenTK.Core.Platform.PlatformEventType.Focus)|[`FocusEventArgs`](xref:OpenTK.Core.Platform.FocusEventArgs)|This event is triggered when a window gains or loses input focus. `FocusEventArgs.GotFocus` tells if the window got or lost focus.|
|[`WindowMove`](xref:OpenTK.Core.Platform.PlatformEventType.WindowMove)|[`WindowMoveEventArgs`](xref:OpenTK.Core.Platform.WindowMoveEventArgs)|This event is triggered when a window has its position changed on screen.|
|[`WindowResize`](xref:OpenTK.Core.Platform.PlatformEventType.WindowResize)|[`WindowResizeEventArgs`](xref:OpenTK.Core.Platform.WindowResizeEventArgs)|This event is triggered when a window has its size changed on screen.|
|[`WindowModeChange`](xref:OpenTK.Core.Platform.PlatformEventType.WindowModeChange)|[`WindowModeChangeEventArgs`](xref:OpenTK.Core.Platform.WindowModeChangeEventArgs)|This event is triggered when the window mode of a window changes.|
|[`WindowDpiChange`](xref:OpenTK.Core.Platform.PlatformEventType.WindowDpiChange)|[`WindowDpiChangeEventArgs`](xref:OpenTK.Core.Platform.WindowDpiChangeEventArgs)|This event is triggered when the dpi the window should display at has changed. Typically because the user has moved the window to another monitor with different scaling settings, or because the user changed system dpi settings.|
|[`MouseEnter`](xref:OpenTK.Core.Platform.PlatformEventType.MouseEnter)|[`MouseEnterEventArgs`](xref:OpenTK.Core.Platform.MouseEnterEventArgs)|This event is triggered when the mouse cursor enters or exits a window.|
|[`MouseMove`](xref:OpenTK.Core.Platform.PlatformEventType.MouseMove)|[`MouseMoveEventArgs`](xref:OpenTK.Core.Platform.MouseMoveEventArgs)|This event is triggered when the mouse moves.|
|[`MouseDown`](xref:OpenTK.Core.Platform.PlatformEventType.MouseDown)|[`MouseButtonDownEventArgs`](xref:OpenTK.Core.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`MouseUp`](xref:OpenTK.Core.Platform.PlatformEventType.MouseUp)|[`MouseButtonDownEventArgs`](xref:OpenTK.Core.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`Scroll`](xref:OpenTK.Core.Platform.PlatformEventType.Scroll)|[`ScrollEventArgs`](xref:OpenTK.Core.Platform.ScrollEventArgs)|This event is triggered when the scrollwheel on a mouse is used.|
|[`KeyDown`](xref:OpenTK.Core.Platform.PlatformEventType.KeyDown)|[`KeyDownEventArgs`](xref:OpenTK.Core.Platform.KeyDownEventArgs)|This event is triggered when a keyboard key is pressed. <br><br> Do not use this event to handle typing, use the [TextInput event](#textinput) instead.|
|[`KeyUp`](xref:OpenTK.Core.Platform.PlatformEventType.KeyUp)|[`KeyUpEventArgs`](xref:OpenTK.Core.Platform.KeyUpEventArgs)|This event is triggered when a keyboard key is released. <br><br> Do not use this event to handle typing, use the [TextInput event](#textinput) instead.|
|<span id="textinput">[`TextInput`](xref:OpenTK.Core.Platform.PlatformEventType.TextInput)</span>|[`TextInputEventArgs`](xref:OpenTK.Core.Platform.TextInputEventArgs)|This event is triggered when the user has typed text.|
|[`TextEditing`](xref:OpenTK.Core.Platform.PlatformEventType.TextEditing)|[`TextEditingEventArgs`](xref:OpenTK.Core.Platform.TextEditingEventArgs)|This event is triggered when the user is composing text using something like IME (e.g. Chinese, Japanese, or Korean).|
|[`FileDrop`](xref:OpenTK.Core.Platform.PlatformEventType.FileDrop)|[`FileDropEventArgs`](xref:OpenTK.Core.Platform.FileDropEventArgs)|This event is triggered when a user drags files into a window.|
|[`InputLanguageChanged`](xref:OpenTK.Core.Platform.PlatformEventType.InputLanguageChanged)|[`InputLanguageChangedEventArgs`](xref:OpenTK.Core.Platform.InputLanguageChangedEventArgs)|This event is triggered when the input language for the window has changed.|

# Non-window events

Here follows a list of non-window related events.

|Event Enum|EventArgs type|Description|
|----------|--------------|-----------|
|[`ThemeChange`](xref:OpenTK.Core.Platform.PlatformEventType.ThemeChange)|[`ThemeChangeEventArgs`](xref:OpenTK.Core.Platform.ThemeChangeEventArgs)|This event is triggered when a user changes the preferred theme.|
|[`DisplayConnectionChanged`](xref:OpenTK.Core.Platform.PlatformEventType.DisplayConnectionChanged)|[`DisplayConnectionChangedEventArgs`](xref:OpenTK.Core.Platform.DisplayConnectionChangedEventArgs)|This event is triggered when a display is connected or disconnected from the system.|
|[`PowerStateChange`](xref:OpenTK.Core.Platform.PlatformEventType.PowerStateChange)|[`PowerStateChangeEventArgs`](xref:OpenTK.Core.Platform.PowerStateChangeEventArgs)|This event is triggered when the power state of the system changes. For example if the user put the system to sleep.|
|[`ClipboardUpdate`](xref:OpenTK.Core.Platform.PlatformEventType.ClipboardUpdate)|[`ClipboardUpdateEventArgs`](xref:OpenTK.Core.Platform.ClipboardUpdateEventArgs)|This event is triggered when the contents of the clipboard has changed.|
