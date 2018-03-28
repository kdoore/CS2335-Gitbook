#GameData with UnityEvent

This example uses simple UnityEvents to notify any Listeners that 

```java


using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

public class GameData : MonoBehaviour {


public static GameData instanceRef; //singleton reference variable
    private int health;
    private int lives;
    private int totalScore;

    public UnityEvent onPlayerDataUpdate;

    public int TotalScore
    { //read only
        get { return totalScore; }
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


        // Set HighScore using Unity PlayerPrefs dictionary
        if (PlayerPrefs.HasKey("HighScore"))
        {
            totalScore = PlayerPrefs.GetInt("HighScore");
            Debug.Log("Starting High Score");
        }
        else
        {
            PlayerPrefs.SetInt("HighScore", 0);
        }

        if(onPlayerDataUpdate == null){
            onPlayerDataUpdate = new UnityEvent();
        }
    }
    // Use this for initialization
    void Start()
    {
        health = 100;
        lives = 5;
        totalScore = 0;
    }


    ////Called in Player controller when the player collides with a pickup
    public void Add(PickUp item)
    {
        totalScore += item.value; // update totalScore by the value of this current item
        Debug.Log("Adding item value to totalScore, totalScore = " + totalScore);
        checkResetHighScore(); //should we update PlayerPrefs, is this the alltime high score?

        if(onPlayerDataUpdate != null){
            onPlayerDataUpdate.Invoke();
        }
    } // end Add()
     
    public void TakeDamage(int damage){
        health -= damage;
        if(health < 0){
            Debug.Log("GameOver due to low health");
        }

        if (onPlayerDataUpdate != null)
        {
            onPlayerDataUpdate.Invoke();
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

