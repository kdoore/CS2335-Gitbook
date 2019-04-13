#Simplified MiniGame

If you did not submit a working MiniGame for Project 1, then follow these steps to add required functionality to a **backup version of your Project 2 Unity project.**  You will modify this simplified MiniGame, to complete Project 3. 

**Required Configuration Details:**

- **Step 1: Download and install: [Inventory-System Class Files UnityPackage](https://utdallas.box.com/v/InventorySystem-Code). **

After downloading, installing, the Inventory-System Code, you must reconfigure the GameManager gameObject in BeginScene to insure that GameData.cs script component is correctly configured.  

- **Step 2:** Create a **Inventory ScriptableObject**, and attach to GameData in BeginScene.

- **Resolve all errors** before downloading, installing the MiniGame_v2 Package.


- **Step 3:**  Download and install: [Simplified Minigame_v2: UnityPackage](https://utdallas.box.com/v/miniGame-v2-Proj3-startAsset) 

- **Step 4:** Attach GameData.cs to GameManager gameObject in MiniGame_v2.   

- **Step 5**  In MiniGame_v2 Scene:  Sorting Layers must be set for all objects with Image/SpriteRenderer Component:  If a gameObject is not visible, set the sorting-layer for that object. 

- **Step 6** In MiniGameStateManager_v2.cs:  Modify the code in Start() so that it matches the code below.


```java
//MiniGameStateManager_v2, Replace Start() with Code below.

void Start()    {        startButton.onClick.AddListener(StartGame);        StartGame(); //calls method with StartGame Logic        //StartGame() disables startButton, so enable it at start        startButton.gameObject.SetActive(true); //displays the StartButton     }

```


**Package Contents:** 

 - MiniGame_v2 Scene with configured GameObjects 
 - Custom Scripts:  PlayerController_v2, MiniGameManager_v2
 


