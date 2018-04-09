### GameData - Simple

In the game we'll need an object that is persisted throughout the gameplay session so that we can keep track of score, lives, health, etc.  

Below is a simple class definition for GameData, we'll add more to this as we progress.

This script will be attached to the GameManager in the BeginScene and will be persisted throughout the game since it uses the singleton design pattern.

To use the singleton reference in another class use the following syntax, for example, when calling the Add method from the PlayerController script:

// in PlayerController.cs

GameData.instanceRef.Add( );


###GameData Class Definition

```java


using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

//Class to create a singleton object
//this will be attached to the GameManager in BeginScene and
//persisted throughout the game
//This object will maintain the important data about the gameplay experience
public class GameData : MonoBehaviour
{

	public static GameData instanceRef;  //singleton reference variable
	
	private int health;
	private int totalScore;
	private int levelScore;

	///Properties
	public int TotalScore {  //read only
		get{ return  totalScore; }
	}
	
	public int Health {  //read only
		get{ return  health; }
	}
	
	public int LevelScore {  //read only
		get{ return  levelScore; }
		set{  levelScore= value; }

	}


	void Awake ()
	{  //create singleton
		if (instanceRef == null) {
			instanceRef = this;
			DontDestroyOnLoad (gameObject);  //the gameObject this is attached to 
		} else {   //
			DestroyImmediate (gameObject);   
			Debug.Log ("Destroy GameObject");
		}
		
		health = 100;
		totalScore = 0;
		levelScore = 0;


		// Set HighScore using Unity PlayerPrefs dictionary
		if (PlayerPrefs.HasKey ("HighScore")) {
			int priorHighScore = PlayerPrefs.GetInt ("HighScore");
			Debug.Log ("Starting High Score" + priorHighScore);
		} else {
			PlayerPrefs.SetInt ("HighScore", 0);
		}
	}
	// Use this for initialization
	void Start ()
	{
		//move to Awake
	}

	////Called in Player controller when the player collides with a pickup    
	public void Add (PickUp item)
	{
		totalScore += item.value;  // update totalScore by the value of this current item
		Debug.Log ("Adding item value to totalScore, totalScore =  " + totalScore);
		checkResetHighScore ();  //should we update PlayerPrefs, is this the alltime high score?

	}
	// end Add()

	public void TakeDamage(int damage){        health -= damage;        if(health < 0){            Debug.Log("GameOver due to low health");        }    }
	/// <summary>
	/// Function to check high score in PlayerPrefs and update with the current high score if necessary
	/// </summary>
	private void checkResetHighScore ()
	{
		int curHighScore = PlayerPrefs.GetInt ("HighScore");
		if (TotalScore > curHighScore) {
			PlayerPrefs.SetInt ("HighScore", TotalScore);
		}
	}
	// end checkReset


}



```