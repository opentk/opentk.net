# Colors
We briefly mentioned how to work with colors in OpenGL in the previous tutorials, but so far only touched the surface of colors. Here we'll extensively discuss what colors are and start building the scene for the upcoming lighting tutorials.

In the real world, colors can take practically any known color value with each object having its own color(s). In the digital world we need to map the (infinite) real colors to (limited) digital values and therefore not all real-world colors can be represented digitally. We can however represent so many colors that you probably won't notice the difference anyways. Colors are digitally represented using a **red**, **green** and **blue** component commonly abbreviated as **RGB**. Using different combinations of just those 3 values we can represent almost any color there is. For example, to get a coral color we define a color vector as:

```cs
Color4 coral = new Color4(1.0f, 0.5f, 0.31f, 1.0f);
```
>Note: there is an extra component **alpha** which is how transparent the color is, this is only helpful when displaying several colors on top of each other, the majority of the time we can just leave this value at 1.

The colors we see in real life are not the colors the objects actually have, but are the colors **reflected** from the object; the colors that are not absorbed (rejected) by the objects are the colors we perceive of them. For example, the light of the sun is perceived as a white light that is the combined sum of many different colors (as you can see in the image). So if we would shine the white light on a blue toy, it absorbs all the white color's sub-colors except the blue color. Since the toy does not absorb the blue value, it is reflected and this reflected light enters our eye, making it look like the toy has a blue color. The following image shows this for a coral colored toy where it reflects several colors with varying intensity:

![Light reflection](img/1-light_reflection.png)

You can see that the white sunlight is actually a collection of all the visible colors and the object absorbs a large portion of those colors. It only reflects those colors that represent the object's color and the combination of those is what we perceive (in this case a coral color).

These rules of color reflection apply directly in graphics-land. When we define a light source in OpenGL we want to give this light source a color. In the previous paragraph we had a white color so we'll give the light source a white color as well. If we would then multiply the light source's color with an object's color value, the resulting color is the reflected color of the object (and thus its perceived color). Let's revisit our toy (this time with a coral value) and see how we would calculate its perceivable color in graphics-land. We retrieve the resulting color vector by doing a component-wise multiplication on both color vectors:

```cs
Color4 lightColor = new Color4(1.0f, 1.0f, 1.0f, 1.0f);
Color4 toyColor = new Color4(1.0f, 0.5f, 0.31f, 1.0f);
Color4 result = lightColor * toyColor; // = (1.0f, 0.5f, 0.31f, 1.0f);
```
We can see that the toy's color *absorbs* a large portion of the white light, but reflects several red, green and blue values based on its own color value. This is a representation of how colors would work in real life. We can thus define an object's color as *the amount of each color component it reflects from a light source*. Now what would happen if we used a green light?

```cs
Color4 lightColor = new Color4(0.0f, 1.0f, 0.0f, 1.0f);
Color4 toyColor = new Color4(1.0f, 0.5f, 0.31f, 1.0f);
Color4 result = lightColor * toyColor; // = (0.0f, 0.5f, 0.0f, 1.0f);
```

As we can see, the toy has no red and blue light to absorb and/or reflect The toy also absorbs half of the light's green value, but also still reflects half of the light's green value. The toy's color we perceive would then be a dark-greenish color. We can see that if we use a green light, only the green color components can be reflected and thus perceived; no red and blue colors are perceived. The result is that the coral object suddenly becomes a dark-greenish object. Let's try one more example with a dark olive-green light:

```cs
Color4 lightColor = new Color4(0.33f, 0.42f, 0.18f, 1.0f);
Color4 toyColor = new Color4(1.0f, 0.5f, 0.31f, 1.0f);
Color4 result = lightColor * toyColor; // = (0.33f, 0.21f, 0.06f, 1.0f);
```

As you can see, we can get unexpected colors from objects by using different light colors. It's not hard to get creative with colors.

But enough about colors, let's start building a scene where we can experiment in.

## A lighting scene
In the upcoming tutorials we'll be creating interesting visuals by simulating real-world lighting making extensive use of colors. Since now we'll be using light sources we want to display them as visual objects in the scene and add at least one object to simulate the lighting on.

The first thing we need is an object to cast the light on and we'll use the infamous container cube from the previous tutorials. We will also be needing a light object to show where the light source is located in the 3D scene. For simplicity's sake we'll represent the light source with a cube.

So, filling a vertex buffer object, setting vertex attribute pointers and all that weird stuff should be easy for you by now so we won't walk you through those steps. If you still have difficulties with those items I suggest you review the previous tutorials before continuing.

