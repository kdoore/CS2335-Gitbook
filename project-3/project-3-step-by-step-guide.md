#Project 3 - Step - by - Step Guide

Follow the guidelines below to complete project 3.

###1. Create Scene and StateX.cs script for MiniGame
Follow the same steps used in [Project 2](https://kdoore.gitbooks.io/cs-2335/content/project-2-create-new-scene-and-state.html), to create a new scene and a new state.cs script.

1.  Create a new scene, MiniGame, in a copy of Project 2. (You can name your scene anything you want, I'll refer to mine as MiniGame.  
 
2.  Create a state script, MiniGame.cs, which implements the IStateBase interface.  Here is a link to code you can use for that file: [MiniGame code](/minigame_-_view.md)

3.  Follow the steps in Project 2, to configure the canvas, so it uses [screen-space-camera](/screen-space_canvas.md) rendering mode.

###2. Player GameObject
You need a **Player GameObject**, where movement is controlled by keyboard input.  This gameObject is similar to the Basket GameObject in the ApplePicker game.  

 1.  Create a **2D sprite** gameObject, name it `Player`. 
 2.  Create a script:  **PlayerController.cs**, using the example [PlayerController.cs](/project-3/playercontroller.md) script as a starting point.  You will need to modify this script to fit your gameplay scenario.
 
 3.  Add the following **Components** to your **Player** gameObject:
  - **PlayerController.cs**  script component
  - **Rigidbody2d**, use default settings, including Dynamic Body-type.  
      - Modify, Select: Constraints: Freeze Rotation Z
  - **Collider2D**, default settings
  - **Animator Component** (if you choose to animate your player)
  
  - Set the GameObject: Layer to `Player`
  - Set the SpriteRenderer Sorting Layer to `Foreground`
  - Make sure the Transform.Position.Z value is 0
  
 4.  Customize the `PlayerController.cs` script as needed.  The Script has 4 main functional sections:
  - **Keyboard input**: Check for keyboard input, set variables such as jump, inputX based on keyboard input. 
  - **Physics Movement:** This movement occurs in the FixedUpdate method.  
  - **Animator Controller:** Use Hero-State enums to send parameter values into the Animator Controller.
  - **OnTriggerEnter2D( )**  You will add code for logic that should be exectued when the player collides with other GameObjects, depending on the Tags:  Collectible, or Hazard.
  5. If you will use your Player in multiple scenes, save your Player as a prefab, then save your project.
  
###3. PickUp Prefabs - Create 4:
You are required to have a minimum of 4 different types of PickUp objects for the player to interact with.  

For each of your 4 required PickUp type objects:   
1. Create a 2D sprite gameObject.
2. Set the Transform.Position values to x: 0,y: 0,z: 0 
2. Modify the PickUp.cs script file as needed:  Change the `PickupType` enums to match your game's theme and objects.
3.  Set the SpriteRenderer Sorting Layer to `Foreground`.
4.  If you will be using a Spawner, drag your PickUp around the screen to determine the range of X and Y values where this object can be spawned (for use in configuring the Spawner)
5.  Add the following **Components** to your PickUp GameObject:
 - Collider2D:  with: `isTrigger` checked as true
 - Tag:  Hazard or Collectible
 - [PickUp.cs](/pickup_items.md) script
    - Set: PickupType from the dropdown
    - Set: value to assign point value
     
Create a prefab of this object by dragging into your Resources folder.  
Remove all prefab objects from the game-scene. 
Save your project.

###Spawner 
You are not required to use a spawner. But using one or more spawners may be the easiest method to have objects for the player to interact with, for each mini-game level.

To create a basic spawner:
1.  Create an empty gameObject, name it Spawner.
2.  Check the Transform.Position values, make sure the Z value is 0.  
3.  Move the Spawner off-screen. If you use the spawner to spawn PickUp objects that will fall due to gravity, then move the spawner to the top of the scene so you can use the Spawner's transform Y and Z values to initialize the spawned object's initial position.
4.  Determine the range of X values that are valid for initializing the position of spawned, by dragging a PickUp object to the left and right borders of the scene and noting the values for the Transform.Position.X.
5.  Repeat the previous step to determine range of Y values where an object can be spawned.
5.  Create a C# script: [Spawner.cs](/project-3/simple-spawner.md).   Attach the script to the Spawner GameObject.
6.  In the inspector, select Prefabs from your assets folder to populate the GoodPrefab, BadPrefab variables.
7.  Customize variable values in the Spawner.cs script so they correspond with your game's requirements.

    
###GameData
GameData functions as a singleton database for storing all data for the game.  Since GameData is a singleton, it will be an active object in all gameScenes, so there should not be any scene-specific code or dependencies within the GameData.cs code.  Instead, we'll use custom events to notify interested objects in any scene when the data has been updated.  UnityEvents provides the simplest way to implement custom events, so that is the first approach we'll use in GameData.

1.  Create a C# script:  GameData.cs.
2.  Use code from: [GameData](/project-3/gamedata-with-unityevent.md) to populate the class code in Visual Studio. 
3.  In your project's first scene, (BeginScene), add the GameData script component to the GameManager gameObject.  
4.  In your project's MiniGame scene, create a new empty  GameObject, GameManager, and add the GameData script component to the GameManager gameObject.
5.  You will. start customizing your other scripts, so they will use GameData for storing data created by gameplay.

###LevelManager
If you use a single scene for your entire MiniGame, then we can create a LevelManager gameObject and Script component to manage the gameplay logic, such as determining the current level.


   
  
  