### GameData - Version 1

In the game we'll need an object that is persisted throughout the gameplay session so that we can keep track of score, lives, health, etc.

Below is a simple class definition for GameData, we'll add more to this as we progress.

This script will be attached to the GameManager in the first game Scene and will be persisted throughout the game since it uses the singleton design pattern.

**Add GameData script to an Empty GameObject:** named: **GameManager** in your starting scene, so it will be executed as a singleton and shows up as 'DontDestroyOnLoad' in the Hierarchy when the game is played.

**DontDestroyOnLoad - in Play-mode**
![](/assets/Screen Shot 2018-11-08 at 12.35.36 PM.png)

**How to use GameData Singleton**
To use the singleton reference in another class use the following syntax, for example, when calling the Add method from the PlayerController script:

_Example of using GameData singleton in PlayerController.cs_
```java
GameData.instanceRef.Add( item.value );
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
    private int score;
    
    ///Properties
    public int  Score {  //read only
        get{ return  totalScore; }
    }

    public int Health {  //read only
        get{ return  health; }
    }

    void Awake (){
        //initialize variables
        health = 100;
        score = 0;     

    //create singleton
        if (instanceRef == null) { //has never been initialized before
            instanceRef = this;
            DontDestroyOnLoad (gameObject);  //the gameObject this is attached to 
        } else {   //
            DestroyImmediate (gameObject);   
            Debug.Log ("Destroy Duplicate GameData");
        }

    // Use this for initialization
    void Start ()
    {
        //moved code to Awake
    }

    ////Called in Player controller when the player collides with a pickup    
    ///this will be modified later so the input parameter is a PickUp item so it can be added to inventory
    public void Add (int value)
    {
        score += value;  // update score by the value of this current item
        Debug.Log ("Adding item value to score, score =  " + score);
   
    }
    // end Add()

    public void TakeDamage(int damage){
        health -= damage;
        if(health < 0){
            Debug.Log("GameOver due to low health");
        }
    }
   
    //Add code for: ResetGameData, see below 
        
} //end GameData version1
```

###Update:  ResetGameData() - Resets Score and Health to correct initial values.  Public, so it can be executed from MiniGameManager Script.


 public void ResetGameData()
    {
        health = 100; //initialize
        score = 0;
    } 
