# Level Manager 

### Level Manager Overview

In the code below, the Level Manager is responsible for controlling the Mini\_Game Level Logic, this includes determining when to change levels and what gameObjects are impacted when the level changes.  The LevelManager Custom Script is attached to the LevelManager game object in the Mini-Game scene and provides Finite State Machine logic to determine the current level and state of dependent gameObjects within the Mini-game.

### Inter-Dependent Game Objects

Game objects that are inter-dependent with the LevelManager are:

1. Splash Screen - UI Panel with StartMiniGame Button - is hidden when StartGameButton is clicked.
2. Background Image - changed with each level-change
3. Spawner - start spawning when splash screen start-button is clicked
4. Spawner - spawn different prefabs depending on the current level
5. UI Elements - Level and Score UI displays must be updated to reflect the current Level and the current Score value.
6. PlayerController - this GameObject will sense collisions with PickUps, this will change the score and the level
7. GameData - create a new variable levelScore, and a new property: LevelScore, these are changed in GameData when the score is updated.


#LevelManager Class
Start by adding the following code to the top of the LevelManager Class

```java
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;
using UnityEngine.SceneManagement;

public class LevelManager : MonoBehaviour {

        ///CODE TO BE ADDED HERE

} //end class

```



### Level-State Enums
The next code we'll add to the LevelManager class is a set of Enums that are used to track which state we are in.  We'll create a LevelState variable: curLevel, and it's value will change as the Level changes.  This corresponds to a simple Finite State Machine for managing levels.

```java
public enum LevelState
{
    start,
    level1,
    level2,
    level3,
    win,
    lose

}
```

### Declare LevelManager Object Reference Variables
After declaring the LevelState enums, then we'll declare the LevelManager's object reference variables.

```java
   LevelState curLevel; //track current level for FSM
   
   //levelScore is stored in GameData
   int maxLevelScore; //when to change levels
    
    //UI game Objects - LevelText, StartGameButton, StartGamePanel
    Button startGameButton;
    CanvasGroup cg;
    Text levelText;

    //references to custom script components
    Spawner spawner;
    //to start the spawner, change objects that are spawned
```

### Start - Initialize Object References
Add the following code to the Start( ) method.

```java
// Use this for initialization
   void Start()
    {
        curLevel = LevelState.start;
       
        maxLevelScore = 30;  //ADJUST FOR YOUR GAME
        startGameButton = GameObject.Find("StartGameButton").GetComponent<Button>();
        startGameButton.onClick.AddListener(NextLevel);

        levelText = GameObject.Find("LevelText").GetComponent<Text>();
        levelText.text = "";

        cg = GameObject.Find("StartGamePanel").GetComponent<CanvasGroup>();
        Utility.ShowCG(cg);  //show StartButton

        spawner = GameObject.Find("Spawner").GetComponent<Spawner>();

    ///REGISTER AS A LISTENER TO GameData event: OnPlayerDataUpdate
    GameData.instanceRef.OnPlayerDataUpdate.AddListener(CheckLevelEnd);
    }
    
    
```

### Check For Level End
Write code for the CheckLevelEnd method, this method will be executed every time the OnPlayerDataUpdate event occurs in GameData.

```java
     /// <summary>
    /// Checks the level end.
    ///   If LevelScore > maxLevelScore, then reset the LevelScore and call the nextLevel( ) method
    ///  to change the level
    /// </summary>
    ///this will be called when the OnPlayerDataUpdate event 
    ///happens in GameData, it is registered as a listener for that event

    public void CheckLevelEnd ( )
    {
        int levelScore = GameData.instanceRef.LevelScore;
        Debug.Log ("Check if level is over" + levelScore);

        if (levelScore >= levelMaxScore) { ///level has changed
         
            GameData.instanceRef.LevelScore= 0;   //reset GameData.LevelScore

            NextLevel ();  //go to next level
        }

    }
```

### NextLevel - Finite State Machine Logic
This method manages the FSM control logic using switch-case structure. Each time this method is called, the matching logic must change the value of curLevel, and call a custom method: loadLevelX( ) where the details of the level loading logic are specified.

```java


public void NextLevel()
    {

        switch (curLevel)
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
    }```

### LoadLevel Methods

For each level change, a method: loadLevelX( )contains the level change logic. You'll need to add additional LoadLevel Methods for 
loadLevel3( ), etc.

```java
void loadLevel1 ()
    {
        //STARTS Gameplay, Spawner, etc
        
        Utility.HideCG (cg); //hide the StartGamePanel and StartGameButton
        spawner.StartSpawning ();
        levelText.text = "Level 1";
    }

    void loadLevel2 ()
    {
        ///change background image?
        ///change objects getting spawned?
        
        levelText.text = "Level 2";
    }
    
    void loadLevel3 ()
    {
        ///change background image?
        ///change objects getting spawned?
    
        levelText.text = "Level 3";
    
    }

    void miniGameOver( ){
    //make sure to include: Using UnityEngine.SceneManagement at top of script
    //this will change Scene/State
        SceneManager.LoadScene ("EndScene");  //actual scene name
        StateManager.instanceRef.SwitchState (new EndState ());  //create new state, pass to StateManager     
    }
```



