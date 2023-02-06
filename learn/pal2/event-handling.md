
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

## Close

> Enum: `PlatformEventType.Close`

> Args: [`CloseEventArgs`](xref:OpenTK.Core.Platform.CloseEventArgs)

This event is triggered when the user presses the exit button of the window. `CloseEventArgs.Window` contains the handle to the window the user wanted to close.

## Focus

> Enum: `PlatformEventType.Focus`

> Args: [`FocusEventArgs`](xref:OpenTK.Core.Platform.FocusEventArgs)

This event is triggered when a window gains or loses input focus. `FocusEventArgs.GotFocus` tells if the window got or lost focus.

## WindowMove

> Enum: `PlatformEventType.WindowMove`

> Args: [`WindowMoveEventArgs`](xref:OpenTK.Core.Platform.WindowMoveEventArgs)

This event is triggered when a window has its position changed on screen.

## WindowResize

> Enum: `PlatformEventType.WindowResize`

> Args: [`WindowResizeEventArgs`](xref:OpenTK.Core.Platform.WindowResizeEventArgs)

This event is triggered when a window has its size changed on screen.

## WindowModeChange

> Enum: `PlatformEventType.WindowModeChange`

> Args: [`WindowModeChangeEventArgs`](xref:OpenTK.Core.Platform.WindowModeChangeEventArgs)

This event is triggered when the window mode of a window changes.

## MouseEnter

> Enum: `PlatformEventType.MouseEnter`

> Args: [`MouseEnterEventArgs`](xref:OpenTK.Core.Platform.MouseEnterEventArgs)

This event is triggered when the mouse cursor enters or exits a window.

## MouseMove

> Enum: `PlatformEventType.MouseMove`

> Args: [`MouseMoveEventArgs`](xref:OpenTK.Core.Platform.MouseMoveEventArgs)

This event is triggered when the mouse moves.

## MouseDown

> Enum: `PlatformEventType.MouseDown`

> Args: [`MouseButtonDownEventArgs`](xref:OpenTK.Core.Platform.MouseButtonDownEventArgs)

This event is triggered when a mouse button is pressed.

## MouseUp

> Enum: `PlatformEventType.MouseUp`

> Args: [`MouseButtonUpEventArgs`](xref:OpenTK.Core.Platform.MouseButtonUpEventArgs)

This event is triggered when a mouse button is released.

## Scroll

> Enum: `PlatformEventType.Scroll`

> Args: [`ScrollEventArgs`](xref:OpenTK.Core.Platform.ScrollEventArgs)

This event is triggered when the scrollwheel on a mouse is used.

## KeyDown

> Enum: `PlatformEventType.KeyDown`

> Args: [`KeyDownEventArgs`](xref:OpenTK.Core.Platform.KeyDownEventArgs)

This event is triggered when a keyboard key is pressed.

Do not use this event to handle typing, use the [TextInput event](#textinput) instead.

## KeyUp

> Enum: `PlatformEventType.KeyUp`

> Args: [`KeyUpEventArgs`](xref:OpenTK.Core.Platform.KeyUpEventArgs)

This event is triggered when a keyboard key is released.

Do not use this event to handle typing, use the [TextInput event](#textinput) instead.

## TextInput

> Enum: `PlatformEventType.TextInput`

> Args: [`TextInputEventArgs`](xref:OpenTK.Core.Platform.TextInputEventArgs)

This event is triggered when the user has typed text.

## TextEditing

> Enum: `PlatformEventType.TextEditing`

> Args: [`TextEditingEventArgs`](xref:OpenTK.Core.Platform.TextEditingEventArgs)

This event is triggered when the user is composing text using something like IME (e.g. Chinese, Japanese, or Korean).

## FileDrop

> Enum: `PlatformEventType.FileDrop`

> Args: [`FileDropEventArgs`](xref:OpenTK.Core.Platform.FileDropEventArgs)

This event is triggered when a user drags files into a window.

# Non-window events

Here follows a list of non-window related events.

## ThemeChange

> Enum: `PlatformEventType.ThemeChange`

> Args: [`ThemeChangeEventArgs`](xref:OpenTK.Core.Platform.ThemeChangeEventArgs)

This event is triggered when a user changes the preferred theme.

## DisplayConnectionChanged

> Enum: `PlatformEventType.DisplayConnectionChanged`

> Args: [`DisplayConnectionChangedEventArgs`](xref:OpenTK.Core.Platform.DisplayConnectionChangedEventArgs)

This event is triggered when a display is connected or disconnected from the system.

## PowerStateChange

> Enum: `PlatformEventType.PowerStateChange`

> Args: [`PowerStateChangeEventArgs`](xref:OpenTK.Core.Platform.PowerStateChangeEventArgs)

This event is triggered when the power state of the system changes.
