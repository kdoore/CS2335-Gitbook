Project 1 - Score and UI Elements

In order for these interacting sprites to feel like a game, we need to keep track of the score, we need to define conditions for winning and losing, and we need to display this information to the player.  

###GameController Class
It's common to have a GameController class to manage the higher level logic of the game, such as keeping track or score, winning and losing, etc.  We'll create a GameController class for this purpose, and we'll attach the script to the main-camera, it's common practice to attach such scripts to the main-camera, as every scene is guaranteed to have a main-camera.