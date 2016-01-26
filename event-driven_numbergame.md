# Event-Driven Number Game

In the previous iterations of the game, we have used the Update() Unity Event function to implement code for control and logic of the game.  In every frame, our code checks the current activeState, then checks for any input, then we reset the UI text elements to the correct values when the activeState changes.  So, the only time that we need to be changing UI text element values is when an input event has occurred. 

We can restructure our game so that we use UI-Buttons and the Unity managed: button `on-click` event, then we can eliminate some of the code that's being executed, as we check for valid keyboard input values each frame in the Update function.  

###UI-Button: OnClick Event-Handlers
Unity provides an event system for the Unity User-Interface (UI) components. GameObjects like Button have a Button Component that provides easy configuration to have our custom functions executed when the user clicks on a UI button in our active game scene. 