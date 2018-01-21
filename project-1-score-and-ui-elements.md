#Project 1 - Score and UI Elements

In order for these interacting sprites to feel like a game, we need to keep track of the score, we need to display notification for winning and losing, and and we need a button that will let us start and restart the game.  

These UI / display elements will all be controlled by the GameController class, but in this section, we'll discuss how to create and position UI elements in a scene.

###Canvas
All UI elements are children of the Canvas class.  When the first UI element is added to a scene, in addition, a Canvas gameObject and an EventSystem gameObject are also added to a scene, these are required for UI elements to be displayed and have interactivity.  

###Canvas: ScreenSpace-Overlay
When the Canvas is added to a scene, by default, it's layout is offset from the main camera.  For our purposes, it is easiest to configure the Canvas so that it's rendered in the same position as the regular game scene.  To do that, we need to change the Render-Mode of the Canvas, we'll change it from Screen-Space Overlay to Screen-Space Camera, and we'll select the main camera as the camera for the canvas.    

![](Screenshot 2016-02-15 20.14.52.png)

###UI-Button:
