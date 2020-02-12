#Sorting Layers to Control Sprite Rendering Order

In this section, we'll look at how to create Sorting Layers to control the layer ordering of sprite rendering.

###Sorting Layers to Control Sprite Rendering Order
[Unity Video on Sorting Layers](https://unity3d.com/learn/tutorials/topics/2d-game-creation/sorting-layers)

Sorting Layers provide control over layer ordering of sprite rendering. The animation below shows how to add a sprite to function as a background image.  Next, on the sprite component for that background image, select the SortingLayer dropdown, choose to create a Sorting Layer.  Create 2 Sorting Layers (we'll create additional layers for more complicated games).  Create a Background and a Foreground sorting layers.  The order of these items in the  Sorting Layer list determines the relative render ordering of gameObjects.  Items higher in the list: Background, are rendered first, with items lower on the list being rendered later, or closer to the viewer.
For all sprite gameObjects and prefabs, go through and add these to the Foreground sorting layer so they will appear in front of the background image

###Animation to Create and Apply Sorting Layers

![](http://g.recordit.co/3cOESi4plt.gif)

###Sorting Groups
Sorting Groups allow Sorting Layers to be added to any Hierarchy of GameObjects, by default, sorting layers are already available for default hierarchy  objects.  The canvas provides an example of a hierarchy that does not have sorting layers for child objects by default, this can be changed by adding a Sorting Group component to the parent Canvas gameObject.



