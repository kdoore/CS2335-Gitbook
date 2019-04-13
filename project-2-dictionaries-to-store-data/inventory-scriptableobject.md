#Inventory System using ScriptableObjects

The **Inventory System** uses the following Classes:

The code for these classes is included in the Inventory System Code package. [Download Link: Inventory System Code Unity Package](https://utdallas.box.com/v/InventorySystem-Code)


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

Other Required Files:
- class **Hazard**: MonoBehaviour


Modified files:
- **GameData**: MonoBehaviour

- **PlayerController**: MonoBehaviour

- **PickUp**: MonoBehaviour

###Download Unity Package with Updated Code files:
_Updated Apr 10, 2019_

[Download Link: Inventory System Code Unity Package](https://utdallas.box.com/v/InventorySystem-Code)

**IMPORTANT:** Importing this package will overwrite all of your existing files listed below. 

**Changes will be required to reconfigure gameObjects in BeginScene, and MiniGameScene, and MiniGame PickUp Prefabs which use GameData.cs, PlayerController.cs, and PickUp.cs.  **

**You may choose not to import the following items, and instead, paste the updated code into your scripts:**
- GameData.cs,
- PlayerController.cs
- PickUp.cs

**InventorySystem Package Included Files:**
**Overwrite of Existing Files**
- GameData.cs
- PlayerController.cs
- PickUp.cs

**New files: InventorySystem**
- Item.cs
- Gem.cs
- Potion.cs
- Inventory.cs
- InventoryDisplay.cs
- Slot.cs
- Hazard.cs

###Prefabs: Inventory Display, Slot, Store, BtnAdd-Item 
After completing install of InventorySystem files from the UnityPackage listed above, then you can download and install the UnityPackage that includes the UI elements.

**Do not install the package below until you have completed reconfiguration for Inventory-System, detailed above. **

**Download: Unity Package file** with Prefab of UI Inventory Display GameObjects
**Link:** [UI-DisplayPrefab_Package-S19](https://utdallas.box.com/v/UI-InventoryDisplay-S19)

[Configuration Details for the InventoryDisplay](https://kdoore.gitbooks.io/cs-2335/content/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.html)

###Inventory System: Reference Tutorials

Below are links to several tutorials about custom Unity Inventory-Systems using ScriptableObjects which were used to create the InventorySystem in this course..  

- [Michael Palmos: Inventory System - Blog](https://toqoz.svbtle.com/a-unity-inventory-system-that-actually-works)  This inventory system is based on this blog series by Michael Palmos.

- [RPG Inventory-System:](https://www.mvcode.com/lessons/unity-rpg-inventory-system-jamie) - by Jamie at MVCode Clubs 













