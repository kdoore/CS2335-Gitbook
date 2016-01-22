# Event-Driven Number Game

In the previous iterations of the game, we have used the Update() Unity Event function to implement control and logic of the game.  In every frame, our code checked the current activeState, then check for any input, then we reset the UI text elements to the correct values when the activeState changes.  So, the only time that we need to be changing UI text element values is when an input event has occurred. We can restructure our game so that we use UI-Buttons and button-click events, then we can eliminate some of the code that's being executed, as we check for valid input values each frame in the Update function.  

###UI-Button: OnClick Event-Handlers