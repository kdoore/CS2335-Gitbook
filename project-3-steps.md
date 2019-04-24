#Project 3 - Steps

Project 3 is focuses on Understanding Event-Driven System-level code-structures.  

###Inventory System
The Inventory System uses Scriptable-objects to create custom objects that can represent items a player might collect during game-play.  The Inventory-System separates game-data storage-functionality from UI-interaction-display functionality. [Inventory-System ](/project-2-dictionaries-to-store-data/inventory-scriptableobject.md)Code, [InventoryDisplay](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md) Prefabs, and [Instructions](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md) are provided in these sections of the gitbook.

###Level-Manager for Enhanced MiniGame

Once you have a working Inventory-System, proceed to completing the enhanced MiniGame by adding logic for multi-level game.  

**Custom UnityEvents:** The LevelManager-System uses custom UnityEvents to manage decoupled object-communication. Scripts were modified to integrate additional logic for Publishing, Listening for custom UnityEvents across several classes: **GameData, PlayerController, LevelManager, MiniGState.
**
**Enhanced Game-play:**The Minigame is modified to include enhanced concepts:  **timer, collect-use items, camera-follow, water-hazard, platforms, scene-fading, scene-reloading. **Consequences for win/lose mini-game must exist.

##Project 3 - Enhanced MiniGame 

###Update GameData
- **Step 1: Update GameData Script** Update the code for [GameData.](/class-code-examples/gamedata-final.md)  To play-test in the MiniGame Scene, add GameData script to an empty-gameObject: GameManager, make sure you have created a ScriptableObject - Inventory object, select that Inventory on the GameData script. 

 **Updated GameData: TakeDamage: 4/24/19**

```java
        public void TakeDamage( int value){
        health -= value;
        if (health < 0) health = 0;  //makes sure health !< 0
        Debug.Log("Health is updated " + health);
        InvokePlayerDataUpdate();
    }
```

###Player, PlayerController, PlayerController_v2

- **Player:  Update PlayerController Script**   This includes 2 custom UnityEvents.
  
 - **PlayerSpawnPoint:** Create an Empty GameObject:  PlayerSpawnPoint, select an icon so it is visible in the scene.  Position this gameObject at the position that you want the player positioned at the start of each level.  Populate this on the PlayerController, and LevelManager components in the inspector. **Make sure the PlayerSpawnPoint has Transform.Position.Z = 0 (if Z= -10 you won't see the player)**

 - **PlayerController_v2: Possible Issue:** If using **PlayerController_v2**, you must update code in LevelManager that uses PlayerController, or rename your file to: PlayerController 
 - **Set Tag: 'Player'** for the Player gameObject for CameraFollow

###LevelManager GameObject 
 - **Create empty gameObject: LevelManager** 
Delete MiniGameManager, and MiniGameManager.cs, replace with LevelManager

  - **Add script: [LevelManager.cs](/class-code-examples/levelmanager-final.md) to LevelManager in Scene**  Create or update code in LevelManager, add to LevelManager empty gameObject.  Look at the inspector fields for LevelManager.

You will need to do further configuration for LevelManager, see below

###ScreenFader (optional) 

- **ScreenFader (optional)**   The LevelManager includes code for a ScreenFader functionality, either remove that code, or create a new C# script, paste code for [ScreenFader.cs](/class-code-examples/screenfader.md).  Put the ScreenFader script on the MainCamera gameObject in any scene you want Fade-in during start.  Code must be modified in State scripts if you want Fade-out at the end of a scene.  For any scene that uses ScreenFader, you must create a UI-Image gameObject, move it out of the camera's view, set the color to black. 

- **Important:** **If you don't use ScreenFader**, you must **change code in LevelManager: ReloadMiniGame() **

```java

 //fader.EndScene(curScene.buildIndex); //remove
  SceneManager.LoadScene(curScene.buildIndex); //add this if not using fader
```      

###CameraFollow (optional) 
- **CameraFollow (optional) : Player must have Tag: Player **  Create a new C# script, paste code for [CameraFollow.cs](/cameraFollow), attach to MainCamera in MiniGame.  This assumes you have a background image larger than the camera's viewport, play around with variables that restrict amount of camera movement, so it works with your backgrounds.

###PlayerStats
 [PlayerStats](/class-code-examples/playerstats-final.md) Code updated 4/24/2019
 
- PlayerStats.cs script must be updated so it uses code from LevelManager.  

##MiniGame - New GameObjects

###LevelX_GameObjects
- **Create 3 Level-Specific: Parent GameObjects**, one for each Level, with children gameObjects: **background, spawner,** etc.  Add these to the LevelManager script.  Ordering in the Hierarchy and Inspector must match images below.

![](/assets/Screen Shot 2019-04-24 at 1.11.38 PM.png)

- **UI Elements:**






