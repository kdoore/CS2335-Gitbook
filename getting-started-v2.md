# Getting Started - Project 1

#### Create a new 2D project in Unity

After downloading and installing the Unity Editor, Open the Unity application and create a new **2D** project.

#### Download Some Free Game Art Assets:

**GameArt 2D Free Assets**  
[Winter Tile Set](https://www.gameart2d.com/winter-platformer-game-tileset.html)

[Cat and Dog Sprites](https://www.gameart2d.com/cat-and-dog-free-sprites.html)

**Planet Cute** [Planet Cute Zipfile ](https://utdallas.box.com/v/planet-cute-zipfile) - from: [http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html](http://www.lostgarden.com/2007/05/dancs-miraculously-flexible-game.html)

Images Below are from Planet Cute: Star, CatGirl, EnemyBug:

![](/star.png)![](/Character Cat Girl.png)![](/assets/Enemy Bug.png)

#### Add Sprites to Unity Project Assets

1. Create a new folder: _**textures**_ in the Project / Assets windows by right-clicking and selecting new folder 
2. Add .png files to your Unity project by dragging each file into the textures folder in your project / assets window. 
3. When you select an image in your assets folder, the Inspector should show that the **texture-type is Sprite \(2D and UI\)**
4.  You can also use the 3D basic shape-primitives as place-holders for prototyping 2D gameObjects.  [Unity Manual - primitive objects](https://docs.unity3d.com/Manual/PrimitiveObjects.html)

####Create a 2D Sprite GameObject - Player:

Select **GameObject** in the top menu, select: **GameObject &gt; 2D Object &gt; Sprite,** this should add a new item in the **Hierarchy panel**.  Select this new gameObject in the Hierarchy Panel,  Now, the **Inspector panel** should show details of this selected gameObject, **rename** the gameObject by typing the 'Player' or  into the top textbox in the Inspector panel. To **select a sprite** image \(from the assets tab\) to associate with this new 2D object, you will select/press the small circle icon to the right of the Sprite Renderer Component's **Sprite** field. This will create a new pop-up window that shows all possible items in the assets that can be selected as the sprite to be rendered.  

![](/assets/SelectSpriteImg.png)

###Physics2D
The Physics2D Engine manages movement and collisions for 2D gameObjects.  We'll add Rigidbody2D for gameObjects that need to move in a scene.  We'll add Collider2D components to give gameObjects a physical boundary for collisions with other gameObjects.

![](/assets/Screen Shot 2019-01-17 at 2.47.49 PM.png)

####RigidBody2D
This component puts a gameObject's movement under control of the Physics2D engine.  The gameObject's movement should be managed using Physics2D methods which can add forces to the object, change the velocity, etc.  The Physics2D engine should be used to determine a moving gameObject's changing position values. In contrast, moving a gameObject by changing the transform position values is not recommended.  The Rigidbody2D component manages movement of the gameObject's colliders in an optimized manner. You will need to expand the Constraints section so you can FreezeRotation for the Z axis as shown in the image above.
**Any gameObject that will be moving in a scene should have a Rigidbody2D component. **

####Collider2D Components
This component gives sprites a physical presence, it gives them a collision boundary, it allows for collision interactions with other gameObjects that also have Collider2D component.  GameObjects that will have any movement during gameplay should have an attached Rigidbody2D component which also manages movement of the object's Collider2D's.  Multiple colliders can be used on a single gameObject, if the boundaries overlap, they will act as a composite collider for the gameObject.  Sometimes you may also have a small collider nested within a larger collider, where the outer-collider can function as a trigger, but the inner collider will interact with the floor's collider to keep the object within the scene.

* **IsTrigger** - when checking this checkbox: then this collider **will not display** collision interaction behavior, but it will cause the **OnTriggerEnter2D** event to be exectued. This is often used for sensing movement into zones, or for for objects that will be destroyed.   

###Steps to Create and Configure 2D Sprite GameObjects:

1. **Background** Create a 2D Sprite GameObject by selecting a sprite that can be used as a background image - scale to fill the camera's viewport. Objects higher in the Hierarchy panel are rendered behind gameObjects lower in the Hierarchy, however, 

**Sorting Layers** is the preferred method for ordering sprite layering for rendering. [Unity Tutorial Video on SortingLayers](https://unity3d.com/learn/tutorials/topics/2d-game-creation/sorting-layers)

2. **Player** Create a 2D Sprite GameObject
    * Select desired sprite from the Assets for the SpriteRenderer component's sprite field (as detailed above).
    * Add **Physics2D &gt; RigidBody2D** Component - this is required for objects that will have **movement**, physics forces should be used to give movement to gameObjects.
    
    * Add **Physics2D &gt; Collider2D** Components - select 1 or more Colliders to fit your gameObject.  Select the Edit-collider button to change the size of the collider, manually change the x or y offset.
   
   - Create C\# Script:  **PlayerController** - Simple Version \(script provided below
   
  - Add PlayerController as a Script Component to Player GameObject  

###Other GameObjects:

1. ** Pickup:**
Create Several 2D Sprite Game Objects: \(objects for the player to interact with - we'll call these **PickUp **objects \)
    - Add one or more **Physics2D &gt; Collider2D** components to create a collision-boundry for each object
    - Select **IsTrigger** checkbox for these collider components.
    - [Create C\# Script:  **PickUp **](/pickup_items.md)
        - Add PickUp as a Script Component to each PickUp gameObject
    - Create a **Prefab** from each type of PickUp object

2. **Floor** - Create an empty GameObject, attach a  BoxCollider2D, edit the collider so it is a wide rectangle, move toward the bottom of the screen.

3. **GameData:** [Create C\# Script: **GameData **
](/gamedata-simple.md) 
         - Add GameData as a Script Component to new Empty GameObject named: **GameManager**

4.  **PlayerStats UI-Display:** [ Create c\# Script: **PlayerStats**
](/playerstatsdisplay-version1.md) - Add Script to a UI-Panel that has 2 children:  UI-Text ScoreText, UI-Text HealthText.

5.  **Utility**  [Create C\# Script: Utility](/utility_-_static_class.md) - This script does not inherit from MonoBehaviour, it will not be put on a gameObject in any scenes, it's code will be accessible to other scripts using the class name, and then the desired method to be exectued:  `Utility.ShowCG( somePanel );`

6. **Spawner** [Create C\# Script: Spawner](/project-3/simple-spawner.md) Add Script to an Empty GameObject:  **Spawner**

7.  **MiniGameManager**  Create C\# Script:  MiniGameManager, Add Script to Empty GameObject:  **MiniGameManager**


##Links To Code

###Player Controller Script - version 1

See Link [PlayerController - Version1:](/player_gameobject.md)

###GameData - Version1

See link:[ GameData-Version1:](/gamedata-simple.md)



### PickUp - version1

See Link: [PickUp PreFabs](/pickup_items.md)



