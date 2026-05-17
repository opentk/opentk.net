
# Introduction

OpenTK provides a number of events that can be used to respond to user actions or system changes. These are exposed through a global event queue.

To register for events all you have to do is register to the `Toolkit.Event.EventRaised` event. The handler should look like this:
```cs
void EventRaised(EventArgs args)
{

}
```
and is registered as follows:
```cs
Toolkit.Event.EventRaised += EventRaised;
```

With this we are ready to handle some events!

The first event most applications are going to want to handle is the `Close` event. We use pattern matching to see if `args` is of the type `CloseEventArgs` which tells us this is a `Close` event.

```cs
void EventRaised(EventArgs args)
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

|EventArgs type|Description|
|--------------|-----------|
|[`CloseEventArgs`](xref:OpenTK.Platform.CloseEventArgs)|This event is triggered when the user presses the exit button of the window. `CloseEventArgs.Window` contains the handle to the window the user wanted to close.|
|[`FocusEventArgs`](xref:OpenTK.Platform.FocusEventArgs)|This event is triggered when a window gains or loses input focus. `FocusEventArgs.GotFocus` tells if the window got or lost focus.|
|[`WindowMoveEventArgs`](xref:OpenTK.Platform.WindowMoveEventArgs)|This event is triggered when a window has its position changed on screen.|
|[`WindowResizeEventArgs`](xref:OpenTK.Platform.WindowResizeEventArgs)|This event is triggered when a window has its size changed on screen.|
|[`WindowFramebufferResizeEventArgs`](xref:OpenTK.Platform.WindowFramebufferResizeEventArgs)|This event is triggered when a window has its size changed on screen.|
|[`WindowModeChangeEventArgs`](xref:OpenTK.Platform.WindowModeChangeEventArgs)|This event is triggered when the window mode of a window changes.|
|[`WindowScaleChangeEventArgs`](xref:OpenTK.Platform.WindowScaleChangeEventArgs)|This event is triggered when the scale the window should display at has changed. Typically because the user has moved the window to another monitor with different scaling settings, or because the user changed system dpi settings.|
|[`MouseEnterEventArgs`](xref:OpenTK.Platform.MouseEnterEventArgs)|This event is triggered when the mouse cursor enters or exits a window.|
|[`MouseMoveEventArgs`](xref:OpenTK.Platform.MouseMoveEventArgs)|This event is triggered when the mouse moves.|
|[`RawMouseMoveEventArgs`](xref:OpenTK.Platform.RawMouseMoveEventArgs)|The event is triggered when the mouse moves and raw mouse input is enabled.|
|[`MouseButtonDownEventArgs`](xref:OpenTK.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`MouseButtonDownEventArgs`](xref:OpenTK.Platform.MouseButtonDownEventArgs)|This event is triggered when a mouse button is pressed.|
|[`ScrollEventArgs`](xref:OpenTK.Platform.ScrollEventArgs)|This event is triggered when the scrollwheel on a mouse is used.|
|[`KeyDownEventArgs`](xref:OpenTK.Platform.KeyDownEventArgs)|This event is triggered when a keyboard key is pressed. <br><br> Do not use this event to handle typing, use the [TextInputEventArgs](#textinputeventargs) instead.|
|[`KeyUpEventArgs`](xref:OpenTK.Platform.KeyUpEventArgs)|This event is triggered when a keyboard key is released. <br><br> Do not use this event to handle typing, use the [TextInputEventArgs](#textinputeventargs) instead.|
|[`TextInputEventArgs`](xref:OpenTK.Platform.TextInputEventArgs)|<a name="textinputeventargs"></a>This event is triggered when the user has typed text.|
|[`TextEditingEventArgs`](xref:OpenTK.Platform.TextEditingEventArgs)|This event is triggered when the user is composing text using something like IME (e.g. Chinese, Japanese, or Korean).|
|[`FileDropEventArgs`](xref:OpenTK.Platform.FileDropEventArgs)|This event is triggered when a user drags files into a window.|
|[`InputLanguageChangedEventArgs`](xref:OpenTK.Platform.InputLanguageChangedEventArgs)|This event is triggered when the input language for the window has changed.|

# Non-window events

Here follows a list of non-window related events.

|EventArgs type|Description|
|--------------|-----------|
|[`ThemeChangeEventArgs`](xref:OpenTK.Platform.ThemeChangeEventArgs)|This event is triggered when a user changes the preferred theme.|
|[`DisplayConnectionChangedEventArgs`](xref:OpenTK.Platform.DisplayConnectionChangedEventArgs)|This event is triggered when a display is connected or disconnected from the system.|
|[`PowerStateChangeEventArgs`](xref:OpenTK.Platform.PowerStateChangeEventArgs)|This event is triggered when the power state of the system changes. For example if the user put the system to sleep.|
|[`ClipboardUpdateEventArgs`](xref:OpenTK.Platform.ClipboardUpdateEventArgs)|This event is triggered when the contents of the clipboard has changed.|