So, the first thing we will actually need is a vertex shader to draw the container. The vertex positions of the container remain the same (although we won't be needing texture coordinates this time) so the code should be nothing new. We'll be using a stripped down version of the vertex shader from the last tutorials:

```glsl
#version 330 core
layout (location = 0) in vec3 aPos;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
}
```
Make sure to update your vertex data and attribute pointers to correspond with the new vertex shader (if you want you can actually keep the texture data and attribute pointers active; we're just not using them right now, but it's not a bad idea to start from a fresh start).

Because we are also going to create a lamp cube, we want to generate a new VAO specifically for the lamp. We could also represent a lamp using the same VAO and then simply do some transformations on the model matrix, but in the upcoming tutorials we'll be changing the vertex data and attribute pointers of the container object quite often and we don't want these changes to propagate to the lamp object (we only care about the lamp's vertex positions), so we'll create a new VAO:


```cs
//Initialize the vao for the lamp, this is mostly the same as the code for the model cube
_vaoLamp = GL.GenVertexArray();
GL.BindVertexArray(_vaoLamp);
//We only need to bind to the VBO, the container's VBO's data already contains the correct data.
GL.BindBuffer(BufferTarget.ArrayBuffer, _vertexBufferObject);
//Set the vertex attributes (only position data for our lamp)
vertexLocation = _lampShader.GetAttribLocation("aPos");
GL.EnableVertexAttribArray(vertexLocation);
GL.VertexAttribPointer(vertexLocation, 3, VertexAttribPointerType.Float, false, 3 * sizeof(float), 0);
```
The code should be relatively straightforward. Now that we created both the container and the lamp cube there is one thing left to define and that is the fragment shader:

```glsl
#version 330 core
out vec4 FragColor;
  
uniform vec3 objectColor;
uniform vec3 lightColor;

void main()
{
    FragColor = vec4(lightColor * objectColor, 1.0);
}
```
The fragment shader accepts both an object color and a light color from a uniform variable. Here we multiply the light's color with the object's (reflected) color just like we discussed at the beginning of this tutorial. Again, this shader should be easy to understand. Let's set the object's color to the last section's coral color with a white light:

```cs
_lightingShader.SetVector3("objectColor", new Vector3(0.0f, 0.5f, 0.31f));
_lightingShader.SetVector3("lightColor", new Vector3(1.0f, 1.0f, 1.0f));
```
One thing left to note is that when we start to change the vertex and fragment shaders, the lamp cube will change as well and this is not what we want. We don't want the lamp object's color to be affected by the lighting calculations in the upcoming tutorials, but rather keep the lamp isolated from the rest. We want the lamp to have a constant bright color, unaffected by other color changes (this makes it look like the lamp really is the source of the light).

To accomplish this we actually need to create a second set of shaders that we will use to draw the lamp, thus being safe from any changes to the lighting shaders. The vertex shader is the same as the current vertex shader so you can simply copy the source code for the lamp's vertex shader. The fragment shader of the lamp ensures the lamp's color stays bright by defining a constant white color on the lamp:

```glsl
#version 330 core
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0); // set all 4 vector values to 1.0
}
```
When we want to draw our objects, we want to draw the container object (or possibly many other objects) using the lighting shader we just defined and when we want to draw the lamp, we use the lamp's shaders. During the tutorials we'll gradually be updating the lighting shaders to slowly achieve more realistic results.

The main purpose of the lamp cube is to show where the light comes from. We usually define a light source's position somewhere in the scene, but this is simply a position that has no visual meaning. To show the actual lamp we draw the lamp cube at the same location of the light source. This is accomplished by drawing the lamp object with the lamp shader, ensuring the lamp cube always stays white, regardless of the light conditions of the scene.

So let's declare a global vector3 variable that represents the light source's location in world-space coordinates:

```cs
private readonly Vector3 _lightPos = new Vector3(1.2f, 1.0f, 2.0f);
```
We then want to translate the lamp's cube to the light source's position before drawing it and we'll also scale it down a bit to make sure the lamp isn't too dominant:

```cs
Matrix4 lampMatrix = Matrix4.Identity;
lampMatrix *= Matrix4.CreateScale(0.2f);
lampMatrix *= Matrix4.CreateTranslation(_lightPos);
```
The resulting drawing code for the lamp should then look something like this:
```cs
//Draw the lamp
GL.BindVertexArray(_vaoModel);

_lampShader.Use();

Matrix4 lampMatrix = Matrix4.Identity;
lampMatrix *= Matrix4.CreateScale(0.2f);
lampMatrix *= Matrix4.CreateTranslation(_lightPos);

_lampShader.SetMatrix4("model", lampMatrix);
_lampShader.SetMatrix4("view", _camera.GetViewMatrix());
_lampShader.SetMatrix4("projection", _camera.GetProjectionMatrix());

GL.DrawElements(PrimitiveType.Triangles, _indices.Length, DrawElementsType.UnsignedInt, 0);
```		
Injecting all the code fragments at their appropriate locations would then result in a clean OpenGL application properly configured for experimenting with lighting. If everything compiles it should look like this:
![Light reflection](img/1-light_reflection.png)

Not really much to look at right now, but I'll promise it will get more interesting in the upcoming tutorials.

If you have difficulties finding out where all the code snippets fit together in the application as a whole, check the source code [here](https://github.com/opentk/LearnOpenTK/tree/master/Chapter%202/1%20-%20Colors) and carefully work your way through the code/comments.

Now that we have a fair bit of knowledge about colors and created a basic scene for some sexy lighting stuff we can jump to the next tutorial where the real magic begins.