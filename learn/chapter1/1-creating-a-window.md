# Creating a window

Unlike OpenGL, OpenTK comes with its own windowing system. This tutorial will teach you how to use it. Go ahead and make a C# console project in your favourite IDE, and make a new file called Game.cs, and add the following using directives:

```cs
using OpenTK;
using OpenTK.Graphics;

namespace YourNamespaceHere
{
    public class Game
    {

    }
}
```

Now we have a blank class. Time to turn it into a `GameWindow`. To do that, simply extend `GameWindow`, like so:

```cs
public class Game : GameWindow
```

Now your class is operable as a basic window. This is good, but this limits you customizations. There are plenty of ways you can customize your `GameWindow`, but in this tutorial we're going to create a simple constructor to let us set the width, height, and title of the window. We do this by overriding a `base` constructor included with OpenTK:

```cs
public Game(int width, int height, string title) : base(width, height, GraphicsMode.Default, title) { }
```

Your `GameWindow` is ready to go! Now all you have to do is invoke it from your program. You should've had a Program.cs automatically created when you made the program earlier in the tutorial, as well as a `Main` function. To open your window when the program starts, we must:
- Create an instance of your `Game` class
- Start all the pumps by calling the `Run` function
- After we're done, dispose the `Game`.

```cs
// This line creates a new instance, and wraps the instance in a using statement so it's automatically disposed once we've exited the block.
using (Game game = new Game(800, 600, "LearnOpenTK"))
{
    //Run takes a double, which is how many frames per second it should strive to reach.
    //You can leave that out and it'll just update as fast as the hardware will allow it.
    game.Run(60.0);
}
```

Plug that code into your `Main` function, and go ahead and build & run your program! You now have a blank window, great job! However, the only way you can close your window is by using the cross (`X`) button or Alt+F4. We don't want that, Let's do a little bit of input handling!

`GameWindow` has plenty of methods that you can override to add all sorts of functionality to your game. You can look at the "API" section of this website to look at them all, but in this case the one we're interested in is `OnUpdateFrame`.

By just typing `override OnUpdateFrame` your IDE should be able to generate a function like this one:

```cs
protected override void OnUpdateFrame(FrameEventArgs e)
{
    base.OnUpdateFrame(e);
}
```

It's really simple to detect key presses! OpenTK has a class called `KeyboardState` with an `IsKeyDown` method, which returns `true` if a key is pressed. For example, `KeyboardState.IsKeyDown(Key.Escape)` will only return `true` if the escape (`Esc`) key is pressed.

We want to exit when the escape key is pressed, and with the above information is in mind, exiting when the escape key is pressed is as easy at this:

```cs
KeyboardState input = Keyboard.GetState();

if (input.IsKeyDown(Key.Escape))
{
    Exit();
}
```

Now, the function should look like this:

```cs
protected override void OnUpdateFrame(FrameEventArgs e)
{
    KeyboardState input = Keyboard.GetState();

    if (input.IsKeyDown(Key.Escape))
    {
        Exit();
    }

    base.OnUpdateFrame(e);
}
```

### Review

In this tutorial, we created a blank window that listens for the escape key being pressed and exits when it is. In the next tutorial, we'll draw a triangle on this blank window you've just created. That's all for now!
