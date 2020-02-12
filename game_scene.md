# Game Scene

To build on our project, we will add some scenes with game activities.  This will provide us with an opportunity to write code that deals with dynamic game objects that interact with each other.  We'll create a very simple 2D game that has a simple sprite character that can collide with objects to generate some dynamic game data that we'll want to display in other scenes.

So far all of our scenes have been UI scenes which relied on using Canvas GameObjects.  In these scenes, we have the Canvas Component set to Screen Space Camera and we have Selected the MainCamera as the RenderCamera.  In addition, you can use the CanvasScaler to adjust how your Canvas adjusts to match the screen Size of your application, as shown in the image below. Also, you can see that we've selected the Foreground Sorting layer for the Canvas, details about how to create custom sorting layers in discussed [ Custom Sort Layers](player_gameobject.html#custom-sorting-layers) 

![](Screenshot 2015-11-12 23.46.46.png)

