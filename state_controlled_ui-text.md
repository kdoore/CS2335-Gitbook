#State-Controlled UI GameObjects

For the Number game, we want to create a graphical version of the game where the prompts are displayed on the game screen.  We will work in 2D mode, and we need to use a UI game object to display  the text.  This will involve a few steps in order to allow our game script file to modify the UI-Text elements during the game. When we add a UI Text element to our scene, it also creates a Canvas and an Event Object in the scene.  We won't use the Event object in this project, but we will use the canvas object.  
###Canvas
The canvas object is the container for any UI-Text objects in our scene, and our Text object's transform object is defined relative to the canvas since the Text is a child of canvas in the hierarchy panel. 

    -  Add a UI -> Text gameObject to the scene
    -  Set the color, position, fontSize of the Text in the inspector
    
    
     




