#Level Manager


See code in: [LevelManager-Events](/level-manager-in-class.md)

###GameData -- new code must be incorporated -
You will need to add some new code to GameData, this is specified in the LevelManager-Events documentation.  It may be easier to simply replace all code in GameData with the code linked below.

These are changes that should be incorporated into GameData prior to working with the Level Manager
    - levelScore variable
    - onPlayerDataUpdate, UnityEvent
    - inventory - Dictionary<PickupType, int>
    - LevelScore, Health properties
    - Update Add( )
        - increment levelScore with points
        - invoke onPlayerDataUpdate
    - Update TakeDamage( )
        - invoke onPlayerDataUpdate


[See updated version of GameData](/project-3/gamedata-with-unityevent.md)


###LevelManager Code 
You will need to modify this code to match your game's logic


```java

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;
using UnityEngine.SceneManagement;

public class LevelManager : MonoBehaviour {

    public enum LevelState
    {
        start,
        level1,
        level2,
        level3
    }

    LevelState curLevel;

    // FSM - 1 unit of memory
   
    int maxLevelScore; //when to change levels
    //UI game Objects - LevelValue, StartGameButton, StartGamePanel
    Button startGameButton;
    CanvasGroup cg;
    Text levelText;

    //references to custom script components
    Spawner spawner;
    //to start the spawner, change objects that are spawned


    void Start()
    {
        curLevel = LevelState.start;
        maxLevelScore = 30;
        startGameButton = GameObject.Find("StartGameButton").GetComponent<Button>();
        startGameButton.onClick.AddListener(NextLevel);

        levelText = GameObject.Find("LevelText").GetComponent<Text>();

        cg = GameObject.Find("StartGamePanel").GetComponent<CanvasGroup>();
        Utility.ShowCG(cg);

        spawner = GameObject.Find("Spawner").GetComponent<Spawner>();

        ///Update Check to see if level is over when playerDataUpdate event happens
        GameData.instanceRef.onPlayerDataUpdate.AddListener(CheckLevelEnd);
    }

    public void CheckLevelEnd()
    {
        int levelScore = GameData.instanceRef.LevelScore;
        //Debug.Log("Check if level is over" + levelScore);

        //Add code here to see if health is <=0
        

        //if health is ok, then check if level is over
        if (levelScore >= maxLevelScore)
        { ///level has changed
          ///reset level value display
            GameData.instanceRef.LevelScore = 0;   //reset GameData.LevelScore
        NextLevel();  //go to next level
        }

    }

    //This method implements the Finite State Machine to Manage Level Logic. 
    //You will modify this code to correspond to your game's logic
    public void NextLevel()
    {

        switch (curLevel)  //check the curLevel, find matching case below
        {

            case LevelState.start:  // called when StartPanel, StartGameButton is clicked
                curLevel = LevelState.level1; //change level
                loadLevel1();
                break;

            case LevelState.level1:  //called when in Level1 from checkLevelEnd( ) 
                curLevel = LevelState.level2; //change level

                loadLevel2();
                break;
            case LevelState.level2: //called when in Level2 from checkLevelEnd( ) 
                curLevel = LevelState.level3; //change level

                loadLevel3();
                break;
            case LevelState.level3: //called when in Level3 from checkLevelEnd( ) 
                //ADD logic to determine if it's a winning or losing ending
                
                miniGameOver();
                break;
            
            default:
                Debug.Log("No match on curLevel");
                break;
        }
    }

////YOU WILL MODIFY THESE METHODS SO THEY CORRESPOND TO YOUR GAME"S LOGIC
    void loadLevel1()
    {
        //STARTS Gameplay, Spawner, etc

        Utility.HideCG(cg); //hide the StartGamePanel and StartGameButton
        spawner.StartSpawning();  //Make sure to remove this code from Start in the spawner script
        levelText.text = "Level 1";
    }

    void loadLevel2()
    {
        ///change background image?
        ///change objects getting spawned?
         
        levelText.text = "Level 2";
    }

    void loadLevel3()
    {
        ///change background image?
        ///change objects getting spawned?

        levelText.text = "Level 3";

    }

    void miniGameOver()
    {
        //make sure to include: Using UnityEngine.SceneManagement at top of script
        //this will change Scene/State
        ///ADD Additional logic to determine which scene to go to 
        /// based on win or lose condition
        SceneManager.LoadScene("EndScene");  //actual scene name
        StateManager.instanceRef.SwitchState(new EndState());  //create new state, pass to StateManager     
    }

	
}

```

