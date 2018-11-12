### GameData - Simple

In the game we'll need an object that is persisted throughout the gameplay session so that we can keep track of score, lives, health, etc.

Below is a simple class definition for GameData, we'll add more to this as we progress.

This script will be attached to the GameManager in the BeginScene and will be persisted throughout the game since it uses the singleton design pattern.

Add GameData script to the GameManager GameObject in your starting scene, so it will be executed as a singleton and shows up as 'DontDestroyOnLoad' in the Hierarchy when the game is played.

**Add to GameData to GameManager in Start Scene**
![](/assets/Screen Shot 2018-11-08 at 12.33.45 PM.png)

**DontDestroyOnLoad - in Play-mode**
![](/assets/Screen Shot 2018-11-08 at 12.35.36 PM.png)

**MiniGame-DataManager, Game Data without StateManager for Game-play testing**

When designing the mini-game scene, it is helpful to have a GameManager gameObject in the hierarchy that has the just the GameData script attached, but does not have the StateManager script attached. Otherwise, when testing the mini-game scene, you'll need to start from the BeginScene in order to prevent errors from trying to access GameData in the miniGame.

**MiniGame Scene - DataManager**
![](/assets/Screen Shot 2018-11-08 at 12.29.37 PM.png)

**How to use GameData Singleton**
To use the singleton reference in another class use the following syntax, for example, when calling the Add method from the PlayerController script:

_Example of using GameData singleton in PlayerController.cs_
```java
GameData.instanceRef.Add( item );
```


### GameData Class Definition

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

//Class to create a singleton object
//this will be attached to the GameManager in BeginScene and
//persisted throughout the game
//This object will maintain the important data about the gameplay experience
public class GameData : MonoBehaviour
{

    public static GameData instanceRef;  //singleton reference variable

    private int health;
    private int totalScore;
    private int lives;

    ///Properties
    public int TotalScore {  //read only
        get{ return  totalScore; }
    }

    public int Health {  //read only
        get{ return  health; }
    }

    public int Lives {  //read 
        get{ return  lives; }
    }

    void Awake (){
        //initialize variables
        health = 100;
        totalScore = 0;
        lives = 3;  

      //create singleton
        if (instanceRef == null) {
            instanceRef = this;
            DontDestroyOnLoad (gameObject);  //the gameObject this is attached to 
        } else {   //
            DestroyImmediate (gameObject);   
            Debug.Log ("Destroy GameObject");
        }

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
        //moved code to Awake
    }

    ////Called in Player controller when the player collides with a pickup    
    public void Add (PickUp item)
    {
        totalScore += item.value;  // update totalScore by the value of this current item
        Debug.Log ("Adding item value to totalScore, totalScore =  " + totalScore);
   
    }
    // end Add()

    public void TakeDamage(int damage){
        health -= damage;
        if(health < 0){
            Debug.Log("GameOver due to low health");
        }
    }
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



