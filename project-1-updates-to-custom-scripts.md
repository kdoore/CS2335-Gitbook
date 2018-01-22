$$$$#Project 1 - Updates to Custom Scripts

The following changes need to be made to the custom scripts created earlier: AppleTree and Basket.  In addition, a Rock script is created to add a destructive object to the game. The Rock script is similar to the Apple.cs script, but the pointValue is negative.  

###GameController.gameActive to Control Gameplay Actions 

Since we don't want apples being dropped before the StartGame button has been selected, and because we want to stop apples from dropping when the game has ended, we can create a boolean variable in the GameController class: `bool gameActive`.  We can toggle the value of this variable to start / stop apples from dropping; since apple dropping is managed in the AppleTree script, we need to give this variable a `public` access modifier.  

