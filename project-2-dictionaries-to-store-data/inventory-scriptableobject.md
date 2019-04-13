#Inventory System using ScriptableObjects

###Download, Install Inventory-System Code in Unity Package
**Download:** The code for these classes is included in the Inventory System Code package. [Download Link: Inventory System Code Unity Package](https://utdallas.box.com/v/InventorySystem-Code)

**IMPORTANT:** Importing this package will overwrite all of the existing files listed below, you will need to fix some minor issues detailed in the section below.
 
 _Updated Apr 10, 2019_

The **Inventory System** uses the following Classes:

**ScriptableObject Classes - Data-Model**
- **abstract class Item**: ScriptableObject
- class **ItemInstance:** ScriptableObject
- class **Inventory**: ScriptableObject
- classes for custom items:
    - class **Gem**: Item
    - class **Potion**: Item
    
**MonoBehaviour Classes: Display - UI - View**

- class **InventoryDisplay**: MonoBehaviour
- class **Slot**: MonoBehaviour, IPointerClickHandler

Other MonoBehaviour Required Files:
- class **Hazard**: MonoBehaviour


**Modified, Existing files:**
- **GameData**: MonoBehaviour
- **PlayerController**: MonoBehaviour
- **PickUp**: MonoBehaviour


###Fix Unity Errors due to changes in GameData, PlayerController, PickUp classes.
**Important: several changes are required to re-configure GameObjects in BeginScene, MiniGameScene, and Asset Prefabs: PickUp.**  

- **Step 1: BeginScene - GameData issues:**  
    - In BeginScene, make sure the GameManager gameObject has no errors on the GameData.cs script component. (fix: Remove component, re-add)
    - Create an Inventory ScriptableObject: named: GameInventory. ( no additional configuration needed for GameInventory scriptableObject )
    - Add GameInventory scriptableObject to the GameData.cs component on GameManager.
    
    
- **Step 2: MiniGameScene - PlayerController issues**
    - In the MiniGame, Select the Player gameObject, make sure the PlayerController script has no errors. (fix: Remove component, re-add)
    - Reconfigure PlayerController script - Reset GroundCheck by selecting the GroundCheck empty gameObject.
    - Reconfigure PlayerController script LayerMask: Ground
    
    
- **Step 3: MiniGame-Prefab Asset: PickUp issues:**
    - To edit the PickUp prefabs, drag an instance of each type of prefab into the hierarchy panel so it can be edited. (Unity 2018 - edits can be done in the directly in prefab editor)
    - For Prefabs with 'Collectible Tag':  make sure the PickUp script component shows no errors. (fix: Remove component, re-add ).
    - An ItemInstance scriptableObject must be added to the PickUp script.
        - Create a scriptableObject from the Gem or Potion class and set all values, sprites.
        - Add the scriptableObject to the PickUp's Item field as shown in images below.
         
![](/assets/Screen Shot 2019-04-13 at 3.54.56 PM.png)
![](/assets/Screen Shot 2019-04-13 at 3.53.50 PM.png)

##Inventory Display: Slot, Store, BtnAdd-Item 

See details for downloading and installing InventoryDisplay Unity Package. **[Configuration Details for the InventoryDisplay Package](https://kdoore.gitbooks.io/cs-2335/content/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.html)**

After completing install and configuration of InventorySystem files from the UnityPackage listed above, then you can download and install the UnityPackage that includes the UI elements.

**Do not install the package, linked below, until you have completed reconfiguration for Inventory-System, detailed above. **

**Download: Unity Package file** with Prefab of UI Inventory Display GameObjects

**Link:** [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

###Inventory System: Reference Tutorials

Below are links to several tutorials about custom Unity Inventory-Systems using ScriptableObjects which were used to create the InventorySystem in this course..  

- [Michael Palmos: Inventory System - Blog](https://toqoz.svbtle.com/a-unity-inventory-system-that-actually-works)  This inventory system is based on this blog series by Michael Palmos.

- [RPG Inventory-System:](https://www.mvcode.com/lessons/unity-rpg-inventory-system-jamie) - by Jamie at MVCode Clubs 













