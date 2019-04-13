#Simplified MiniGame - Starter Code 

 If you did not submit a working MiniGame for Project 1, then you can follow these steps to add required functionality of simplified minigame for Project 3.   

**Required Configuration Steps, Details:**

- **Start:** Create a **backup version of your Project 2 Unity project.**  You will add and modify this simplified MiniGame, to create Project 3. 

- **Step 1: Download and install Inventory-System Code: [Inventory-System Class Files UnityPackage](https://utdallas.box.com/v/InventorySystem-Code). **

- **Step 2: **After downloading, installing, the Inventory-System Code, you must reconfigure the GameManager gameObject in BeginScene to insure that GameData.cs script component is attached. correctly configured.  

- **Step 3:** Create a **Inventory ScriptableObject**, and **Attach to GameData in BeginScene**. (See additional details)

- **Resolve all errors** before downloading, installing the MiniGame_v2 Package.

- **Step 4:  Download and install: [Simplified Minigame_v2: UnityPackage](https://utdallas.box.com/v/miniGame-v2-Proj3-startAsset) **
  - **Package Contents:** 
   - MiniGame_v2 Scene with configured GameObjects 
   - Custom Scripts:  PlayerController_v2, MiniGameManager_v2

- **Step 5:** Attach **GameData.cs** to empty gameObject: GameManager gameObject in MiniGame_v2.   

- **Step 6:** Attach **Inventory ScriptableObject** (created in step 2) to the GameData component in the MiniGame_v2.

- **Step 7**  In MiniGame_v2 Scene:  **Sorting Layers** must be set for all objects with Image/SpriteRenderer Component:  If a gameObject is not visible, set the sorting-layer for that object. 

- **Finally:** Try to **play the MiniGame_v2**. 

- **Verify Functionality:** StartButton starts the game, pickUp prefabs are visible, player gameObject can be moved using arrow keys, jump with spacebar.  Verify score and health change when hitting PickUp objects. 


 


