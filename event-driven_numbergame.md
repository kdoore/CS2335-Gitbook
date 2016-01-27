# Event-Driven Number Game

In the previous iterations of the game, we have used the Update() Unity Event function to implement code for control and logic of the game.  In Update() for every frame execution, our code checks the current activeState, then checks for any input that is valid for that state, then we reset the UI-Text elements to the correct values when the activeState changes.  In effect, the only time that we need to change UI-Text element values is when an input event has occurred. 

We can restructure our game so that we use UI-Buttons and the Unity managed: button `on-click` event, then we can eliminate some of the code that's being executed, as we check for valid keyboard input values each frame in the Update function.  

###UI-Button: OnClick Event-Handlers
Unity provides an event system for the Unity User-Interface (UI) components. GameObjects like Button have a Button Component that provides easy configuration to have our custom functions executed when the user clicks on a UI button in our active game scene. 

###Custom Function
In order to add custom behavior to a UI-Button, first we need to write some code that we'd like to have executed when the button has been clicked.  For our number game project, we can add a start game button that the user will click to indicate that they want to play the game.  Currently, we prompt the user when they are in the Initialize GameState to enter 'Y' if they went to play, or to enter 'N' to quit.  So, we have the following logic in our code, where we are listening for 'Y' when the activeState = GameState.Initialize:
```
	if (activeState == GameState.Initialize) {
			
			if (Input.GetKeyDown (KeyCode.Y)) {
				Debug.Log ("Think of a number between " + min + " and " + max + " press Enter when ready");
				activeState = GameState.Start;
			} 
			if (Input.GetKeyDown (KeyCode.N)) {
				Debug.Log ("No game today");
				activeState = GameState.End;
			}

		}
```