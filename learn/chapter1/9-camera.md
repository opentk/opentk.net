# Camera
In the previous tutorial we discussed the view matrix and how we can use the view matrix to move around the scene (we moved backwards a little). OpenGL by itself is not familiar with the concept of a *camera*, but we can try to simulate one by moving all objects in the scene in the reverse direction, giving the illusion that **we** are moving.

In this tutorial we'll discuss how we can set up a camera in OpenGL. We will discuss an FPS-style camera that allows you to freely move around in a 3D scene. In this tutorial we'll also discuss keyboard and mouse input and finish with a custom camera class.

## Camera/View space
When we're talking about camera/view space we're talking about all the vertex coordinates as seen from the camera's perspective as the origin of the scene: the view matrix transforms all the world coordinates into view coordinates that are relative to the camera's position and direction. To define a camera we need its position in world space, the direction it's looking at, a vector pointing to the right and a vector pointing upwards from the camera. A careful reader might notice that we're actually going to create a coordinate system with 3 perpendicular unit axes with the camera's position as the origin.

![Camera Axes](img/9-camera_axes.png)

### 1. Camera position
Getting a camera position is easy. The camera position is basically a vector in world space that points to the camera's position. We set the camera at the same position we've set the camera in the previous tutorial:


```cs
Vector3 Position = new Vector3(0.0f, 0.0f, 3.0f);
``` 
> Don't forget that the positive z-axis is going through your screen towards you so if we want the camera to move backwards, we move along the positive z-axis.
### 2. Camera direction
The next vector required is the camera's direction e.g. at what direction it is pointing at. For now we let the camera point to the origin of our scene: **(0,0,0)**. Remember that if we subtract two vectors from each other we get a vector that's the difference of these two vectors? Subtracting the camera position vector from the scene's origin vector thus results in the direction vector. Since we know that the camera points towards the negative z direction we want the direction vector to point towards the camera's positive z-axis. If we switch the subtraction order around we now get a vector pointing towards the camera's positive z-axis:


```cs
Vector3 cameraTarget = Vector3.Zero;
Vector3 cameraDirection = Vector3.Normalize(cameraPos - cameraTarget);
```
> The name direction vector is not the best chosen name, since it is actually pointing in the reverse direction of what it is targeting.
### 3. Right axis
The next vector that we need is a *right* vector that represents the positive x-axis of the camera space. To get the *right* vector we use a little trick by first specifying an *up* vector that points upwards (in world space). Then we do a cross product on the up vector and the direction vector from step 2. Since the result of a cross product is a vector perpendicular to both vectors, we will get a vector that points in the positive x-axis's direction (if we would switch the vectors we'd get a vector that points in the negative x-axis):


```cs
Vector3 up = Vector3.UnitY
Vector3 cameraRight = Vector3.Normalize(Vector3.Cross(up, cameraDirection));
```
### 4. Up axis
Now that we have both the x-axis vector and the z-axis vector, retrieving the vector that points in the camera's positive y-axis is relatively easy: we take the cross product of the right and direction vector:


