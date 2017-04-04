# Level Manager - In Class

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

### Level-State Enums

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

### Declare Object References

```java
   LevelState curLevel;
    // FSM - 1 unit of memory
    int levelScore;

    //UI game Objects - LevelValue, StartGameButton, StartGamePanel
    Button startGameButton;
    CanvasGroup cg;
    Text levelValue;

    //references to custom script components
    Spawner spawner;
    //to start the spawner, change objects that are spawned
    ChangeSprite changeSprite;
    //to change background sprite
```

### Start - Initialize Object References

```java
// Use this for initialization
    void Start ()
    {
        curLevel = LevelState.start;
        levelScore = 0;   // initialize

        startGameButton = GameObject.Find ("StartGameButton").GetComponent<Button> ();
        startGameButton.onClick.AddListener (nextLevel);
        levelValue = GameObject.Find ("LevelValue").GetComponent<Text> ();

        cg = GameObject.Find ("StartGamePanel").GetComponent<CanvasGroup> ();
        Utility.ShowCG (cg);

        spawner = GameObject.Find ("Spawner").GetComponent<Spawner> ();
        changeSprite = GameObject.Find ("BackgroundImage").GetComponent<ChangeSprite> ();
    }
```

### Check For Level End

```java
     /// <summary>
    /// Checks the level end.
    /// Pass in PickUp item so levelScore can be updated using
    /// item.value.  If levelScore > 10, then the nextLevel( ) method
    /// is executed to change the level
    /// </summary>
    /// <param name="item">Item.</param>
    ///this will be called in the PlayerController whenever the score changes - OnTriggerEnter2D()
    public void CheckLevelEnd (PickUp item)
    {
        levelScore += item.value;
        Debug.Log ("Check if level is over" + levelScore);

        if (levelScore > 10) { ///level has changed
            ///reset level value display
            levelScore = 0;   //reset level score

            nextLevel ();  //go to next level
        }

    }
```

### NextLevel - Finite State Machine Logic

```java
///This method manages the FSM control logic using switch-case structure. 
///Each time this method is called, the matching logic must change the value of curLevel, 
///and call a custom method: loadLevelX( ) where the details of the level loading logic are specified.

public void nextLevel ()
    {

        switch (curLevel) {

        case LevelState.start:  // called when StartPanel, StartGameButton is clicked
            curLevel = LevelState.level1;
            loadLevel1 ();
            break;

        case LevelState.level1:  //called when in Level1 from checkLevelEnd( ) 
            curLevel = LevelState.level2;
            loadLevel2 ();
            break;
        case LevelState.level2: //called when in Level2 from checkLevelEnd( ) 
            curLevel=LevelState.level3;
            loadLevel3();
            break;
        case: LevelState.level3: //called when in Level3 from checkLevelEnd( ) 
            miniGameOver();
        default:
            Debug.Log ("No match on curLevel");
            break;
        }
    }
```

### LoadLevel Methods

For each level change, a method: loadLevelX\( \) contains the level change logic.

```java
void loadLevel1 ()
    {
        Utility.HideCG (cg); //hide the StartGamePanel and StartGameButton
        spawner.StartSpawning ();
        levelValue.text = "1";

    }

    void loadLevel2 ()
    {
        ///change background image
        changeSprite.SwapSprite ();
        levelValue.text = "2";
    }

    void miniGameOver( ){
    //make sure to include: Using UnityEngine.SceneManagement at top of script
    //this will change Scene/State
        SceneManager.LoadScene ("EndScene");  //actual scene name
        StateManager.instanceRef.SwitchState (new EndState ());  //create new state, pass to StateManager     
    }
```



