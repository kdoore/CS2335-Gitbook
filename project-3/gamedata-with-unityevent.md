#GameData with UnityEvent

This version of the GameData script uses a custom UnityEvent to notify listeners that some change happened to the player's data.

**UnityEvent**
This example uses a simple UnityEvent: `onPlayerDataUpdate,` to notify any Listeners that the playerData has been updated.  Potential Listener objects include the LevelManager script, the PlayerStatsDisplay, and the InventoryDisplay.  Make sure to add:  using `UnityEngine.Events;`, to the top of your script

**GameObject:**  GameData should be added to the GameManager, empty gameObject, in the BeginScene, and in the MiniGame scene for easy testing of the game.

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;  //ADD THIS

///Singleton Class - Accessible in all scenes
///Manages all data for the game
public class GameData : MonoBehaviour {
    public static GameData instanceRef; //singleton reference variable
    
    //UNITY EVENT: onPlayerDataUpdate
    public UnityEvent onPlayerDataUpdate;

    private int health;
    private int totalScore;
    
    //ADD CODE BELOW if using LevelManager
    private int levelScore;
    
    ///Properties
    public int LevelScore{
        get{ return levelScore };
        set{ levelScore = value };
    }
    

    public int TotalScore  //public property 
    { //read only
        get { return totalScore; }
    }
    
    public int Health //public property 
    { //read only
        get { return health; }
    }



    void Awake()
    { //create singleton 
        if (instanceRef == null)
        {
            instanceRef = this;
            DontDestroyOnLoad(gameObject); //the gameObject this is attached to
            
        }
        else
        { //
            DestroyImmediate(gameObject);
            Debug.Log("Destroy GameObject");
        }
        
        //initialize the UnityEvent by calling the constructor.
        if(onPlayerDataUpdate == null){
        onPlayerDataUpdate = new UnityEvent();
        }
        
        ///ADD THIS CODE TO AWAKE
        health=0;
        levelScore = 0;
        totalScore = 0;

        ///////UPDATE THIS CODE - IT HAD AN ERROR
        // Get / Set HighScore using Unity PlayerPrefs dictionary
        if (PlayerPrefs.HasKey("HighScore"))
        {
            ////REMOVE or COMMENT-OUT the line of code below
           /// totalScore = PlayerPrefs.GetInt("HighScore"); 
            Debug.Log("Initial High Score:" + PlayerPrefs.GetInt("HighScore"));
        }
        else //"HighScore" has never been created, so create it and give initial value of 0.
        {
            PlayerPrefs.SetInt("HighScore", 0);
        }
    } // end Awake
    
    
    
    // Use this for initialization
    void Start()
    {
      ////REMOVE INITIALIZATION CODE, MOVE TO AWAKE
    }


    ////Called in Player controller when the player collides with a pickup
    public void Add(PickUp item)
    {
        totalScore += item.value; // update totalScore by the value of this current item
        Debug.Log("Adding item value to totalScore, totalScore = " + totalScore);
        checkResetHighScore(); //should we update PlayerPrefs, is this the alltime high score?

        if(onPlayerDataUpdate != null){  // some object is listening
            onPlayerDataUpdate.Invoke(); //broadcast the event
        }
    } // end Add()
     
     //Called from PlayerController when the player has 
     //collided with a Hazard, Damage is a negative value, so we should just add it to health.
    public void TakeDamage(int damage){ 
        health += damage;
        Debug.Log("TakeDamage: Health " + health);
        if(health < 0){
            Debug.Log("GameOver due to low health");
        }

        if (onPlayerDataUpdate != null){// some object is listening
            onPlayerDataUpdate.Invoke();//broadcast the event
            }
    }

    /// <summary>
    /// Function to check high score in PlayerPrefs and update with the current high score if necessary
    /// </summary>
  private void checkResetHighScore()
    {
        int curHighScore = PlayerPrefs.GetInt("HighScore");
        if (TotalScore > curHighScore)
        {
            PlayerPrefs.SetInt("HighScore", TotalScore);
        }
    }
    // end checkReset

}




```

