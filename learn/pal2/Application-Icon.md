# Setting up your PAL2 application with an application icon

Every operating system has a different way of handling application icons. This document will guide you through the different icon options available in PAL2 and how to set them up to work.

The simplest way to set your application icon is through [`Toolkit.Window.SetIcon`](xref:OpenTK.Platform.IWindowComponent.SetIcon(OpenTK.Platform.WindowHandle,OpenTK.Platform.IconHandle)) which will set the window icon at runtime. But this might be undesirable as the desktop icon that the operating system will use to represent your application will still be unset. This is where the operating system specific setup comes into play.

Here are the guides for the different operating systems.
- [Windows](#windows)
- [macOS](#macos)
- [Linux](#linux)

## Windows

> [!NOTE]
> TODO: Add note about multi-resolution icons and accessing embedded resources through the PAL2 interface.

### The simple way
On windows your `.exe` contains a number of embedded resources. These resources contain a bunch of info related to your application. One of these pieces of information is the icon the `.exe` should use. The easiest way to set this up is by adding the following line to your `.csproj` file:
```xml
<PropertyGroup>
    <ApplicationIcon>path/to/icon.ico</ApplicationIcon>
</PropertyGroup>
```

This will automatically add this `.ico` file into the manifest of your built `.exe` and so the application will have the correct appearence in explorer and on the desktop.

### The more involved way

Setting the icon like we did the simple way works fine for application icons, but if you want to have more control over your icon or potentially add native `.cur` cursors to your project you might want to consider using a win32 resource file (`.res`). 
This requires a little setup when building C# projects, this is because you need to compile your binary `.res` file from a `.rc` file using `rc.exe` (located in your windows SDK, e.g. `C:\Program Files (x86)\Windows Kits\10\bin\10.0.22000.0\x64\rc.exe`). More info on resource files can be found here: https://learn.microsoft.com/en-us/windows/win32/menurc/about-resource-files

But a basic `app.rc` file might look like this:
```rc
app_ico ICON "path/to/app.ico"
```
`app_ico` is a name you can later use to load this icon, but by placing it as the first icon in the `.rc` file it will also become the application icon.

Then to compile this file you would run `rc.exe app.rc` which will produce a `app.res` file that we will now include include in our `.csproj` like this:

```xml
<PropertyGroup>
    <Win32Resource>path/to/app.res</Win32Resource>
</PropertyGroup>
```

This will allow you to add additional entries in your `.rc` resource file like cursors, manifests, or whatever else you might want. To read more about windows publishing, please read this article: [TODO: Windows publishing]()

## macOS

> [!NOTE]
> TODO: Mention `MacOSIconComponent.CreateSFSymbol`?

On macOS individual windows do not have icons. Instead there is a single application icon for all windows. This is called the dock icon and can be set through [`MacOSWindowComponent.SetDockIcon`](xref:OpenTK.Platform.Native.macOS.MacOSWindowComponent.SetDockIcon(OpenTK.Platform.WindowHandle,OpenTK.Platform.IconHandle)). This only sets the application icon at runtime, so the published application will not have an icon set.

On macOS an application is a special folder that contains a bunch of different files and folders. One of these files is called `info.plist` and contains a bunch of information about your application, including the icon to use for the application.

A simple `info.plist` could look like this:
```plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>CFBundleIdentifier</key>
    <string>my.app.Application</string>
    <key>CFBundleInfoDictionaryVersion</key>
	<string>6.0</string>
    <key>CFBundleName</key>
	<string>My app name</string>
    <key>CFBundleIconFile</key>
    <string>icon.png</string>
    <key>CFBundlePackageType</key>
	<string>APPL</string>
</dict>
</plist>
```

You can read more about `info.plist` and other keys and features related to it here: https://developer.apple.com/documentation/bundleresources/information_property_list/managing_your_app_s_information_property_list?language=objc

`info.plist` files are most easily authored using the Xcode tools available through the developer kit on macOS, but can be written by hand with enough attention to detail.

`info.plist` keys of interest might be `CFBundleDisplayName` in combination with a `InfoPlist.strings` file for application name localization, or `CFBundleDevelopmentRegion` to set the default language for the application.

To include this in your project we can do the following in the `.csproj`:
```xml
<ItemList>
    <ItemGroup>
        <!-- FIXME: Add condition on compiling for macOS so we don't have an unecessary files on other platforms. -->
        <Content Include="info.plist">
            <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
        </Content>
    </ItemGroup>
</PropertyGroup>
```

To read more about packaging on macOS, read this article: [TODO: macOS publishing]().

## Linux

### The simple way

If you need to create a multi-resoltion icon at runtime you can use the [`X11IconComponent.Create(int, int, IconImage[])`](xref:OpenTK.Platform.Native.X11.X11IconComponent.Create(System.Int32,System.Int32,OpenTK.Platform.Native.X11.X11IconComponent.IconImage[])) api to create the icon, and then use [`Toolkit.Window.SetIcon`](xref:OpenTK.Platform.IWindowComponent.SetIcon(OpenTK.Platform.WindowHandle,OpenTK.Platform.IconHandle)) like usual. This still has the problem that the file browser will not show the application icon properly.

### The more involved way

On KDE and GNOME `.desktop` files are used to specify application metadata similar to `info.plist`. The specification for how these `.desktop` files can be found here: https://specifications.freedesktop.org/desktop-entry-spec/latest/

Here is a basic `my.application.desktop` (TODO: Make a note about this matching some OpenTK specified d-bus name... something something):
```
[Desktop Entry]
Version=1.0
Type=Application
Name=My Application
Exec=/usr/bin/myapplication
Path=/usr/bin/
Icon=/usr/share/icons/Humanity/apps/32/icon.svg
Terminal=false
Categories=Game;
```

Additional entires might be interesting such as `PrefersNonDefaultGPU` as a hint to default to a more powerful GPU (if the desktop environment honors this hint).

This file should then at install time be placed into `/usr/share/applications/my.application.desktop`. To read more about publishing for linux, please read this article: [TODO: linux publishing]()