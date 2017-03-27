# MiniGame



###MiniGame.cs 
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