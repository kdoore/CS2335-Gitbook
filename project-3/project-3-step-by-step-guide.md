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
  
###3. PickUp Prefabs - Create 4:
You are required to have a minimum of 4 different types of PickUp objects for the player to interact with.  

For each of your 4 required PickUp type objects:   
1. Create a 2D sprite gameObject 
2. Modify the PickUp.cs script file as needed:  Change the `PickupType` enums to match your game's theme and objects.
3.  Add the following **Components** to your PickUp GameObject:
 - Collider2D:  with: isTrigger checked
 - Tag:  Hazard or Collectible
 - PickUp.cs script
    - Set: PickupType from the dropdown
    - Set: value to assign point value
    
   
  
  