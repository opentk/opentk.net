
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
        Toolkit.Window.Destroy(closeArgs.Window);
    }
}
```

# Window Events

Here follows a list of events that extend [WindowEventArgs](xref:OpenTK.Platform.WindowEventArgs) and what they mean.

|Event Enum|EventArgs type|Description|
|---------|--------------|-----------|
|[`Close`](xref:OpenTK.Platform.PlatformEventType.Close)|[`CloseEventArgs`](xref:OpenTK.Platform.CloseEventArgs)|This event is triggered when the user presses the exit button of the window. `CloseEventArgs.Window` contains the handle to the window the user wanted to close.|
|[`Focus`](xref:OpenTK.Platform.PlatformEventType.Focus)|[`FocusEventArgs`](xref:OpenTK.Platform.FocusEventArgs)|This event is triggered when a window gains or loses input focus. `FocusEventArgs.GotFocus` tells if the window got or lost focus.|
|[`WindowMove`](xref:OpenTK.Platform.PlatformEventType.WindowMove)|[`WindowMoveEventArgs`](xref:OpenTK.Platform.WindowMoveEventArgs)|This event is triggered when a window has its position changed on screen.|
|[`WindowResize`](xref:OpenTK.Platform.PlatformEventType.WindowResize)|[`WindowResizeEventArgs`](xref:OpenTK.Platform.WindowResizeEventArgs)|This event is triggered when a window has its size changed on screen.|
|[`WindowFramebufferResize`](xref:OpenTK.Platform.PlatformEventType.WindowFramebufferResize)|[`WindowFramebufferResizeEventArgs`](xref:OpenTK.Platform.WindowFramebufferResizeEventArgs)|This event is triggered when a window has its size changed on screen.|
|[`WindowModeChange`](xref:OpenTK.Platform.PlatformEventType.WindowModeChange)|[`WindowModeChangeEventArgs`](xref:OpenTK.Platform.WindowModeChangeEventArgs)|This event is triggered when the window mode of a window changes.|
|[`WindowScaleChange`](xref:OpenTK.Platform.PlatformEventType.WindowScaleChange)|[`WindowScaleChangeEventArgs`](xref:OpenTK.Platform.WindowScaleChangeEventArgs)|This event is triggered when the scale the window should display at has changed. Typically because the user has moved the window to another monitor with different scaling settings, or because the user changed system dpi settings.|
|[`MouseEnter`](xref:OpenTK.Platform.PlatformEventType.MouseEnter)|[`MouseEnterEventArgs`](xref:OpenTK.Platform.MouseEnterEventArgs)|This event is triggered when the mouse cursor enters or exits a window.|
|[`MouseMove`](xref:OpenTK.Platform.PlatformEventType.MouseMove)|[`MouseMoveEventArgs`](xref:OpenTK.Platform.MouseMoveEventArgs)|This event is triggered when the mouse moves.|
|[`RawMouseMove`](xref:OpenTK.Platform.PlatformEventType.RawMouseMove)|[`RawMouseMoveEventArgs`](xref:OpenTK.Platform.RawMouseMoveEventArgs)|The event is triggered when the mouse moves and raw mouse input is enabled.|
|[`MouseDown`](xref:OpenTK.Platform.PlatformEventType.MouseDown)|[`MouseButtonDownEventArgs`](xref:OpenTK.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`MouseUp`](xref:OpenTK.Platform.PlatformEventType.MouseUp)|[`MouseButtonDownEventArgs`](xref:OpenTK.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`Scroll`](xref:OpenTK.Platform.PlatformEventType.Scroll)|[`ScrollEventArgs`](xref:OpenTK.Platform.ScrollEventArgs)|This event is triggered when the scrollwheel on a mouse is used.|
|[`KeyDown`](xref:OpenTK.Platform.PlatformEventType.KeyDown)|[`KeyDownEventArgs`](xref:OpenTK.Platform.KeyDownEventArgs)|This event is triggered when a keyboard key is pressed. <br><br> Do not use this event to handle typing, use the [TextInput event](#textinput) instead.|
|[`KeyUp`](xref:OpenTK.Platform.PlatformEventType.KeyUp)|[`KeyUpEventArgs`](xref:OpenTK.Platform.KeyUpEventArgs)|This event is triggered when a keyboard key is released. <br><br> Do not use this event to handle typing, use the [TextInput event](#textinput) instead.|
|<span id="textinput">[`TextInput`](xref:OpenTK.Platform.PlatformEventType.TextInput)</span>|[`TextInputEventArgs`](xref:OpenTK.Platform.TextInputEventArgs)|This event is triggered when the user has typed text.|
|[`TextEditing`](xref:OpenTK.Platform.PlatformEventType.TextEditing)|[`TextEditingEventArgs`](xref:OpenTK.Platform.TextEditingEventArgs)|This event is triggered when the user is composing text using something like IME (e.g. Chinese, Japanese, or Korean).|
|[`FileDrop`](xref:OpenTK.Platform.PlatformEventType.FileDrop)|[`FileDropEventArgs`](xref:OpenTK.Platform.FileDropEventArgs)|This event is triggered when a user drags files into a window.|
|[`InputLanguageChanged`](xref:OpenTK.Platform.PlatformEventType.InputLanguageChanged)|[`InputLanguageChangedEventArgs`](xref:OpenTK.Platform.InputLanguageChangedEventArgs)|This event is triggered when the input language for the window has changed.|

# Non-window events

Here follows a list of non-window related events.

|Event Enum|EventArgs type|Description|
|----------|--------------|-----------|
|[`ThemeChange`](xref:OpenTK.Platform.PlatformEventType.ThemeChange)|[`ThemeChangeEventArgs`](xref:OpenTK.Platform.ThemeChangeEventArgs)|This event is triggered when a user changes the preferred theme.|
|[`DisplayConnectionChanged`](xref:OpenTK.Platform.PlatformEventType.DisplayConnectionChanged)|[`DisplayConnectionChangedEventArgs`](xref:OpenTK.Platform.DisplayConnectionChangedEventArgs)|This event is triggered when a display is connected or disconnected from the system.|
|[`PowerStateChange`](xref:OpenTK.Platform.PlatformEventType.PowerStateChange)|[`PowerStateChangeEventArgs`](xref:OpenTK.Platform.PowerStateChangeEventArgs)|This event is triggered when the power state of the system changes. For example if the user put the system to sleep.|
|[`ClipboardUpdate`](xref:OpenTK.Platform.PlatformEventType.ClipboardUpdate)|[`ClipboardUpdateEventArgs`](xref:OpenTK.Platform.ClipboardUpdateEventArgs)|This event is triggered when the contents of the clipboard has changed.|
