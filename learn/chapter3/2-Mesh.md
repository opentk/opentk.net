# Mesh

 With Assimp we can load many different models into the application, but once loaded they're all stored in Assimp's data structures. What we eventually want is to transform that data to a format that OpenGL understands so that we can render the objects. We learned from the previous chapter that a mesh represents a single drawable entity, so let's start by defining a mesh class of our own.

 Let's review a bit of what we've learned so far to think about what a mesh should minimally have as its data. A mesh should at least need a set of vertices, where each vertex contains a position vector, a normal vector, and a texture coordinate vector. A mesh should also contain indices for indexed drawing, and material data in the form of textures (diffuse/specular maps).

 Now that we set the minimal requirements for a mesh class we can define a vertex in OpenGL:

```cs
public struct Vertex {
  public Vector3 Position;
  public Vector3 Normal;
  public Vector2 TexCoords;
}
```

 We store the id of the texture and its type e.g. a diffuse or specular texture.

 Knowing the actual representation of a vertex and a texture we can start defining the structure of the mesh class:

```cs
public class Mesh {
  public readonly int VAO;
  public readonly int indicesCount;
  public Mesh(Span<Vertex> vertices, Span<int> indices);
}
```

As you can see, the class isn't too complicated. In the constructor we give the mesh all the necessary data, we initialize the buffers. It basically became a storage class.

The function content of the constructor is pretty straightforward and is basically the same as we had with drawing a rectangle or a cube. 

We now have large lists of mesh data that we can use for rendering, just not explicitly giver, as we had before. We do need to set up the appropriate buffers and specify the vertex shader layout via vertex attribute pointers. By now you should have no trouble with these concepts.

Create and populate vertecies and index buffers, connect it to a `VAO`, here connecting it to a `VAO` is done by `GL.BindVertexArray(VAO);`, all buffers that are used after are automatically binded to this `VAO`


```cs
public Mesh(Span<Vertex> vertices, Span<int> indices){
  indicesCount = indices.Length;

  VAO = GL.GenVertexArray();
  int VBO = GL.GenBuffer();
  int EBO = GL.GenBuffer();

  GL.BindVertexArray(VAO);
  GL.BindBuffer(BufferTarget.ArrayBuffer, VBO);
  GL.BufferData(BufferTarget.ArrayBuffer, vertices.Length * Unsafe.SizeOf<Vertex>(), ref MemoryMarshal.GetReference(vertices), BufferUsageHint.StaticDraw);

  GL.BindBuffer(BufferTarget.ElementArrayBuffer, EBO);
  GL.BufferData(BufferTarget.ElementArrayBuffer, indices.Length * sizeof(int), ref MemoryMarshal.GetReference(indices), BufferUsageHint.StaticDraw);

  // Vertex positions
  GL.EnableVertexAttribArray(0);
  GL.VertexAttribPointer(0, 3, VertexAttribPointerType.Float, false, Unsafe.SizeOf<Vertex>(), Marshal.OffsetOf<Vertex>(nameof(Vertex.Position)));
  // Vertex normals
  GL.EnableVertexAttribArray(1);
  GL.VertexAttribPointer(1, 3, VertexAttribPointerType.Float, false, Unsafe.SizeOf<Vertex>(), Marshal.OffsetOf<Vertex>(nameof(Vertex.Normal)));
  // Vertex texture coords
  GL.EnableVertexAttribArray(2);
  GL.VertexAttribPointer(2, 2, VertexAttribPointerType.Float, false, Unsafe.SizeOf<Vertex>(), Marshal.OffsetOf<Vertex>(nameof(Vertex.TexCoords)));

  GL.BindVertexArray(0);
}
```
 Nothing special going on here. Let's delve right into the SetupMesh function now.

## **Rendering**

The last function we need to define for the `Mesh` class to be complete is its `Draw` function. Before rendering the mesh, we will need to set which `Shader` we are using and bind the appropriate textures before calling `GL.DrawElements`. For simplicity, within the `Draw` function itself we will only bind the `VAO`, call `GL.DrawElements`, then unbind the `VAO`.

```cs
public void Draw() {
	// draw mesh
  GL.BindVertexArray(VAO);
  GL.DrawElements(PrimitiveType.Triangles, indices.Length, DrawElementsType.UnsignedInt, 0);
  // unbind the `VAO`
  GL.BindVertexArray(0);
}
```

You can find the full source code of the `Mesh` class here.

The `Mesh` class we just defined is an abstraction for many of the topics we've discussed in the early chapters. In the next chapter we'll create a model that acts as a container for several mesh objects and implements Assimp's loading interface.