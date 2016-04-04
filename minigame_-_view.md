# MiniGame - View

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
		if (crystalSpawner != null) {
			crystalSpawner.onSpawn += IncreaseCount;  // add method with delegate-type
		}
		gameData = GameObject.Find("GameManager").GetComponent<GameData>();
		gameData.onPlayerDataUpdate += UpdatePlayerData ;  // register as as an eventHandler for the gameData.onPlayerUpdateEvent notification
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



}

```