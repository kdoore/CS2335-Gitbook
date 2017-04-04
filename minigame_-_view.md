# MiniGame

### Dynamic Interaction with Goals and Consequences

The MiniGame is the portion of the Project where the player can dynamically interact with a player sprite, with some objective or goal that will result in some game-data generated that will have consequential impact on the player experience in subsequent scenes in the game.

In order to design the logic of the mini-game, it will be good to take a high-level view of  several concepts that will impact our code design.  What are new script components that represent important concepts for the mini-game design.

# Mini-Game Script Components

1. **GameData** - what is the data generated in the game and how do we store the data so we can access it in subsequent game scenes.

   * Data Structures:  
     * List&lt; T &gt;  
     * Dictionary &lt; TKey, TValue &gt; 

2. LevelManager - this custom script component will manage organization of the MiniGame logic and behaviors.

   * What game objects does it need to communicate with?
     * Player - score events
     * UI - Level Display
     * Spawner - when / what prefabs are spawned
     * BackgroundImage - modify sprite when level changes
     * 

### MiniGame.cs

Since we've been using our State.cs classes for implementing the logic for the game UI, for now we can say that the MiniGame.cs class, which Implements IStateBase and  represents the current activeState for our State-Machine Framework.

```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using System;

public class MiniGameState : IStateBase{

// Use this for initialization

    private StateManager managerRef;
    private  GameScene scene;

    //Public Property GameState
    public GameScene Scene {
        get{ return scene; }
    }

    //constructor  // add comments
    public MiniGameState ()
    {
        scene = GameScene.MiniGame;
    }

    public void InitializeObjectRefs (){

     }


}  //end MiniGame.cs
```



