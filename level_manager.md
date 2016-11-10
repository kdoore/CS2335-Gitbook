# Level Manager

Within the MiniGame, we'd like to have several levels of play. Level is an abstract concept, in this gameing context, what do we mean by several levels of play?  We'd like to break the game-experience into meaningful chunks, this will let the player know they have attempted to complete a task, and the game should provide some feedback to indicate their level of success in this challange.

In this course, we are focused on the functional and structural design of the layer-change event logic.  The dynamic logic can be structured using a finite state machine, since the level logic is event driven and since the game-system can only be in a single state at any time.   One question we might ask is how can we integrate feedback to enhance player learning using an FSM system model?  

For a mini-game that uses one scene to implement a series of game-levels, we can use a lightweight Finite State Machine - where we define an enum: LevelState to define a set of states that the mini-game system can be in.  We'll put this code in the LevelManager.cs script file.

```C#
public enum LevelState
{
	Start,
	Level1,
	Level2,
	Level3,
	Win,
	End

}

/// inside LevelManager Class

public LevelState curLevel;  //declare a LevelState variable to store the active state

```

We can use the Start state to allow for a Splash screen and start-button event to trigger the start of the miniGame, or we can start the game-play use the LevelManager Class's Start( ) method.  The transtion between levels is managed by the NextLevel() function which is triggered when the PlayerDataUpdate event is broadcast from the GameData component object;

```C#
/// Inside LevelManager.cs class script
void Start ()
	{

		curLevel = LevelState.Start;  ///initialize curLevel
		crystalSpawner = GameObject.Find ("CrystalSpawner").GetComponent<CrystalSpawner> ();

		gameData = GameData.instanceRef;  //Register CheckLevelEnd function to be notified when GameData broadcasts: onPlayerDataUpdate
		gameData.onPlayerDataUpdate += CheckLevelEnd;  //registering to be notified

		nextLevel ();  //starts the game -
	}

```

###OnPlayerDataUpdate Event - CheckLevelEnd is Registered as an Event Subscriber
The GameData event: OnPlayerDataUpdate event is the trigger that provides notification to the LevelManager whenever the GameData Add( PickUp ) function is executed in response to a OnTriggerEnter2D event.  We've created a custom function that gets registered for notification of the event, and the CheckLevelEnd method matches the   

```C#
void CheckLevelEnd (object sender, PlayerDataEventArgs e)
	{
		int levelScore = e.levelScore;

		Debug.Log ("Check if level is over" + levelScore);
		////change the total score display

		if (levelScore > 10) { ///level has changed
			///reset level value display
			gameData.LevelScore = 0;   //reset level score

			nextLevel ();
		}
	}



```





###Finite State Machine Logic - nextLevel( )

The nextLevel() function provides the FSM logic for the Level management.

```C#

void nextLevel ()
	{
		switch (curLevel) {
		case LevelState.Start:
			Debug.Log ("State changed - goto :LoadLevel 1");
			curLevel = LevelState.Level1;
			LoadLevel1 ();
			break;
		case LevelState.Level1:
			curLevel = LevelState.Level2;
			LoadLevel2 ();
			break;
		case LevelState.Level2:
			curLevel = LevelState.Level3;
             //add code here
			break;
		case LevelState.Level3:
			curLevel = LevelState.Win;
             //add code here
			break;
		case LevelState.Win:
			curLevel = LevelState.End;
            // add code here
			break;
		case LevelState.End:
            //add code here
			break;
		} // end switch
        
	} // end function

```

#Level-Change Behaviors
The nextLevel() method provides the FSM structure to manage state-change logic, so now we just need to decide what to change in the game-scene when this level-change event occurs.
One simple change to implement is toggling visibility of different background sprites.  An easy way to implement this is to make sure the background images are UI canvas images, this will allow us to toggle the CanvasGroup component's alpha and interactivity properties.  