# Element Buffer Objects
We've rendered our first triangle, but what if we wanted to do something more complex? For instance, a rectangle?

OpenGL works exclusively in triangles. It has partial support for squares and other shapes, but those have been deprecated for a very long time and should not be used. You could do it by defining six vertices to make two triangles that look like they're one shape, but that would be a waste since a rectangle only needs four vertices. While that might not seem terrible, consider a cube: only eight vertices if none are wasted, but if you used the approach of making each face out of two triangles, that'd take 36 vertices just for a single cube! The numbers only get worse the more complex your model is; clearly, a better way is needed.

There are many different ways to do this (such as a triangle strip), for this tutorial we'll use what's called an Element Buffer Object, which is a type of buffer that lets us re-use vertices to create multiple primitives out of them. Using an EBO, we'll be able to create a rectangle using only four vertices.

Replace your vertex array with the following:
```cs
float[] vertices = {
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left 
};
```

Then, below it, add the following array:
```cs
uint indices[] = {  // note that we start from 0!
    0, 1, 3,   // first triangle
    1, 2, 3    // second triangle
}; 
```

The indices array will be fed to our EBO to specify which vertices will be used to create triangles. This one will use vertices 0, 1, and 3 to create one triangle, and 1, 2, and 3 to create the second.

Next, as a property, add the line `int ElementBufferObject` below where you added the VertexBufferObject.

Now, in OnLoad, below where you initialize the VertexBufferObject, we initialize the ElementBufferObject in much the same way:

```cs
ElementBufferObject = GL.GenBuffer();
GL.BindBuffer(BufferTarget.ElementArrayBuffer, ElementBufferObject);
GL.BufferData(BufferTarget.ElementArrayBuffer, indices.Length * sizeof(uint), indices, BufferUsageHint.StaticDraw);
```

It's almost exactly the same as how you use the VBO!

The EBO is now ready to go. Down in OnRenderFrame, replace the call to `DrawArrays` with the following:

```cs
GL.DrawElements(PrimitiveType.Triangles, indices.Length, DrawElementsType.UnsignedInt, 0);
```

The arguments are, in order:

- The PrimitiveType. Just like last time, we want to use raw triangles.
- The amount of vertices to draw. We use the length of the indices to draw everything.
- The type of the EBO's elements. Unsigned int.
- The offset of what we want to draw. Since we want to draw everything, we just use 0.

That's all it takes! If you run your program now, you should be able to see that you have a rectangle instead of one triangle.
