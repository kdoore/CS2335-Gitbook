# Level Manager

Within the MiniGame, we'd like to have several levels of play. Level is an abstract concept, in this gameing context, what do we mean by several levels of play?  We'd like to break the game-experience into chunks, this will let the player know they have attempted to complete a task, and the game should provide some feedback to indicate their level of success in this challange.

In this course, we are focused on the functional and structural design of the layer-change event logic.  The dynamic logic can be structured using a finite state machine, since the level logic is event driven and since the game-system can only be in a single state at any time.   One question we might ask is how can we integrate feedback to enhance player learning using an FSM system model?  

For a mini-game that uses one scene to implement a series of game-levels, we can use a lightweight Finite State Machine - where we define an enum: LevelState to define a set of states that the mini-game system can be in.  We'll put this code in the LevelManager.cs script fild.

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

We can use the Start state to allow for a Splash screen and start-button event to trigger the start of the miniGame, or we can start the game-play use the LevelManager Class's Start( ) method to trigger the begin of gameplay.  The transtion between levels is managed by the NextLevel() function which is triggered when the PlayerDataUpdate event from GameData is broadcast;

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



