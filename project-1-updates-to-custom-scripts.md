$$$$#Project 1 - Updates to Custom Scripts

The following changes need to be made to the custom scripts created earlier: AppleTree and Basket.  In addition, a Rock script is created to add a destructive object to the game. The Rock script is similar to the Apple.cs script, but the pointValue is negative.  

###GameController.gameActive to Control Gameplay Actions 

Since we don't want apples being dropped before the StartGame button has been selected, and because we want to stop apples from dropping when the game has ended, we can create a boolean variable in the GameController class: `bool gameActive`.  We can toggle the value of this variable to start / stop apples from dropping; since apple dropping is managed in the AppleTree script, we need to give this variable a `public` access modifier.  

###AppleTree - No Tree Movement unless gameActive is true

We need to modify the AppleTree.cs class to make sure the AppleTree gameObject doesn't move until the StartButton is clicked.  We do this by checking whether GameController.gameActive is true or false.  First we need to create a script Object-Reference variable to allow us to interact with the GameController script component within the AppleTree.cs script.  Since we want to access a variable outside the class definition, we need to declare the variable to have public access.

//Inside GameController.cs:

```java
public bool gameActive = false; //initialize to false
```



//To access inside AppleTree.cs