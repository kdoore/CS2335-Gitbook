#Project 3 - Steps

Project 3 is focuses on Understanding Event-Driven System-level code-structures.  

###Inventory System
The Inventory System uses Scriptable-objects to create custom objects that can represent items a player might collect during game-play.  The Inventory-System separates game-data storage-functionality from UI-interaction-display functionality. [Inventory-System ](/project-2-dictionaries-to-store-data/inventory-scriptableobject.md)Code, [InventoryDisplay](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md) Prefabs, and [Instructions](/project-2-dictionaries-to-store-data/inventory-scriptableobject/inventory-display-slot.md) are provided in these sections of the gitbook.

###Level-Manager for Enhanced MiniGame

Once you have a working Inventory-System, proceed to completing the enhanced MiniGame by adding logic for multi-level game.  

**Custom UnityEvents:** The LevelManager-System uses custom UnityEvents to manage decoupled object-communication. Scripts were modified to integrate additional logic for Publishing, Listening for custom UnityEvents across several classes: GameData, PlayerController, LevelManager, MiniGState.

**Enhanced Game-play:**The Minigame is modified to include enhanced concepts:  timer, collect-use items, camera-follow, water-hazard, platforms, scene-fading, scene-reloading. Consequences for win/lose mini-game must exist.




