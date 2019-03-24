# Camera

In the previous tutorial we discussed the view matrix and how we can use the view matrix to move around the scene (we moved backwards a little). OpenGL by itself is not familiar with the concept of a *camera*, but we can try to simulate one by moving all objects in the scene in the reverse direction, giving the illusion that we are moving.

In this tutorial we'll discuss how we can set up a camera in OpenGL. We will discuss an FPS-style camera that allows you to freely move around in a 3D scene. In this tutorial we'll also discuss keyboard and mouse input and make a custom camera class.

## The camera class



#### NOTES
> These are notes for writing a better tutorial

Camera contains
- View
- Projection
- - SetProjection (Remember on resize)

Drawing/Using the camera

Camera movement
- Movement function
- Inputs
- Movement speed

Rotation
- Rotate function
- Inputs
- Sensitivity
