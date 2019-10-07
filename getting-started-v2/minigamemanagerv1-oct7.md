###MiniGameManager_v1 Oct 7, 2019

This is the code covered Oct 7th.  See [link ](/minigamemanager.md)for screenshots of gameObject hierarchy required to use this script.

This script is placed on the MiniGameManager(Empty GameObject)



```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

//possible game states - Finite State Machine
public enum MiniGameState { idle, active, win, lose}

public class MiniGameManager : MonoBehaviour
{
    MiniGameState curGameState = MiniGameState.idle; //start state is idle

    public Button startButton; //set in the inspector
    public Text resultText;  //set connection in the inspector
    public CanvasGroup resultsCG;


    // Start is called before the first frame update
    void Start()
    {
        Utility.ShowCG(resultsCG);
        resultText.text = " Score points to win";
        //when the startButton onClick event is exectued
        //  for StartGame method to be executed, do this by setting the 
        // StartGame method as a AddListener to the onClick event
        startButton.onClick.AddListener(  StartGame  );

    }

    // Update is called once per frame
    void Update()
    {
        if( curGameState == MiniGameState.active)
        {
            Debug.Log("Active Game ");
        }

    }

    //method want to execute when StartButton is clicked
    public void StartGame(   )  //public void, no input parameters
    {
        Debug.Log("Start the game");
        Utility.HideCG(resultsCG);  //hide when game starts

        //change the gameState to active
        curGameState = MiniGameState.active;
    }
}//end class

```

