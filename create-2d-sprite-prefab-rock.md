#Create 2D Sprite Prefab GameObject

In this section, we'll create a 2D sprite gameObject that will have 2D physics configured to act on the object.  Once the gameObject has been configured, then we'll create a Prefab from the gameObject, this allows us to store the custom gameObject as a named asset which is stored independently from a specific scene.  One use of prefabs is that they can be used for dynamically generating gameObjects at run-time via code.  

###Steps to create a Sprite Asset:
###Option 1: Create a place-holder sprite asset:
 In the project / asset panel, right click and select:
Create > Sprite > Select the sprite shape, such as 'diamond'.
This sprite can be used as the Sprite for a SpriteRenderer component on a 2D Sprite gameObject.  The color can be set using the colorSelector once the sprite has been added to a 2D Sprite gameObject.  This color can also be used to tint an image, however,  remember to select white as the color to show an un-tinted image used as a sprite.

###Option 2: Importing an external image file:  
   1.  Open a finder / explorer window on your system, browse to the folder that contains your image file.
   2.  Drag the image file directly into the Project / Asset panel at the bottom of your Unity window.  
   3. If you have configured your game as a 2D game, then the sprite should be ready to use.  Otherwise, with the image selected in your project panel:  in the Inspector panel, select the options shown in the image below: 
   
   ![](/assets/Screen Shot 2018-01-25 at 12.22.37 PM.png)
   
###Animation: How to Create Sprite Assets.
The animation below shows how to create a place-holder sprite, and how to import an external image, after locating the image file in an explorer or finder window.  The place-holder sprite is set as the sprite for the basket gameObject.

   ![](http://g.recordit.co/TW4j9r8eDV.gif)
   
###Animation: How to use .png image asset to replace a placeholder Sprite for a GameObject.  
In the animation below, we change the sprite used for the basket from the square placeholder sprite, so that it's now using  a .png sprite file that was dragged into the asset folder.  After making the change, we reset the color so it's not tinting the image.

![](http://g.recordit.co/NT0BVSfR53.gif)

###Steps to create a 2D-Sprite GameObject / Prefab

1.  Select from the top menu: GameObject > 2D > Sprite
2.  Select the gameObject in the Hierarchy Panel, change the name, in this case: Rock.
3.  With the Rock gameObject still selected in the hierarchy, do the following steps: 
4.  Check that the gameObject's transform.position.z value = 0;
5.  Select a Sprite for the SpriteRenderer Component, do this by selecting the small circle icon in the SpriteRenderer component, this brings up the Asset / Scene pop-up....Make sure to select a sprite from the Asset tab (not the Scene tab).  This sprite can either be a placeholder sprite, or an imported sprite.  
6. Add the following Components in the Inspector Panel, configure as detailed below:
7. Rigidbody2d - dynamic type selected
8. Collider2D - Select CircleCollider2D, BoxCollider2D, or PolygonCollider2D, edit points if necessary.
9. Add a Script component - add either Rock.cs or PickUp.cs script
10. Set the Script component's pointValue to some negative value in the inspector
11. Add Tag: "Rock" to the gameObject (top of inspector panel).   (If the tag doesn't exist, select 'addTag', and create a new tag: "Rock", then add to your gameObject
12. Once the gameObject has been configured as shown in the animation below, drag it to your Project / Assets panel to turn it into a prefab.  The text should show as blue once it is a prefab.


The animation clip below shows the above steps to create a 2D sprite gameObject, Rock, that can be used as a dropped object in our game.
#Animation Showing how to create a Prefab for the Rock GameObject following the steps above.
![](http://g.recordit.co/xikYSfgGYo.gif)