```cs
Vector3 cameraUp = Vector3.Cross(cameraDirection, cameraRight);
```
With the help of the cross product and a few tricks we were able to create all the vectors that form the view/camera space. For the more mathematically inclined readers, this process is known as the [Gram-Schmidt](https://en.wikipedia.org/wiki/Gram%E2%80%93Schmidt_process) process in linear algebra. Using these camera vectors we can now create a ***LookAt*** matrix that proves very useful for creating a camera.

## Look At
A great thing about matrices is that if you define a coordinate space using 3 perpendicular (or non-linear) axes you can create a matrix with those 3 axes plus a translation vector and you can transform any vector to that coordinate space by multiplying it with this matrix. This is exactly what the LookAt matrix does and now that we have 3 perpendiclar axes and a position vector to define the camera space we can create our own LookAt matrix:
![LookAt](img/9-LookAt.png)

Where ***R*** is the right vector, ***U*** is the up vector, ***D*** is the direction vector and ***P*** is the camera's position vector. Note that the position vector is inverted since we eventually want to translate the world in the opposite direction of where we want to move. Using this LookAt matrix as our view matrix effectively transforms all the world coordinates to the view space we just defined. The LookAt matrix then does exactly what it says: it creates a view matrix that *looks* at a given target.

Luckily for us, OpenTK already does all this work for us. We only have to specify a camera position, a target position and a vector that represents the up vector in world space (the up vector we used for calculating the right vector). OpenTK then creates the LookAt matrix that we can use as our view matrix:


```cs 
Matrix4 view = Matrix4.LookAt(new Vector3(0.0f, 0.0f, 3.0f), 
  		   new Vector3(0.0f, 0.0f, 0.0f),
  		   new Vector3(0.0f, 1.0f, 0.0f));
```
The `Matrix4.LookAt` function requires a position, target and up vector respectively. This creates a view matrix that is the same as the one used in the previous tutorial.

## Walk around
It is time to actually procces some player input and make our window responsive! First we need to set up a camera system, so it is useful to define some camera variables at the top of our program:

```cs
float speed = 1.5f;

Vector3 position = new Vector3(0.0f, 0.0f,  3.0f);
Vector3 front = new Vector3(0.0f, 0.0f, -1.0f);
Vector3 up = new Vector3(0.0f, 1.0f,  0.0f);
```
Here we have introduced one new variables along with the ones needed for the **LookAt** function the ***speed*** will help us definining the speed of the camera when we move around. Now the **LookAt** function now becomes:

```cs
view = Matrix4.LookAt(position, position + front, up);
```
First we set the camera position to the previously defined ***Position***. The direction is the current position + the direction vector we just defined. This ensures that however we move, the camera keeps looking at the target direction. Let's play a bit with these variables by updating the ***Position*** vector when we press some keys.

We have already taken a look at how we can get user input inside the ***OnUpdateFrame*** function, now we can expand this to move the camera

```cs
protected override void OnUpdateFrame(FrameEventArgs e)
{
    if (!Focused) // check to see if the window is focused
    {
        return;
    }

    KeyboardState input = Keyboard.GetState();

    //...

    if (input.IsKeyDown(Key.W))
    {
        position += front * speed; //Forward 
    }

    if (input.IsKeyDown(Key.S))
    {
        position -= front * speed; //Backwards
    }

    if (input.IsKeyDown(Key.A))
    {
        position -= Vector3.Normalize(Vector3.Cross(front, up)) * speed; //Left
    }

    if (input.IsKeyDown(Key.D))
    {
        position += Vector3.Normalize(Vector3.Cross(front, up)) * speed; //Right
    }

    if (input.IsKeyDown(Key.Space))
    {
        position += up * speed; //Up 
    }

    if (input.IsKeyDown(Key.LShift))
    {
        position -= up * speed; //Down
    }
}
```
> Note that we also check on the top if the window is focused and return if it is not, this avoids issues when the window is not in focus.

Whenever we press one of the **WASD** keys, the camera's position is updated accordingly. If we want to move forward or backwards we add or subtract the front vector from the position vector. If we want to move sidewards we do a cross product to create a *right* vector and we move along the *right* vector accordingly. This creates the familiar ***strafe*** effect when using the camera. Additionally we also added the ability to fly up (**Space**) or down (**LShift**), this is done the same as up and down, except on the up vector instead of the front.

> Note that we normalize the resulting *right* vector. If we wouldn't normalize this vector, the resulting cross product might return differently sized vectors based on the ***front*** variable. If we would not normalize the vector we would either move slow or fast based on the camera's orientation instead of at a consistent movement speed.

By now, you should already be able to move the camera somewhat, albeit at a speed that's system-specific at which you may need to adjust ***speed***.

## Movement speed
Currently we used a constant value for movement speed when walking around. In theory this seems fine, but in practice people have different processing powers and the result of that is that some people are able to draw much more frames than others each second. Whenever a user draws more frames than another user he also calls ***OnUpdateFrame*** more often. The result is that some people move really fast and some really slow depending on their setup. When shipping your application you want to make sure it runs the same on all kinds of hardware.

Graphics applications and games usually keep track of a ***deltatime*** variable that stores the time it takes to render the last frame. We then multiply all velocities with this ***deltaTime*** value. The result is that when we have a large ***deltaTime*** in a frame, meaning that the last frame took longer than average, the velocity for that frame will also be a bit higher to balance it all out. When using this approach it does not matter if you have a very fast or slow pc, the velocity of the camera will be balanced out accordingly so each user will have the same experience.

OpenTK actually calculates the ***deltaTime*** for us and it is even passed to the ***OnUpdateFrame*** as the event arg ***e***, and we can access it like this `(float)e.Time`

Now that we have ***deltaTime*** we can take it into account when calculating the velocities:

```cs
if (input.IsKeyDown(Key.W))
{
    position += front * speed * (float)e.Time; //Forward 
}

if (input.IsKeyDown(Key.S))
{
    position -= front * speed * (float)e.Time; //Backwards
}

if (input.IsKeyDown(Key.A))
{
    position -= Vector3.Normalize(Vector3.Cross(front, up)) * speed * (float)e.Time; //Left
}

if (input.IsKeyDown(Key.D))
{
    position += Vector3.Normalize(Vector3.Cross(front, up)) * speed * (float)e.Time; //Right
}

if (input.IsKeyDown(Key.Space))
{
    position += up * speed * (float)e.Time; //Up 
}

if (input.IsKeyDown(Key.LShift))
{
    position -= up * speed * (float)e.Time; //Down
} 
```

Together with the previous section we should now have a much smoother and more consistent camera system for moving around the scene:
<video width="600" height="450" loop="">
    <source src="video/9-camera_smooth.mp4" type="video/mp4">
</video>
And now we have a camera that walks and looks equally fast on any system. We'll see the ***deltaTime*** value frequently return with anything movement related.

## Look around
Only using the keyboard keys to move around isn't that interesting. Especially since we can't turn around making the movement rather restricted. That's where the mouse comes in!

To look around the scene we have to change the ***cameraFront*** vector based on the input of the mouse. However, changing the direction vector based on mouse rotations is a little complicated and requires some trigonemetry. If you do not understand the trigonemetry, don't worry. You can just skip to the code sections and paste them in your code; you can always come back later if you want to know more.

### Euler angles
Euler angles are 3 values that can represent any rotation in 3D, defined by Leonhard Euler somewhere in the 1700s. There are 3 Euler angles: *pitch*, *yaw* and *roll*. The following image gives them a visual meaning:

![Camera pitch yaw and roll](img/9-camera_pitch_yaw_roll.png)

The ***pitch is the angle that depicts how much we're looking up or down as seen in the first image. The second image shows the yaw value which represents the magnitude we're looking to the left or to the right. The roll represents how much we roll as mostly used in space-flight cameras. Each of the Euler angles are represented by a single value and with the combination of all 3 of them we can calculate any rotation vector in 3D.

For our camera system we only care about the yaw and pitch values so we won't discuss the roll value here. Given a pitch and a yaw value we can convert them into a 3D vector that represents a new direction vector. The process of converting yaw and pitch values to a direction vector requires a bit of trigonemetry and we start with a basic case:

![Camera triangle](img/9-camera_triangle.png)

If we define the hypotenuse to be of length **1** we know from trigonometry (soh cah toa) that the adjacant side's length is **cos x/h=cos x/1=cos x** and that the opposing side's length is sin **y/h=sin y/1=sin y**. This gives us some general formulas for retrieving the length in both the **x** and **y** directions, depending on the given angle. Let's use this to calculate the components of the direction vector:

![Camera pitch](img/9-camera_pitch.png)

This triangle looks similar to the previous triangle so if we visualize that we are sitting on the **xz** plane and look towards the **y** axis we can calculate the length / strength of the **y** direction (how much we're looking up or down) based on the first triangle. From the image we can see that the resulting **y** value for a given pitch equals sin **θ**:

```cs
front.Y = (float)Math.Sin(MathHelper.DegreesToRadians(Pitch)); // Note that we convert the angle to radians first 
```
Here we only update the **y** value is affected, but if you look carefully you can also that the **x** and **z** components are affected. From the triangle we can see that their values equal:

```cs
front.X = (float)Math.Cos(MathHelper.DegreesToRadians(Pitch))
front.Z = (float)Math.Cos(MathHelper.DegreesToRadians(Pitch))
```
Let's see if we can find the required components for the yaw value as well:

![Camera yaw](img/9-camera_yaw.png)

Just like the pitch triangle we can see that the **x** component depends on the **cos(yaw)** value and the z value also depends on the **sin** of the yaw value. Adding this to the previous values results in a final direction vector based on the pitch and yaw values:

```cs
front.X = (float)Math.Cos(MathHelper.DegreesToRadians(Pitch)) * (float)Math.Cos(MathHelper.DegreesToRadians(Yaw));
front.Y = (float)Math.Sin(MathHelper.DegreesToRadians(Pitch));
front.Z = (float)Math.Cos(MathHelper.DegreesToRadians(Pitch)) * (float)Math.Sin(MathHelper.DegreesToRadians(Yaw));
```
This gives us a formula to convert yaw and pitch values to a 3-dimensional direction vector that we can use for looking around. You probably wondered by now: how do we get these yaw and pitch values?

## Mouse input
The yaw and pitch values are obtained from mouse (or controller/joystick) movement where horizontal mouse-movement affects the yaw and vertical mouse-movement affects the pitch. The idea is to store the last frame's mouse positions and in the current frame we calculate how much the mouse values changed in comparrison with last frame's value. The higher the horizontal/vertical difference, the more we update the pitch or yaw value and thus the more the camera should move.

First we will tell OpenTK that it should hide the cursor and *capture* it. Capturing a cursor means that once the application has focus the mouse cursor stays within the window (unless the application loses focus or quits). We can do this with one simple configuration call:

```cs
CursorVisible = false;
CursorGrabbed = true;
```
After this call, wherever we move the mouse it won't be visible and it should not leave the window. This is perfect for an FPS camera system.

When handling mouse input for an FPS style camera there are several steps we have to take before eventually retrieving the direction vector:

1. Calculate the mouse's offset since the last frame.
2. Add the offset values to the camera's yaw and pitch values.
3. Add some constraints to the maximum/minimum pitch values
4. Calculate the direction vector

The first step is to calculate the offset of the mouse since the last frame. For this we need to store the last mouse positions in the application for that we will use a vector defined in the top of our program.


```cs
Vector2 lastPos
```
Then in the mouse's callback function we calculate the offset (delta) movement between the last and current frame:

```cs
float deltaX = mouse.X - lastPos.X;
float deltaY = mouse.Y - lastPos.Y;
lastPos = new Vector2(mouse.X, mouse.Y);
```

Next we add the offset values to globally declared ***pitch*** and ***yaw*** values:

```cs
yaw += deltaX * sensitivity;
pitch -= deltaY * sensitivity; // reversed since y-coordinates range from bottom to top
```

Note that we multiply the offset values by a ***sensitivity*** value. If we omit this multiplication the mouse movement would be way too strong; fiddle around with the sensitivity value to your liking.

In the third step we'd like to add some constraints to the camera so users won't be able to make weird camera movements (also prevents a few weird issues). The pitch will be constrained in such a way that users won't be able to look higher than **89** degrees (at **90** degrees the view tends to reverse, so we stick to **89** as our limit) and also not below **-89** degrees. This ensures the user will be able to look up to the sky and down to his feet but not further. The constraint works by just replacing the resulting value with its constraint value whenever it breaches the constraint:

```cs
if(pitch > 89.0f)
{
    pitch = 89.0f;
}
else if(pitch < -89.0f)
{
    pitch = -89.0f;
}
else
{
    pitch -= deltaX * camera.Sensitivity
}
```
Note that we set no constraint on the yaw value since we don't want to constrain the user in horizontal rotation. However, it's just as easy to add a constraint to the yaw as well if you feel like it.

The fourth and last step is to calculate the actual direction vector from the resulting yaw and pitch value as discussed in the previous section:

```cs
front.X = (float)Math.Cos(MathHelper.DegreesToRadians(pitch)) * (float)Math.Cos(MathHelper.DegreesToRadians(yaw));
front.Y = (float)Math.Sin(MathHelper.DegreesToRadians(pitch));
front.Z = (float)Math.Cos(MathHelper.DegreesToRadians(pitch)) * (float)Math.Sin(MathHelper.DegreesToRadians(yaw));
front = Vector3.Normalize(front);
```
This computed direction vector then contains all the rotations calculated from the mouse's movement. Since the ***cameraFront*** vector is already included in openTKs ***lookAt*** function we're set to go.

If you would now run the code you will notice that the camera makes a large sudden jump whenever the window first receives focus of your mouse cursor. The cause for the sudden jump is that as soon as your cursor enters the window the ***OnMouseMove*** function is called with an ***XDelta*** and ***YDelta*** position equal to the location your mouse entered the screen. This is usually a position that is quite a distance away from the center of the screen resulting in large offsets and thus a large movement jump. We can circumvent this issue by simply defining a **bool** variable to check if this is the first time we receive mouse input and if so, we dont wanna update the ***pitch*** and the ***yaw***:

```cs
if(firstMouse) // this bool variable is initially set to true
{
    lastPos = new Vector2(mouse.X, mouse.Y);
    firstMove = false;
}
else if (focused) { // check to see if the window is focused
    //...
}
```
The final code then becomes:

```cs
protected override void OnUpdateFrame(FrameEventArgs e)
{
    //Keyboard movement...

    if (FirstMove)
    {
        lastPos = new Vector2(mouse.X, mouse.Y);
        firstMove = false;
    }
    else
    {
        float deltaX = mouse.X - lastPos.X;
        float deltaY = mouse.Y - lastPos.Y;
        lastPos = new Vector2(mouse.X, mouse.Y);

        camera.Yaw += deltaX * camera.Sensitivity;
        if(pitch > 89.0f)
        {
            pitch = 89.0f;
        }
        else if(pitch < -89.0f)
        {
            pitch = -89.0f;
        }
        else
        {
            pitch -= deltaX * camera.Sensitivity
        }
    }
    
    front.X = (float)Math.Cos(MathHelper.DegreesToRadians(pitch)) * (float)Math.Cos(MathHelper.DegreesToRadians(yaw));
    front.Y = (float)Math.Sin(MathHelper.DegreesToRadians(pitch));
    front.Z = (float)Math.Cos(MathHelper.DegreesToRadians(pitch)) * (float)Math.Sin(MathHelper.DegreesToRadians(yaw));
    front = Vector3.Normalize(front);

    base.OnUpdateFrame(e);
}
```
Only one small thing left to do, even though we cannot see the mouse it is still there, and it is actually prohibited from moving out of the viewport. For this reason we need to center it once it has moved. So we can add the following function to fix this behaviour.
```cs
protected override void OnMouseMove(MouseMoveEventArgs e)
{
    if (focused) // check to see if the window is focused  
    {
        Mouse.SetPosition(X + Width/2f, Y + Height/2f);
    }

    base.OnMouseMove(e);
}
```
This just sets the mouse position to be in the center of the window every time it has moved.

Give it a spin and you'll see that we can now freely move through our 3D scene!

## Zoom
As a little extra to the camera system we'll also implement a zooming interface. In the previous tutorial we said the *Field of view* or *fov* defines how much we can see of the scene. When the field of view becomes smaller the scene's projected space gets smaller giving the illusion of zooming in. To zoom in, we're going to use the mouse's scroll-wheel. Similar to mouse movement and keyboard input we have a callback function for mouse-scrolling:


```cs
protected override void OnMouseWheel(MouseWheelEventArgs e)
{
    if (value >= 45.0f)
    {
        _fov = 45.0f;
    }
    else if (value <= 1.0f)
    {
        _fov = 1.0f;
    }
    else
    {
        fov -= e.DeltaPrecise;
    }

    base.OnMouseWheel(e);
}
```
When scrolling, the ***yoffset*** value represents the amount we scrolled vertically. When the ***OnMouseWheel*** function is called we change the content of the ***fov*** variable. Since **45.0f** is the default fov value we want to constrain the zoom level between **1.0f** and **45.0f**.

We now have to upload the perspective projection matrix to the GPU each render iteration but this time with the fov variable as its field of view:

```cs
Matrix4.CreatePerspectiveFieldOfView(MathHelper.DegreesToRadians(fov), AspectRatio, 0.01f, 100f);
```

And there you have it. We implemented a simple camera system that allows for free movement in a 3D environment.
<video width="600" height="450" loop="">
    <source src="video/9-mouse.mp4" type="video/mp4">
</video>

Camera class
In the upcoming tutorials we will always use a camera to easily look around the scenes and see the results from all angles. However, since a camera can take up quite some space on each tutorial we'll abstract a little from the details and create our own camera object that does most of the work for us with some neat little extras. Unlike the Shader tutorial we won't walk you through creating the camera class, but just provide you with the (fully commented) source code if you want to know the inner workings.

Just like the Shader object we create it entirely in a single file. You can find the camera object [here](https://github.com/opentk/LearnOpenTK/blob/master/Common/Camera.cs). You should be able to understand all the code by now. It is advised to at least check the class out once to see how you could create a camera object like this.

> The camera system we introduced is an FPS-like camera that suits most purposes and works well with Euler angles, but be careful when creating different camera systems like a flight simulation camera. Each camera system has its own tricks and quirks so be sure to read up on them. For example, this FPS camera doesn't allow for pitch values higher than **90** degrees and a static up vector of **(0,1,0)** doesn't work when we take roll values into account.
The updated version of the source code using the new camera object can be found [here](https://github.com/opentk/LearnOpenTK/blob/master/Common/Camera.cs).

Exercises
See if you can transform the camera class in such a way that it becomes a true fps camera where you cannot fly; you can only look around while staying on the **xz** plane.
Try to create your own LookAt function where you manually create a view matrix as discussed at the start of this tutorial. Replace openTKs LookAt function with your own implementation and see if it still acts the same.
