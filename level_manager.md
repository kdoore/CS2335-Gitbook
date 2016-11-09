# Level Manager

Within the MiniGame, we'd like to have several levels of play. Level is an abstract concept, in this gameing context, what do we mean by several levels of play?  We'd like to break the game-experience into chunks, this will let the player know they have attempted to complete a task, and the game should provide some feedback to indicate their level of success in this challange.

In this course, we are focused on the functional and structural design of the layer-change event logic.  The dynamic logic can be structured using a finite state machine, since the level logic is event driven and since the game-system can only be in a single state at any time.   One question we might ask is how can we integrate feedback to enhance player learning using an FSM system model?  

