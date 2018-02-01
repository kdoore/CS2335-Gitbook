###StartButton Functionality

In this section, we'll take a look at the functionality of the StartButton in the Scene and the Scripts and other gameObjects that have behaviors that connected with this gameObject.

The StartButton is used to start the game, it is used to call the custom StartGame( ) method in the GameController class. Once the StartButton has been clicked, objects should start falling from the AppleTree, and the StartButton should be hidden. When the StartButton is clicked, the GameController.gameActive  flag variable should be set to true.  

###Steps to Create / Configure The StartButton GameObject
1. Add a UI-Button to the Scene
2. Rename the Button to StartButton
3. Set Button colors: Highlight and Normal colors should be different.
4. Set the Text GameObject to say `Start Game`
5. Set the Anchors and Position of the StartButton, so that it's anchored and moved to the bottom-center of the screen.  
6. Once the anchors are set for the Button, the move tool can be used to move the Button away from the bottom edge.
7. On the OnClick section of the Button's script component, select the GameController gameObject, then select the StartGame() method. This method will be executed when the StartButton is clicked.

###Animation to Create and Configure StartButton GameObject

![](http://g.recordit.co/j0q1PTchAr.gif)

###Code for the StartButton is in the GameController Script


