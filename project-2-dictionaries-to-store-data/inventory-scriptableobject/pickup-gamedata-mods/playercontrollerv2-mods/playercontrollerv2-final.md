#Player Controller_v2 Final Code
This code is to be used with the Simplified MiniGame.


Includes Updates for:  InventorySystem, LevelManager.

**OnTriggerEnter2D** contains code to test for collisions with GameObjects with Collider2D set as 'Trigger' based on several different gameObject tags
** Tags used in PlayerController **:
 - **Collectible**
     - Requires either PickUp.cs or ScorePickUp.cs
     - PickUp.cs requires ItemInstance - ScriptableObject - adds value to Score, adds ItemInstance to Inventory
     - ScorePickUp.cs - adds value to score
     
 - **Hazard**
     - Requires either PickUp.cs or Hazard.cs
 - **Water**
     - Invokes: onPlayerDied
 - **Exit**
     - Invokes: onReachedExit

Audio clips played if colliding with Pickup with correct audioSource.  ( Collectible, Water )

Audio Clips:  


Contains custom Events, LevelManager is the subscriber object:  
- onPlayerReachedExit
- onPlayerDied 
 


