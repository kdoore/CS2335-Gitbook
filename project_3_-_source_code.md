## Project 3 - Source Code

Here is a link to the zip file for the Space Girl Game.  This includes the Pickup Classes and the GameData - Inventory Dictionary.  Also, there is an InventoryDisplay class that is used in the game scene to display pickup total points scored in the game.


[Link to Project3 Zip file](https://utdallas.box.com/Project3Inventory)



[Link to Project3 December 2 Code](https://utdallas.box.com/Project3Dec2)


[Final Link to Project 3 Starter Code:  From December 3](https://utdallas.box.com/Project3inventoryStore)

In this final version I have created an additional player Inventory component in the store that shows 4 of the player's items.  

There are several dictionaries in the GameData class, one for store Inventory which hasn't been integrated into the project yet.  One for player Inventory of items purchased in the store, one for Pickup Items.  

The player Inventory dictionary contains InventoryItems, these get added when something is purchased in the store.  Currently there is only logic to support single quantities of items as stored in the dictionaries and the store arrays, how can we change that, it might make sense to add a quantity field to the inventory item itself, so the item could store the number of discrete elements in a store or for the player.

There's a new player inventory manager script that's used to control adding items to the player's inventory within the store scene.


