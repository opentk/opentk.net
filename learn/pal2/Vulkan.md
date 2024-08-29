# Getting started with the OpenTK PAL2 Vulkan component

OpenTK 5 brings Vulkan bindings to OpenTK. Unlike OpenGL Vulkan contains extensions for platform creation and display, so with the basic native access that both GLFW and PAL2 offer it's possible to create your own Vulkan instance and device just with the OpenTK Vulkan bindings.

However this requires code for each operating system which is not very beginner friendly or in the spirit of OpenTK as a cross-platform API. So like APIs like GLFW and SDL, OpenTKs PAL2 contains a Vulkan component that makes creating cross-platform device instances easier.

## The [`VKLoader`](xref:OpenTK.Graphics.VKLoader) class

The first piece of the puzzle is the [`VKLoader`](xref:OpenTK.Graphics.VKLoader) class that contains the logic for loading Vulkan function entry-points.
All of the `VK.*` functions use this loader to on-demand load Vulkan functions as they are called.
For this to work [`VKLoader`](xref:OpenTK.Graphics.VKLoader) must first dynamically load the Vulkan library of the current platform by calling [`VKLoader.Init()`](xref:OpenTK.Graphics.VKLoader.Init).
Below is a list of the default names OpenTK will look for:

|Platform|Vulkan dynamic library|
|--------|----------------------|
|Windows |`vulkan-1.dll`        |
|Linux   |`libvulkan.so.1`      |
|FreeBSD |`libvulkan.so.1`      |
|macOS   |`libvulkan.1.dylib`   |

After the Vulkan loader has been found [`VKLoader`](xref:OpenTK.Graphics.VKLoader) will load the `vkGetInstanceProcAddr` entry-point which is the future base for all function loading in Vulkan. This type of loading will work fine for functions that are "global commands":

|Global commands|
|----|
|[`vkEnumerateInstanceVersion`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/vkEnumerateInstanceVersion.html)|
|[`vkEnumerateInstanceExtensionProperties`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/vkEnumerateInstanceExtensionProperties.html)|
|[`vkEnumerateInstanceLayerProperties`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/vkEnumerateInstanceLayerProperties.html)|
|[`vkCreateInstance`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/vkCreateInstance.html)|

To load further entry-points a Vulkan instance needs to be created and provided to [`VKLoader`](xref:OpenTK.Graphics.VKLoader) through [`VKLoader.SetInstance(VkInstance)`](xref:OpenTK.Graphics.VKLoader.SetInstance(OpenTK.Graphics.Vulkan.VkInstance)). This will enable loading of further function entry-points of the Vulkan API.

> [!IMPORTANT]
> Currently OpenTKs Vulkan bindings do not have built-in support for loading through `vkGetDeviceProcAddr` or having multiple sets of entry-points for different devices. For now `vkGetInstanceProcAddr` is the only option. Enabling `vkGetDeviceProcAddr` loading is planned functionality.

## Using the PAL2 Vulkan component

The PAL2 Vulkan component is accesible through [`Toolkit.Vulkan`](xref:OpenTK.Platform.Toolkit.Vulkan) and contains convenience features for easily creating cross-platform Vulkan instances and devices.

|Function|Description|
|--------|-----------|
|[`Toolkit.Vulkan.GetRequiredInstanceExtensions()`](xref:OpenTK.Platform.IVulkanComponent.GetRequiredInstanceExtensions)|This function returns a list of required Vulkan instance extensions required to be able to create a window surface on the current platform.|
|[`Toolkit.Vulkan.GetPhysicalDevicePresentationSupport(...)`](xref:OpenTK.Platform.IVulkanComponent.GetPhysicalDevicePresentationSupport(OpenTK.Graphics.Vulkan.VkInstance,OpenTK.Graphics.Vulkan.VkPhysicalDevice,System.UInt32))|Queries if the specified device queue supports presentation. Only queues which support presentation can be passed to [`vkQueuePresentKHR`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/vkQueuePresentKHR.html) and therefore only these queues can be displayed to the user.|
|[`Toolkit.Vulkan.CreateWindowSurface(...)`](xref:OpenTK.Platform.IVulkanComponent.CreateWindowSurface(OpenTK.Graphics.Vulkan.VkInstance,OpenTK.Platform.WindowHandle,OpenTK.Graphics.Vulkan.VkAllocationCallbacks*,OpenTK.Graphics.Vulkan.VkSurfaceKHR@))|This is used to create a `VkSurfaceKHR` from a `WindowHandle` on the current platform. This function calls the instance extensions enabled by `GetRequiredInstanceExtensions()` to create the surface.|

With these functions in-hand, following any Vulkan tutorial (written for c/c++) using OpenTK should be pretty easy to adapt to be able to use these functions. For a complete working example, please see the [VulkanTestProject](https://github.com/opentk/opentk/blob/opentk5.0/tests/VulkanTestProject/Program.cs) in the OpenTK repo.