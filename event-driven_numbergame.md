# UI-Button: Event-Handler NumberGame

In the previous iterations of the game, we have used the Update() Unity Event function to implement control and logic of the game.  In every frame, our code checked the current activeState, then reset the UI text elements to the correct values, even if the activeState hasn't changed.  Actually, the only time that we need to be changing UI text element values is when a game-event has occurred. If we restructure our game so that we use UI-Buttons and button-click events, then we can eliminate some of the code that's being executed each frame in the Update function. 

###UI-Button: OnClick Event-Handlers