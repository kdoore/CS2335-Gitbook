#Mini Game Manager

This script will be on emptyGameObject: MiniGameManager
You'll need to populate public fields in the inspector. See Images below: 

Create The following GameObjects:

ResultsPanel: UI-Panel with CanvasGroup Component
ResultsText: UI-Text - child of ResultsPanel
StartButton: UI-Button
MiniGameManager: Empty GameObject

![](/assets/Screen Shot 2019-02-19 at 4.10.44 PM.png) 
![](/assets/Screen Shot 2019-02-19 at 4.11.18 PM.png)

Put Script on an empty gameObject in the Scene.

![](/assets/Screen Shot 2019-02-19 at 4.11.04 PM.png)

###Overview:
This class manages logic for:
- Start Button to Start Gameplay
    - Function ReStartGame( ) is executed when the StartButton is clicked
  - Resets GameData
  - Hides the Results Panel:
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
#Simplified Code Version
The code below is a simplified version, either version should work, select the one you prefer


```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

//custom data-type to manage Finite State Machine - variable: curGamestate is the FSM's memory
public enum MiniGameState { idle, active, win, lose }

public class MiniGameManager : MonoBehaviour
{

    public MiniGameState curGameState = MiniGameState.idle;
    public Spawner spawner; //set in inspector
    public Button startButton; //set in inspector
    public Text resultText;
    public CanvasGroup resultsCG; //canvas group on results panel
    public int winScore = 30;


    // Use this for initialization
    void Start()
    {
        Utility.ShowCG(resultsCG); //show the panel and it's children
        resultText.text = "Score " + winScore + " points to win";
        startButton.onClick.AddListener(ReStartGame);
    }

    // Update is called once per frame
    void Update()
    {
        if (curGameState == MiniGameState.active)
        {
            if (GameData.instanceRef.Health > 0) //still have health
            {
                if( GameData.instanceRef.Score >= winScore)
                {
                    curGameState = MiniGameState.win;
                    resultText.text = "You Win";
                    GameOver();
                }
            }
            else //we have no health
            {
                curGameState = MiniGameState.lose;
                resultText.text = "You Lost";
                GameOver();
            }
        }
    } //end update

    private void GameOver()
    {
        Utility.ShowCG(resultsCG); //make the panel and children visible
        startButton.gameObject.SetActive(true); //set gameObject with Button Component to active
        Text btnText = startButton.GetComponentInChildren<Text>();
        btnText.text = "Play Again";
        spawner.activeSpawning = false;
        spawner.DestroyAllPickups();
    }


    /// <summary>
    /// starts game.
    /// Executed by Unity when the StartButton is clicked
    /// </summary>
    public void ReStartGame()
    {
        GameData.instanceRef.ResetGameData();
        Utility.HideCG(resultsCG);
        curGameState = MiniGameState.active; //FSM - change state
        startButton.gameObject.SetActive(false); //disables the StartButton GO
        spawner.activeSpawning = true;
        spawner.StartSpawning();

    }
}

```


