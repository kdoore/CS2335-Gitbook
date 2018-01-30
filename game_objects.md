# Game Objects

The UnityEngine provides methods that allow easy message communication between GameObjects and components some of these methods are FindGameObject(), AttachListener(), OnTriggerEnter2D(), etc.

Now that we are starting to work with GameObjects that aren't UI elements, we need to understand how we can define custom sorting layers to control rendering layer ordering and also how to define custom tags to help easily identify gameObjects within a game scene.

[2D Sprites Editing Tutorial](https://www.youtube.com/watch?v=tp9PRN2TMy0)

###Layers:
Layers provide a means to control physics interactions between objects.  


###Tags:
Tags provide a method to add categories to gameObjects that can be used to identify gameObjects within a custom script.  Tags are also created using the Layer editor, by selecting the 'Edit Layers' option, you are provided with an interface where you can create custom tags.  Then, a tag can be set for a gameObject at the top of the inspector panel for that gameObject.

###Sprite Renderer Component - Sorting Layers:
Sorting layers provide a method to control the layer order of sprite rendering.  Sorting Layer are is attribute of the Sprite Renderer component for 2D sprite gameObjects.  Sorting Layers are created using the Layer inspector panel, at the upper left of the editor window, see image below.  Select the bottom choice 'Edit Layers' to add new Sorting Layers to your game. Although the Layer editor is the same for Layers, Tags, and Sorting Layers, it's important to realize that Layers and Tags refer to the GameObject, while Sorting Layers refer only to the Rendering Components.  There is no ability to set Sorting-layer for individual UI elements, there is a single Sorting-Layer that can be set for the entire Canvas.  So, to control layer ordering of Canvas UI elements, the Hierarchy order controls layering, lower elements in the Hierarchy are considered to be closer to the camera.

![](/assets/SortingLayer.png)





