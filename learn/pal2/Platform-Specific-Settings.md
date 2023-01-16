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