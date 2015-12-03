## Project 3 - Source Code

Here is a link to the zip file for the Space Girl Game.  This includes the Pickup Classes and the GameData - Inventory Dictionary.  Also, there is an InventoryDisplay class that is used in the game scene to display pickup total points scored in the game.


old:  [Link to Project3 Zip file](https://utdallas.box.com/Project3Inventory)



old:  [Link to Project3 December 2 Code](https://utdallas.box.com/Project3Dec2)


**Most Current StarterCode version: December 3**

[Final Link to Project 3 Starter Code:  From December 3](https://utdallas.box.com/Project3inventoryStore)

In this final version I have created an additional player Inventory component in the store that shows 4 of the player's items.  

There are several dictionaries in the GameData class, one for store Inventory which hasn't been integrated into the project yet.  One for player Inventory of items purchased in the store, one for Pickup Items.  

The player Inventory dictionary contains InventoryItems, these get added when something is purchased in the store.  Currently there is only logic to support single quantities of items as stored in the dictionaries and the store arrays, how can we change that, it might make sense to add a quantity field to the inventory item itself, so the item could store the number of discrete elements in a store or for the player.

There's a new player inventory manager script that's used to control adding items to the player's inventory within the store scene.

If you want to display multiple items such as text, background image, etc to be associated with an inventory item, then put the slot into a panel and within that panel or gameObject you can have multiple UI elements visible.  Any gameObject can only have 1 visible element at a time, so if it's a spriteRenderer component that's displaying your inventoryItem in some type of slot gameObject, you'll want to put that slot within a larger container to allow display of additional UI elements.
