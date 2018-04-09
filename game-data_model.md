# Game-Data Model - With C# Events

**Important:**  If you are interested in a simplified version of this code, look at the [GameData with UnityEvents](/project-3/gamedata-with-unityevent.md) example.  


###C# Events
The example below uses C# events, which are a little more complicated, but provide options for custom events where data is passed to the listener when the event is invoked.  We have defined an Event Handler and an Event to notify other gameObjects when the PlayerData has been updated.



###GameData 
Below is the start of a custom class that we'll use to manage game data throughout the entire game.  It will use the singleton pattern and it'll be attached to the GameManager, so it will exist through the life of our application.

```java
using UnityEngine;
using UnityEngine.UI;
using System.Collections;
using System;  //adds EventHandler


public class GameData : MonoBehaviour {
	///we could have created our own custom delegate to pass relevent PlayerGameData
	//public delegate void OnDataUpdate( int totalScore, int health, int lives);
	//instead,we use the generic-type: C# EventHandler delegate for the event definition
	//EventHandler< T:EventArgs >, we must define the Type: T of the data that is sent with the event notification
	//We created a custom PlayerDataEventArgs class so we can use it to pass our data for this event
	//EventHandler: void EventHandler(object sender, EventArgs e);

	/// <summary>Event Notification - EventHandler Delegate
	/// Occurs when player data update event occurs. Use C# EventHandler Delegate, must include: using System;
	/// </summary>
	public event EventHandler<PlayerDataEventArgs> onPlayerDataUpdate;  //Event notification

	public static GameData instanceRef;
	private int health;
	private int totalScore;

	public int TotalScore {  //read only
		get{ return  totalScore; }
		}

	void Awake(){  
		health = 100;
		levelScore = 0;
		totalScore = 0;
		
	//create singleton
		if (instanceRef == null) {
			instanceRef = this;
		    DontDestroyOnLoad (gameObject);  //the gameObject this is attached to 
		} else {   //
			DestroyImmediate (gameObject);   
			Debug.Log ("Destroy GameObject");
		}
		
		
		// Set HighScore
		if (PlayerPrefs.HasKey ("HighScore")) {
			int priorHighScore = PlayerPrefs.GetInt ("HighScore");
			Debug.Log ("Starting High Score" + priorHighScore);
		} else {
			PlayerPrefs.SetInt ("HighScore", 0);
		}
	}
	// Use this for initialization
	void Start () {
		//move initialization to Awake
		}

	////Called in Player controller when the player collides with a pickup	
	public void Add(PickUp item){
		
		totalScore += item.value;  // update totalScore by the value of this current item

		checkResetHighScore ();  //should we update PlayerPrefs, is this the alltime high score?

		///PlayerDataEventArgs is the data object to send as part
		/// of the onPlayerDataEvent event notification
		/// Create an instance of PlayerDataEventArgs, have it's data elements
		/// populated using method: GetPlayerDataEventArgs, pass in the object reference.

		PlayerDataEventArgs updatedPlayerData = new PlayerDataEventArgs();
		GetPlayerDataEventArgs(updatedPlayerData);  //creating instance

		//PlayerDataEvent has already happened, now we need to send notification
		//check to see if there are any registered eventHandlers
		if(onPlayerDataUpdate != null){
			//data update event notification is involked
			// all registered eventHandlers get notification and updatedPlayerData
			onPlayerDataUpdate ( this , updatedPlayerData  ); 	
			}
	} // end Add()

	/// <summary>
	/// Function to check high score in PlayerPrefs and update with the current high score if necessary
	/// </summary>
	private void checkResetHighScore(){
		int curHighScore = PlayerPrefs.GetInt ("HighScore");
		if (TotalScore > curHighScore) {
			PlayerPrefs.SetInt ("HighScore", TotalScore);
		}
	} // end checkReset

	/// <summary>
	/// Gets the player data event arguments.
	/// </summary>
	/// <returns>The player data event arguments.</returns>
	public void GetPlayerDataEventArgs(PlayerDataEventArgs playerData){
		playerData.totalScore = this.totalScore;
		playerData.health = this.health;
		playerData.lives = this.lives;
	
	}

} // end class: GameData - this class functions as our Data-Model
```
