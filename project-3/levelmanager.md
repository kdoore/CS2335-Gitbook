#Level Manager

###GameData - check for the following updates:
You may need to add some new code to GameData, this is specified in the LevelManager-Events documentation.  

These are changes that should be incorporated into GameData prior to working with the Level Manager
    - levelScore variable, LevelScore property.
    - onPlayerDataUpdate, UnityEvent
    - inventory - Inventory scriptableObject
    - lives, Lives properties?
    
- Data-Events: These trigger onPlayerDataUpdate
    - Event: Add( )
        - increment levelScore with points
        - invoke onPlayerDataUpdate
    - Event: TakeDamage( )
        - invoke onPlayerDataUpdate
    - Event: AddItem( )
        - invoke onPlayerDataUpdate

###LevelManager Code 
You will need to modify this code to match your game's logic

[LevelManager_Final](/class-code-examples/levelmanager-final.md)