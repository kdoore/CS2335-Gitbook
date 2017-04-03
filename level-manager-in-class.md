#Level Manager - In Class 

###Level Manager Overview

In the code below, the Level Manager is responsible for controlling the Mini_Game Level Logic, this includes determining when to change levels and what gameObjects are impacted when the level changes.  The LevelManager Custom Script is attached to the LevelManager game object in the Mini-Game scene and provides Finite State Machine logic to determine the current level and state of dependent gameObjects within the Mini-game. 

###Inter-Dependent Game Objects

Game objects that are inter-dependent with the LevelManager are: 

1. Splash Screen - UI Panel with StartMiniGame Button - is hidden when StartGameButton is clicked.
2. Background Image - changed with each level-change
3. Spawner - start spawning when splash screen start-button is clicked
4. Spawner - spawn different prefabs depending on the current level
5. UI Elements - Level and Score UI displays must be updated to reflect the current Level and the current Score value.
6. PlayerController - this GameObject will sense collisions with PickUps, this will change the score and the level
