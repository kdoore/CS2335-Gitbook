#Mini Game Manager

###Overview:
This class manages logic for:
- Start Button to Start Gameplay
    - Function ReStartGame( ) is executed when the StartButton is clicked
  - Resets GameData
  - Hides the Results Panel that contains win-lose Text
  - Sets the GameState to MiniGameState.active
  - Sets the StartButton so it's inactive (hidden)
  - Starts the Spawner 
      - Set the Spawner's activeSpawning = true
      - Calls Spawner's StartSpawning( ) method.
      
  
- Polling - Code Executed in Update: - Checked Every Frame
     - Checks to determine what the current MiniGameState is
     - if MiniGameState.active
         - Check that Health is still greater than 0
         - If Health is Greater than 0
             - Check Score, if Score > WinScore
                 - Set MiniGameState.Win
                 - Call GameOver( )
         - Else if Health is Less than 0
                 - Set MiniGameState.Lose
                - Call GameOver( )
             
- GameOver( ) Method  
    - Set Result Text to Win or Lose message
    - Make ResultsPanel visible ( Use Utility to set CanvasGroup values) 
    -  StartButton - setActive is true, makes visible 
    -  Stop Spawner by activeSpawning = false
    -  Destroy any remaining spawned objects
    
    
- DisplayResult - This method is called when the Game



#Code MiniGameState, MiniGameManager

```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public enum MiniGameState { idle, active, win, lose } //accessible globally

public class MiniGameManager : MonoBehaviour {

    public MiniGameState curGameState = MiniGameState.idle;
    public Spawner spawner; //component on spawner GameObject
    public Button startButton;
    public CanvasGroup  resultsCG; //let's panel be hidden
    public Text resultText;
    public float WinScore = 20; //determines when player wins
	
    // Use this for initialization
	void Start () {
        resultText.text = "Press to Start, Score 20 points to Win";
        startButton.onClick.AddListener(ReStartGame); //call method when clicked
	}
	
	// Update is called once per frame
	void Update () {
        if (curGameState == MiniGameState.active) //only run if active game
        {
            if (GameData.instanceRef.Health > 0) //still alive
            {
                if (GameData.instanceRef.Score >= WinScore) //win condition
                {
                    curGameState = MiniGameState.win; //change state
                    GameOver();
                }
            }
            else
            { //Health <= 0
                curGameState = MiniGameState.lose; //change state
                GameOver(); //lose condition
            }
        }
    }

    public void ReStartGame(){
        GameData.instanceRef.ResetGameData();
        Utility.HideCG(resultsCG); //hide info panel
        curGameState = MiniGameState.active;
        startButton.gameObject.SetActive(false);
        spawner.activeSpawning = true;
        spawner.StartSpawning();
    }


    public void DisplayResult()
    {
        Utility.ShowCG(resultsCG);
        startButton.gameObject.SetActive(true);
         //get a reference to the Button's child Text element
        Text btnTxt = startButton.GetComponentInChildren<Text>();
        btnTxt.text = "Play Again"; //change button text
        
        
    }


    public void GameOver(){
        if (curGameState == MiniGameState.lose)
        {
            resultText.text = "You Lose";
        }
        if (curGameState == MiniGameState.win){
            resultText.text = "You Win";
        }
        DisplayResult();
        spawner.activeSpawning = false;
        spawner.DestroySpawnedObjects();
    }
}// end class

```

