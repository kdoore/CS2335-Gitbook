#Inventory System using ScriptableObjects

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

Other Required Files:
- class **Hazard**: MonoBehaviour


Modified files:
- **GameData**: MonoBehaviour

- **PlayerController**: MonoBehaviour

- **PickUp**: MonoBehaviour


###Inventory System Tutorials

Below are links to several tutorials about custom Unity Inventory-Systems using ScriptableObjects.  

- [Michael Palmos: Inventory System - Blog](https://toqoz.svbtle.com/a-unity-inventory-system-that-actually-works)  This inventory system is based on this blog series by Michael Palmos.

- [RPG Inventory-System:](https://www.mvcode.com/lessons/unity-rpg-inventory-system-jamie) - by Jamie at MVCode Clubs 













