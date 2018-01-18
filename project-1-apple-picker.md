Project 1 - Apple Picker Game Variation

For project 1, we will make a modified version of the first digital game prototype from the course textbook.  The Apple Picker Game is introduced in Chapter 16, where flow charts show the game logic for each object in the game. You will be required to use sprites in your projects, and the theme of your game must be different than the apple-tree theme the author uses.  In addition, instead of dropping just 1 type of object, your game must drop 3 types of objects, 2 objects that generate points, and 1 type of object that subtracts points from the user's score.  In addition, we'll need to have a splash-screen that will allow starting of the game, and a way to win and lose the game, with popups showing that the user has won or lost the game.

Required Customizations:
    1. Customize the theme. Use sprites to show theme.
    2. Have 3 types of falling objects - 2 good, 1 bad
    3. Have a start-button / splash-screen
    4. Have win / lose states 
    5. Show win / lose UI panel
    6. Restart Game Button

###Physics2D 
There are a few notable differences between our game and the author's game, the main one is that we're creating a 2D game using 2D sprites, whereas he used 3D shape primitives with a 2D orthographic camera.  The difference requires that we always use the Physics2D components for our gameObjects, and that we write our code to reflect that we're using the Physics2D components.  In addition, the author seems to skip adding colliders to his gameObjects, so we'll need to make sure to add colliders to the apples and the baskets, so the Physics2D engine can determine when the objects overlap.
 
 ###Create a New 2D Project
Start by creating a new Unity project - select 2D as the project-type, and save the default scene as Scene0.  Menu: File>Save Project.    

###Camera Component Configuration
 Camera Configuration - Select the Main Camera GameObject, then in the inspector, change the size value from 5 to 10.  Make sure the camera is set to orthographic instead of perspective.
 
###Create PlaceHolder Sprite Assets:
We can use sprite primitives as placeholders for sprites, to do that, in the Project panel, right click and select: Create> Sprite>Circle.  This creates a white circle icon in the asset panel.  Create a folder named Sprites, and keep all of your sprites inside this folder. Also create a Square sprite. 

 ###Create GameObjects
 From the top menu, select GameObject > 2D > Sprite.  This puts a gameObject in the Hierarchy panel, rename this to Apple, and set the Sprite attribute in the Sprite Component to be the Circle sprite, do this by selecting the small circle to the right of the Sprite field in the Sprite component, it provides a way to select items from your Asset folder. Also, repeat this to create a Crate gameObject.  For the AppleTree, this is created from 2 sprite primitives, so you'll create 3 sprites for this, the first is an empty gameObject, name this one AppleTree, create 2 additional Sprite GameObjects, name them leaves and trunk. In the hierarchy panel, drag the leaves in  on top of the AppleTree, this should make it a child-object of the tree, do the same for the trunk.  Set sprites for the leaves and trunk so they resemble a tree, move these sprites.  When you move the parent, AppleTree, gameObject, both child objects should move as a single object.   
 
###Configure a Rigidbody2D Component for AppleTree, Crate
With the AppleTree selected in the Hierarchy panel, in the Inspector Panel, select the 'Components' button, scroll down to find the Physics2D option, select it and scroll down in the selection window to select Rigidbody2D, this should add a new section to the Inspector panel for the AppleTree. Within this new Rigidbody2D component, select the top button and change it from Dynamic to Kinematic. This means that we'll be overriding the Physics Engine, and we'll be moving this object by modifying it's Transform component in a script. Repeat these same steps for the Crate GameObject, but in addition, you must also add a Physics2D > BoxCollider2D to the crate, so it can sense collisions with Apple objects. 

###Conf
