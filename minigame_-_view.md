# MiniGame - View
The MiniGame.cs custom script is acting as the UI View for displaying Player and Spawn Data, so it has defined EventHandlers and needs to Register the event handlers when the Scene has loaded.  First it needs a reference to the gameObjects - scriptComponents that will be sending event notifications:  The CrystalSpawner and the GameData script components which are attached to gameObjects. 


###MiniGame.cs - The UI View
Since we've been using our State.cs classes for implementing the logic for the game UI, for now we can say that the MiniGame.cs class, which Implements IStateBase and  represents the current activeState for our State-Machine Framework.  Later we can create a Prefab to represent our Player heads-up display for presenting player game stat information, that will be the view.

Inside the UI-View, we want to receive notification of spawn events and we want to receive notification of data events so we can display these on the screen. So we need to write EventHandler methods - MiniGame.cs is a subscriber to Events: CrystalSpawner.OnSpawn, GameData.
OnPlayerDataUpdated

```
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using System;

public class MiniGameState : IStateBase{

// Use this for initialization
	private StateManager managerRef;

	private CrystalSpawner crystalSpawner;
	private GameData gameData;

	private Text label;
	private Text score;
	//private float prevScore;  //used for polling example
	private Button gameStartButton;

	private GameState state;

	//Public Property GameState
	public GameState State{
		get{ return state; }
	}

	//constructor  // add comments
	public MiniGameState( StateManager manager  ){
		managerRef = manager;  //// establish connection with StateManager
		state = GameState.Begin;
		//prevScore = 0;
	}


	// Update is called once per frame
	public void StateUpdate () {
		///one option is to use polling to determine when TotalScore changes
		/// then update ScoreText in the UI
		//float curScore = gameData.TotalScore;
		//if (curScore > prevScore) {
			//PollingUpdateScoreText ();  ///called every frame
		//	prevScore = curScore;
		//	Debug.Log ("Polling update Score");
		//}
	}
		
	public void StateGUI(){

	}

	public void InitializeObjectRefs (){
		
		crystalSpawner = GameObject.Find ("CrystalSpawner").GetComponent<CrystalSpawner> ();
        
		crystalSpawner.onSpawn += IncreaseCount;  // register as eventHandler for CrystalSpawner.onSpawn event notification
		
		
        gameData = GameObject.Find("GameManager").GetComponent<GameData>();
		
        gameData.onPlayerDataUpdate += UpdatePlayerData ;  // register as as an eventHandler for the gameData.onPlayerDataUpdate Event notification
		label = GameObject.Find ("CrystalLabel").GetComponent<Text> ();
		score = GameObject.Find ("TotalScore").GetComponent<Text> ();
 	}

	void PollingUpdateScoreText(){
		score.text = string.Format ("Total Score: {0}", gameData.TotalScore);
	}

	/// <summary>
	/// /EventHandler - for OnPlayerDataUpdate Event in GameData
	/// </summary>
	/// <param name="sender">Sender.</param>
	/// <param name="e">E.</param>
	
    void UpdatePlayerData(object sender, PlayerDataEventArgs e){
		int totalScore = e.totalScore;
		score.text = string.Format ("Total Score: {0}", totalScore);
	}
		
	void OnDestroy () {  //when this object current state is no longer the active state: 
		// We unsubscribe to avoid memory leaks
		Debug.Log("Destroyed MiniGameState");
		gameData.onPlayerDataUpdate -= UpdatePlayerData;
		crystalSpawner.onSpawn -= IncreaseCount;
	}

	// Action executed onSpawn event when a new crystal is spawned
	public void IncreaseCount() {
		// Turn the string into a int, add and reconvert to string
		int prevCount = int.Parse(label.text) + 1 ;
		label.text = prevCount.ToString();
	}

	public void leaveScene(){
		//managerRef
		//when we leave this Scene / State, we need to unregister any eventHandlers that
		// we have registered 
		OnDestroy ();
	}

}  //end MiniGame.cs

```